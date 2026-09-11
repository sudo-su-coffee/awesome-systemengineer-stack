# Authentication Architecture: OTPless & Fallbacks

User authentication must be frictionless. Every second spent waiting for an OTP is a lost customer.

## 1. The Primary Strategy: OTPless (WhatsApp Auth)
We will use **OTPless** as the primary authentication method.
- **The Flow**: The user clicks "Login with WhatsApp". The app opens WhatsApp, the user sends a pre-filled message, and they are instantly verified.
- **The Benefit**: It eliminates the cost of SMS (which can be ₹0.15 - ₹0.20 per SMS in India) and verifies the user's phone number natively. 100% conversion rate for users who have WhatsApp installed.

## 2. The Fallback Strategy (Backup Plans)
What happens if the OTPless API is down, or the user doesn't have WhatsApp? We must have a bulletproof backup plan built into the Flutter app and Go backend.

### Fallback A: Native Google Sign-In (One Tap)
If OTPless fails, the Flutter app instantly displays the native **Google One-Tap Sign-In** bottom sheet.
- **How it works**: The user's Android phone automatically pops up their primary Gmail account. They click it once, and they are logged in.
- **Backend Sync**: The Flutter app sends the Google `idToken` to our Go backend. Go verifies the token with Google's public keys, extracts the email, and creates a user in PostgreSQL.

### Fallback B: Traditional SMS OTP (Firebase Auth)
If the user refuses Google Sign-In, we fall back to standard SMS OTP.
- We use **Firebase Phone Authentication** because it gives a generous free tier of SMS messages per month.
- The Go backend verifies the Firebase token and links it to the same user profile.

## 3. Go Backend JWT Issuance
Regardless of whether the user logs in via OTPless, Google, or SMS, the end result is the same:
1. The third-party service verifies the user.
2. The Go backend creates/finds the user in PostgreSQL.
3. The Go backend generates a high-security **JWT (JSON Web Token)**.
4. The Flutter app stores this JWT and uses it for all future API requests.
