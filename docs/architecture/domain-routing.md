# Multi-Tenant Domain Routing (The Vercel Model)

When scaling a SaaS or multi-vendor platform, creating individual DNS records or dedicated servers for every single vendor/domain is practically impossible to manage. Platforms like Vercel, Shopify, and Hashnode use **Domain Mapping** combined with **Host Header Routing** instead of relying entirely on complex DNS rules.

Here is exactly how this architecture works, and how it can be replicated in a Go backend.

## 1. The Wildcard DNS (The Single IP)
Instead of provisioning new IPs for new tenants, the platform uses a single **Anycast IP Address** (e.g., `76.76.21.21`). 
Every single customer is instructed to point their custom domain's DNS A-record to this one exact IP address.
- `vendorA.com` -> `76.76.21.21`
- `vendorB.com` -> `76.76.21.21`

This means the DNS layer simply acts as a funnel, routing everyone globally to the front door of your server cluster.

## 2. The Internal System (Host Header Inspection)
When a user opens their browser and navigates to `vendorA.com`, their browser sends an HTTP request to `76.76.21.21` that looks like this:

```http
GET / HTTP/1.1
Host: vendorA.com
User-Agent: Mozilla/5.0...
```

When the platform's edge server (like Nginx, Envoy, or our Go Fastglue backend) receives this request, it ignores the IP and inspects the `Host:` header. 
It takes the string `vendorA.com`, instantly queries a super-fast internal database (like Redis), and asks: *"Which project/tenant does this domain belong to?"*

## 3. The Reverse Proxy / Handler
Once Redis replies saying *"That domain belongs to Vendor A (Tenant ID: 994)"*, the system invisibly scopes all database queries and traffic internally to Vendor A's data.

---

## Replicating this in Go (Zerodha Fastglue)

Because we are using the high-performance **Zerodha Fastglue (`fasthttp`)** stack, building a Vercel-like domain mapper natively in Go is incredibly easy and ultra-fast. We can have thousands of custom domains hit our single backend IP, and handle the routing in microseconds.

### Go Implementation Example

```go
package handlers

import (
	"context"
	"github.com/zerodha/fastglue"
)

// CustomDomainRouter acts as an edge router mapping custom domains to internal tenants
func (a *App) CustomDomainRouter(r *fastglue.Request) error {
	// 1. Get the domain the user typed into the browser
	domain := string(r.RequestCtx.Host())
	
	// 2. Ask Redis which Vendor/Tenant owns this domain
	vendorID, err := a.Redis.Get(context.Background(), "domain_map:"+domain).Result()
	
	if err != nil {
		// Domain is not registered in our system
		return r.SendJSON(404, map[string]string{
			"error": "Domain not registered or incorrectly configured",
		})
	}
	
	// 3. Serve the specific data, scoped to that vendor ID!
	// (e.g. Fetching Vendor A's products from PostgreSQL)
	return r.SendJSON(200, map[string]interface{}{
		"message":   "Welcome to " + domain,
		"vendor_id": vendorID,
		"status":    "success",
	})
}
```

### Why is this better?
- **Zero DNS Propagation Delays**: Once a user points their domain to your IP, you just add a Redis key. It works instantly.
- **Infinite Scalability**: You manage domains purely in a database, not in DNS zone files.
- **SSL Certificates**: You can use tools like Caddy or Let's Encrypt On-Demand TLS to generate SSL certificates on the fly the first time a valid domain hits your server.
