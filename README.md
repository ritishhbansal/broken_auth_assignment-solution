# Broken Auth Assignment – Debugged and Completed

This project was an intentionally broken authentication API.  
The objective was to identify and fix issues in the authentication flow and ensure the system works end-to-end.

All bugs have been resolved and the full authentication lifecycle is now functioning correctly, including session handling, OTP verification, JWT issuance, and protected route access.

---

## Tech Stack

- Node.js
- Express.js
- JSON Web Token (jsonwebtoken)
- Cookie-based session handling

---

## Issues Identified and Fixed

### 1. Logger Middleware
- `next()` was missing, causing requests to hang.
- Added `next()` to properly forward requests.

### 2. Authentication Middleware
- `next()` was not called after successful JWT verification.
- Added proper request flow continuation.

### 3. Token Endpoint Bug
- `/auth/token` was incorrectly reading session information from the Authorization header.
- Fixed to correctly read session from `req.cookies.session_token`.

### 4. Silent Error Handling
- The token generator contained an empty catch block.
- Added proper error logging and rethrowing to prevent silent failures.

### 5. Missing Cookie Parser
- `cookie-parser` middleware was imported but not used.
- Added `app.use(cookieParser())`.

### 6. Environment Configuration
- Added required environment variables:
  - `JWT_SECRET`
  - `APPLICATION_SECRET`

---

## Final Authentication Flow

### Step 1: Login
**POST /auth/login**

- Generates a `loginSessionId`
- Logs OTP in the server console

---

### Step 2: Verify OTP
**POST /auth/verify-otp**

- Validates OTP
- Sets HTTP-only session cookie

---

### Step 3: Generate Access Token
**POST /auth/token**

- Reads session from cookie
- Returns a signed JWT access token

---

### Step 4: Access Protected Route
**GET /protected**

Header:
