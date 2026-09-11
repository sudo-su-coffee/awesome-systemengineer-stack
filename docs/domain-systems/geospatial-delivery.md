# On-Demand & Geospatial Architecture (Uber/Zomato)

If you are building an app that moves physical things in real-time (like **Uber, Zomato, or Blinkit**), you are solving complex mathematical routing problems on a massive scale.

## 1. Real-Time Geospatial Indexing
You have 10,000 delivery drivers moving around the city. When a user orders food, how do you find the closest driver in under 1 second?
- You cannot loop through 10,000 GPS coordinates in Postgres.
- We use **Redis GEO Indexing** or **PostGIS**.
- The Drivers' phones ping our Go API with their GPS coordinates every 5 seconds. Go updates their location in Redis.
- When an order is placed, Go queries Redis: `GEORADIUS drivers 77.59 12.97 2 km`. Redis instantly returns the 5 drivers within a 2km radius using a mathematical grid system (Geohashes).

## 2. The Polling vs WebSockets Debate
How do you show the little car moving smoothly on the user's map?
- **The Wrong Way**: The Flutter app asks the server "Where is the car?" every 1 second (HTTP Polling). With 1 Million users, your server crashes from billions of empty requests.
- **The Right Way**: The Go backend holds an open **WebSocket** connection. The server silently pushes the new GPS coordinate to the Flutter app *only* when the driver actually moves.
- **The Battery Saver**: If the user backgrounds the app, the WebSocket closes, and the server switches to sending Silent FCM Push Notifications to conserve battery.

## 3. ETA Calculation (Traveling Salesman Problem)
Calculating "Delivery in 15 mins" is not a simple straight-line calculation.
- Our Go backend integrates with **Google Maps Directions API** or an open-source router like **OSRM**.
- It calculates traffic density, the time it takes the restaurant to cook the food, and the driver's historical average speed to provide an accurate, ML-driven ETA.
