# Technical Specification Document

## Laxmi Astrology AI - Current Application

**Version:** 1.6  
**Last Updated:** November 2025  
**Document Status:** Current Production System

---

## Table of Contents

1. [System Overview](#system-overview)
2. [Architecture](#architecture)
3. [Technology Stack](#technology-stack)
4. [Frontend Specification](#frontend-specification)
5. [Backend Specification](#backend-specification)
6. [Database & Storage](#database--storage)
7. [Third-Party Integrations](#third-party-integrations)
8. [API Specifications](#api-specifications)
9. [Authentication & Security](#authentication--security)
10. [Deployment & Infrastructure](#deployment--infrastructure)
11. [Features & Modules](#features--modules)
12. [Performance & Scalability](#performance--scalability)

---

## System Overview

### Application Type

- **Platform:** Cross-platform mobile application (Android/iOS)
- **Primary Platform:** Android (APK available)
- **App Name:** Laxmi Astro AI
- **Package ID:** `com.Laxmiastrology`
- **Version Code:** 7
- **Version Name:** 1.6

### Core Functionality

1. **AI-Powered Astrology Chatbot** - Vedic astrology consultations using OpenAI
2. **Audio/Video Consultations** - Live sessions with astrologers via VideoSDK
3. **Birth Chart Generation** - Detailed astrological charts
4. **Matchmaking** - Compatibility analysis
5. **Panchang** - Daily astrological calendar
6. **Payment System** - Wallet-based payment with Razorpay/PhonePe
7. **Multi-language Support** - Translation services

---

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Mobile Application                        │
│              (React Native - Android/iOS)                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   Firebase   │ │   Railway    │ │    Vercel    │
│  (Auth/DB)   │ │  (AI Backend) │ │  (Payments)  │
└──────────────┘ └──────┬───────┘ └──────────────┘
                        │
                        ▼
                ┌──────────────┐
                │   OpenAI     │
                │   GPT-5.1│
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ AstrologyAPI │
                │  (json.astro) │
                └──────────────┘
```

### Data Flow

1. **User Query** → Frontend (React Native)
2. **Authentication** → Firebase Auth
3. **AI Request** → Railway Backend (Flask)
4. **LLM Processing** → OpenAI API
5. **Tool Execution** → AstrologyAPI.com
6. **Response** → Structured JSON → Frontend
7. **Payment** → Vercel Backend → Razorpay

---

## Technology Stack

### Frontend

| Component            | Technology                     | Version  |
| -------------------- | ------------------------------ | -------- |
| **Framework**        | React Native                   | 0.73.6   |
| **Language**         | JavaScript/JSX                 | ES6+     |
| **State Management** | React Context API              | Built-in |
| **Navigation**       | React Navigation               | 6.x      |
| **UI Components**    | Custom + React Native Elements | -        |
| **Animations**       | React Native Reanimated        | 3.8.1    |
| **Image Loading**    | React Native Fast Image        | 8.6.3    |
| **Icons**            | React Native Vector Icons      | 10.0.3   |
| **Date Handling**    | Moment.js                      | 2.30.1   |
| **HTTP Client**      | Axios                          | 1.13.2   |
| **Storage**          | AsyncStorage                   | 1.24.0   |
| **Video/Audio**      | VideoSDK Live                  | 0.3.12   |

### Backend

| Component           | Technology    | Version |
| ------------------- | ------------- | ------- |
| **Framework**       | Flask         | 3.1.2   |
| **Language**        | Python        | 3.x     |
| **AI/LLM**          | OpenAI SDK    | 1.108.2 |
| **LLM Model**       | GPT-4o-mini   | Latest  |
| **CORS**            | Flask-CORS    | 6.0.1   |
| **Server**          | Gunicorn      | 23.0.0  |
| **Environment**     | python-dotenv | 1.1.1   |
| **HTTP Client**     | Requests      | 2.32.5  |
| **Data Validation** | Pydantic      | 2.11.9  |

### Payment Backend

| Component           | Technology   | Version    |
| ------------------- | ------------ | ---------- |
| **Framework**       | Express.js   | Latest     |
| **Language**        | Node.js      | 18+        |
| **Payment Gateway** | Razorpay SDK | Latest     |
| **Deployment**      | Vercel       | Serverless |

### Database & Storage

| Component          | Technology                 | Type          |
| ------------------ | -------------------------- | ------------- |
| **Primary DB**     | Firebase Realtime Database | NoSQL         |
| **Authentication** | Firebase Auth              | OAuth         |
| **File Storage**   | Firebase Storage           | Cloud Storage |
| **Session Cache**  | Redis (Optional)           | In-memory     |

---

## Frontend Specification

### Application Structure

```
src/
├── screens/              # Main application screens
│   ├── Chatbot/         # AI Chatbot interface
│   ├── AstroGuru/       # Video/Audio consultation
│   ├── Matchmaking/     # Compatibility matching
│   ├── BirthChart/      # Birth chart generation
│   ├── Panchang/        # Daily panchang
│   ├── Payment.js       # Payment & wallet
│   ├── Profile/         # User profile management
│   └── ...
├── Components/          # Reusable UI components
├── navigations/         # Navigation configuration
├── Api/                 # API endpoints configuration
├── services/            # Business logic services
├── Utility/             # Utility functions
├── contexts/            # React contexts
└── assets/              # Images, fonts, animations
```

### Key Screens

1. **Splash.js** - App initialization & authentication check
2. **Login.js** - Google Sign-In authentication
3. **Home.js** - Main dashboard
4. **Chatbot/index.jsx** - AI astrology chatbot
5. **AstroGuru/** - Live consultation booking & calls
6. **Payment.js** - Wallet top-up & payment
7. **Profile/Profile.js** - User profile & settings

### State Management

- **Context API** (`src/Provider/index.js`)
  - User authentication state
  - Profile data
  - Chat history
  - Preloaded images
  - Question count & wallet balance

### Key Features

#### 1. AI Chatbot (`src/screens/Chatbot/index.jsx`)

- Real-time chat interface
- Birth detail extraction from queries
- Structured JSON response handling
- Message history with Firebase
- 3-minute timeout for API calls
- Loading states & error handling

#### 2. Video/Audio Consultation (`src/screens/AstroGuru/`)

- VideoSDK integration
- Meeting creation & joining
- Participant management
- Call controls (mic, camera, leave)
- Appointment scheduling

#### 3. Payment System (`src/screens/Payment.js`)

- Razorpay integration
- PhonePe integration
- Wallet balance management
- Order creation & verification
- Transaction history

#### 4. Multi-language Support

- Translation service (MyMemory API)
- Language selection
- Dynamic content translation
- Pre-translated common words

### Build Configuration

**Android (`android/app/build.gradle`)**

- Min SDK: 21
- Target SDK: 34
- Compile SDK: 34
- ProGuard: Enabled for release
- Hermes: Enabled
- Signing: Release keystore configured

**iOS** (Available but not primary focus)

---

## Backend Specification

### Application Structure

```
railway-backend/
├── simplified_langgraph_app.py  # Main Flask application
├── astrology_tools.py            # OpenAI function definitions
├── config/                       # Configuration files
├── core/                         # Core utilities
│   ├── session_manager.py       # Session management
│   ├── error_handling.py        # Error handling
│   └── performance.py           # Performance monitoring
├── utils/                        # Utility functions
├── requirements.txt              # Python dependencies
├── Procfile                      # Production server config
└── Dockerfile                    # Container configuration
```

### Main Application (`simplified_langgraph_app.py`)

**Architecture:**

- Flask REST API
- OpenAI Chat Completions API with structured outputs
- Function calling for astrology tools
- Two-step API call pattern:
  1. Initial call with tools → Get tool calls
  2. Execute tools → Second call with results → Structured response

**Key Functions:**

1. **`get_system_prompt()`**

   - Generates system prompt with current date
   - Defines response format requirements
   - Sets astrology guidelines

2. **`extract_birth_details(query)`**

   - Regex-based extraction of birth data
   - Extracts: day, month, year, hour, min, lat, lon
   - Returns structured birth data dictionary

3. **`process_chat(query, birth_data)`**
   - Main chat processing function
   - Handles OpenAI API calls
   - Manages tool execution
   - Returns structured JSON response

**API Endpoints:**

- `POST /chat` - Main chatbot endpoint
- `GET /health` - Health check
- `GET /` - Root endpoint

**Response Format:**

```json
{
  "response": {
    "summary": "Short overview",
    "detailed_analysis": "Comprehensive explanation",
    "recommendations": "Actionable suggestions"
  }
}
```

### Astrology Tools (`astrology_tools.py`)

**Tool Definitions:**

- 12 astrology tools for OpenAI function calling
- Each tool includes:
  - Name, description, parameters (JSON schema)
  - Strict mode enabled
  - Required parameters validation

**Available Tools:**

1. `get_birth_chart` - Birth chart details
2. `get_planets` - Planetary positions
3. `get_current_time` - Current date/time
4. `get_dasha` - Dasha periods
5. `get_manglik` - Manglik dosha check
6. `get_panchang` - Panchang details
7. `get_gemstone` - Gemstone recommendations
8. `get_remedies` - Lal Kitab remedies
9. `get_sadhesati` - Sadhesati status
10. `get_varshaphal` - Annual predictions
11. `get_kalsarpa` - Kalsarpa dosha
12. `get_pitra_dosha` - Pitra dosha
13. `get_muhurta` - Auspicious timings

**Tool Execution:**

- `execute_tool(tool_name, arguments)` - Routes to appropriate astrology API
- `call_astro_api(endpoint, data)` - Makes authenticated requests to json.astrologyapi.com
- Timeout: 45 seconds per API call

### Environment Variables

```bash
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4o-mini
ASTRO_API_USER=636578
ASTRO_API_KEY=15d85de042cd7a0deb9e502bc7658af3a7c4f687
PORT=8000
```

### Production Server

**Gunicorn Configuration (`Procfile`):**

```
web: gunicorn simplified_langgraph_app:flask_app --bind 0.0.0.0:$PORT
```

**Workers:** Default (auto-detected)
**Timeout:** 120 seconds (for OpenAI API calls)

---

## Database & Storage

### Firebase Realtime Database

**Structure:**

```
Firebase Realtime Database
├── Users/
│   └── {uid}/
│       ├── name
│       ├── email
│       ├── gender
│       ├── birthDetails
│       └── wallet/
│           ├── balance
│           └── transactions/
├── ChatHistory/
│   └── {uid}/
│       └── {chatId}/
│           ├── message
│           ├── timestamp
│           └── type
└── Astrologers/
    └── {astrologerId}/
        ├── name
        ├── rating
        ├── chatRate
        ├── audioRate
        └── videoRate
```

**Data Models:**

1. **User Model**

   - uid (string)
   - name (string)
   - email (string)
   - gender (string)
   - photoURL (string)
   - birthDetails (object)
   - wallet (object)
   - createdAt (timestamp)

2. **Chat History Model**

   - chatId (string)
   - message (string)
   - timestamp (number)
   - type (string: "user" | "ai")
   - birthDetails (object, optional)

3. **Wallet Model**
   - balance (number)
   - transactions (array)
   - lastUpdated (timestamp)

### Firebase Authentication

- **Providers:** Google Sign-In
- **User Management:** Firebase Auth SDK
- **Session Management:** Automatic token refresh

### Firebase Storage

- User profile images
- Chat attachments (if any)
- Astrologer profile images

---

## Third-Party Integrations

### 1. OpenAI API

- **Service:** GPT-4o-mini model
- **Usage:** AI-powered astrology responses
- **Features:**
  - Structured JSON outputs
  - Function calling
  - Context-aware responses
- **Rate Limits:** Based on OpenAI tier
- **Cost:** Pay-per-use

### 2. AstrologyAPI.com

- **Service:** Vedic astrology calculations
- **Base URL:** `https://json.astrologyapi.com/v1`
- **Authentication:** Basic Auth (User ID + API Key)
- **Endpoints Used:**
  - `/astro_details` - Birth chart
  - `/planets` - Planetary positions
  - `/current_vdasha_all` - Dasha periods
  - `/manglik` - Manglik dosha
  - `/advanced_panchang` - Panchang
  - `/basic_gem_suggestion` - Gemstones
  - `/lalkitab_remedies` - Remedies
  - `/sadhesati_current_status` - Sadhesati
  - `/varshaphal_details` - Annual predictions
  - `/kalsarpa_details` - Kalsarpa dosha
  - `/pitra_dosha_report` - Pitra dosha
  - `/chaughadiya_muhurta` - Muhurta
- **Timeout:** 45 seconds per request

### 3. Razorpay

- **Service:** Payment gateway
- **Integration:** React Native SDK + Node.js backend
- **Features:**
  - Order creation
  - Payment processing
  - Signature verification
  - Refund management
- **Supported Methods:** Cards, UPI, Net Banking, Wallets

### 4. PhonePe

- **Service:** Payment gateway
- **Integration:** React Native SDK
- **Features:** UPI payments

### 5. VideoSDK Live

- **Service:** Video/audio calling infrastructure
- **SDK:** `@videosdk.live/react-native-sdk`
- **Features:**
  - WebRTC-based video calls
  - Audio-only calls
  - Screen sharing (optional)
  - Recording (optional)
- **Authentication:** JWT tokens (client-side generation)

### 6. Google Sign-In

- **Service:** OAuth authentication
- **SDK:** `@react-native-google-signin/google-signin`
- **Configuration:**
  - Web Client ID
  - iOS Client ID
- **Features:** One-tap sign-in

### 7. MyMemory Translation API

- **Service:** Free translation service
- **Usage:** Multi-language support
- **Fallback:** Pre-translated common words

### 8. OpenCage Geocoding API

- **Service:** Location to coordinates conversion
- **Usage:** Birth place geocoding
- **Endpoint:** `https://api.opencagedata.com/geocode/v1/json`

---

## API Specifications

### Railway Backend API

**Base URL:** `https://railway-backend-laxmi-ai-production.up.railway.app`

#### 1. Chat Endpoint

```
POST /chat
Content-Type: application/json

Request:
{
  "query": "What is my rashi?",
  "birth_details": {
    "day": 15,
    "month": 8,
    "year": 1990,
    "hour": 10,
    "min": 30,
    "lat": 28.6139,
    "lon": 77.2090
  }
}

Response:
{
  "response": {
    "summary": "...",
    "detailed_analysis": "...",
    "recommendations": "..."
  }
}
```

#### 2. Health Check

```
GET /health

Response:
{
  "status": "healthy",
  "timestamp": "2025-11-26T..."
}
```

### Payment Backend API (Vercel)

**Base URL:** `https://payment-backend-blush-tau.vercel.app`

#### 1. Create Order

```
POST /api/create-order
Authorization: Bearer {firebase_token}
Content-Type: application/json

Request:
{
  "amount": 100,
  "questionCount": 1
}

Response:
{
  "success": true,
  "orderId": "order_xxx",
  "amount": 10000,
  "currency": "INR",
  "key": "rzp_live_xxx"
}
```

#### 2. Verify Payment

```
POST /api/verify-payment
Authorization: Bearer {firebase_token}
Content-Type: application/json

Request:
{
  "razorpay_order_id": "order_xxx",
  "razorpay_payment_id": "pay_xxx",
  "razorpay_signature": "sig_xxx",
  "questionCount": 1
}

Response:
{
  "success": true,
  "wallet": {
    "balance": 1,
    "transactions": [...]
  }
}
```

#### 3. Get Wallet Balance

```
GET /api/payment/balance
Authorization: Bearer {firebase_token}

Response:
{
  "balance": 5,
  "transactions": [...]
}
```

---

## Authentication & Security

### Authentication Flow

1. **User opens app** → Splash screen
2. **Check Firebase Auth state** → If logged in, proceed
3. **If not logged in** → Show Login screen
4. **Google Sign-In** → Firebase Auth handles OAuth
5. **Check user exists in DB** → If new, show onboarding
6. **Set authentication context** → Store user data

### Security Measures

1. **Firebase Security Rules** (Database)

   - User can only read/write their own data
   - Chat history protected by UID

2. **API Authentication**

   - Payment backend uses Firebase ID tokens
   - JWT verification on server

3. **Payment Security**

   - Razorpay signature verification
   - Server-side payment validation
   - No sensitive data in client

4. **API Keys**

   - Environment variables for secrets
   - No hardcoded credentials (except dev VideoSDK keys)

5. **HTTPS Only**
   - All API calls over HTTPS
   - Secure WebSocket for video calls

### Token Management

- **Firebase ID Tokens:** Auto-refreshed by SDK
- **VideoSDK Tokens:** Generated client-side (dev) or server-side (production)
- **API Keys:** Stored in environment variables

---

## Deployment & Infrastructure

### Backend Deployment (Railway)

**Platform:** Railway.app  
**Service:** `railway-backend-laxmi-ai-production`  
**Runtime:** Python 3.x  
**Server:** Gunicorn  
**Port:** Dynamic (Railway assigns)  
**Environment:** Production

**Deployment Process:**

1. Push to GitHub
2. Railway auto-deploys from main branch
3. Builds from `requirements.txt`
4. Runs `Procfile` command
5. Health check on `/health`

**Environment Variables (Railway):**

- `OPENAI_API_KEY`
- `OPENAI_MODEL`
- `ASTRO_API_USER`
- `ASTRO_API_KEY`
- `PORT` (auto-set by Railway)

### Payment Backend Deployment (Vercel)

**Platform:** Vercel  
**Service:** `payment-backend-blush-tau`  
**Runtime:** Node.js 18+  
**Framework:** Express.js  
**Type:** Serverless Functions

**Deployment Process:**

1. Push to GitHub
2. Vercel auto-deploys
3. Serverless functions created
4. Environment variables configured

**Environment Variables (Vercel):**

- `RAZORPAY_KEY_ID`
- `RAZORPAY_KEY_SECRET`
- Firebase Admin SDK credentials

### Mobile App Distribution

**Android:**

- **Build Tool:** Gradle
- **Output:** APK (debug/release)
- **Signing:** Release keystore
- **Distribution:** Direct APK installation

**iOS:**

- **Build Tool:** Xcode
- **Output:** IPA
- **Distribution:** TestFlight/App Store (if configured)

### Monitoring & Logging

- **Railway Logs:** Application logs via Railway dashboard
- **Vercel Logs:** Function logs via Vercel dashboard
- **Firebase Console:** Database & auth logs
- **Error Tracking:** Console logs (consider Sentry for production)

---

## Features & Modules

### 1. AI Chatbot Module

**Location:** `src/screens/Chatbot/index.jsx`

**Features:**

- Real-time chat interface
- Birth detail extraction from natural language
- Structured response display (summary, analysis, recommendations)
- Chat history persistence
- Question count tracking
- Loading states & error handling
- 3-minute timeout for responses

**Data Flow:**

1. User sends message
2. Extract birth details from query
3. Format query with birth details
4. Call Railway backend `/chat` endpoint
5. Parse structured JSON response
6. Display formatted response
7. Save to Firebase chat history

### 2. Video/Audio Consultation Module

**Location:** `src/screens/AstroGuru/`

**Features:**

- Astrologer listing
- Profile viewing
- Appointment scheduling
- Video/audio call initiation
- Call controls (mic, camera, leave)
- Meeting recording (optional)

**Components:**

- `AstroGuruLanding.js` - Main landing page
- `AvailableAstrologers.js` - Astrologer list
- `AstrologerProfile.js` - Profile & booking
- `ScheduleAppointment.js` - Calendar booking
- `CallScreen.js` - Active call interface

### 3. Matchmaking Module

**Location:** `src/screens/Matchmaking/`

**Features:**

- Partner birth details input
- Compatibility analysis
- Match score calculation
- Detailed match report
- Chat interface for matchmaking queries

### 4. Birth Chart Module

**Location:** `src/screens/BirthChart/`

**Features:**

- Birth details input form
- Chart generation
- Detailed chart display
- Planetary positions
- House analysis

### 5. Panchang Module

**Location:** `src/screens/Panchang/`

**Features:**

- Daily panchang display
- Tithi, nakshatra, yoga
- Auspicious timings
- Date selection

### 6. Payment & Wallet Module

**Location:** `src/screens/Payment.js`

**Features:**

- Wallet balance display
- Top-up options (₹49, ₹99, ₹199, etc.)
- Razorpay integration
- PhonePe integration
- Transaction history
- Question count management

### 7. Profile Module

**Location:** `src/screens/Profile/`

**Features:**

- User profile display
- Birth details management
- Profile editing
- Promo code redemption
- Language selection
- Notification settings

---

## Performance & Scalability

### Current Performance

**Frontend:**

- Initial load: ~2-3 seconds
- Chat response: 5-30 seconds (depending on tool calls)
- Video call setup: 2-5 seconds

**Backend:**

- API response time: 10-60 seconds
- Tool execution: 5-45 seconds per tool
- OpenAI API: 5-30 seconds

### Scalability Considerations

**Current Limitations:**

1. **OpenAI API Rate Limits** - Based on tier
2. **AstrologyAPI Rate Limits** - Based on plan
3. **Firebase Realtime DB** - Concurrent connections limit
4. **VideoSDK** - Concurrent sessions limit

**Optimization Strategies:**

1. **Caching:** Implement Redis for frequent queries
2. **Connection Pooling:** Reuse HTTP connections
3. **Request Batching:** Batch multiple tool calls
4. **CDN:** Use CDN for static assets
5. **Database Indexing:** Optimize Firebase queries

### Monitoring

**Key Metrics to Track:**

- API response times
- Error rates
- User session duration
- Payment success rate
- Video call quality
- Database read/write operations

**Recommended Tools:**

- Firebase Analytics
- Railway metrics
- Vercel analytics
- Custom logging

---

## Development Environment

### Prerequisites

**Frontend:**

- Node.js 18+
- React Native CLI
- Android Studio (for Android)
- Xcode (for iOS)
- Java JDK 11+

**Backend:**

- Python 3.8+
- pip
- Virtual environment (recommended)

### Setup Instructions

**Frontend:**

```bash
npm install
npx react-native run-android  # or run-ios
```

**Backend:**

```bash
cd railway-backend
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt
python simplified_langgraph_app.py
```

**Payment Backend:**

```bash
cd payment-backend
npm install
npm start
```

### Environment Configuration

Create `.env` files for:

- Railway backend: `OPENAI_API_KEY`, `ASTRO_API_USER`, `ASTRO_API_KEY`
- Payment backend: `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET`
- Frontend: API endpoints in `src/Api/index.js`

---

## Known Limitations & Future Improvements

### Current Limitations

1. **VideoSDK Keys:** Currently hardcoded in frontend (should be server-generated)
2. **Error Handling:** Basic error messages, could be more user-friendly
3. **Offline Support:** Limited offline functionality
4. **Push Notifications:** Basic implementation, needs enhancement
5. **Analytics:** Limited analytics tracking
6. **Testing:** No automated tests

### Recommended Improvements

1. **Security:**

   - Move VideoSDK token generation to backend
   - Implement rate limiting
   - Add request validation

2. **Performance:**

   - Implement response caching
   - Optimize image loading
   - Reduce bundle size

3. **Features:**

   - Offline chat history
   - Push notifications for appointments
   - In-app analytics dashboard
   - Social sharing

4. **Testing:**
   - Unit tests for utilities
   - Integration tests for API
   - E2E tests for critical flows

---

## Support & Maintenance

### Current Support Structure

- **Backend:** Railway platform monitoring
- **Payment:** Vercel serverless monitoring
- **Database:** Firebase console
- **Analytics:** Firebase Analytics (basic)

### Maintenance Tasks

**Daily:**

- Monitor error logs
- Check API health

**Weekly:**

- Review user feedback
- Check payment transactions
- Monitor API usage

**Monthly:**

- Update dependencies
- Review security
- Performance optimization

---

## Appendix

### API Endpoints Summary

| Endpoint               | Method | Purpose               |
| ---------------------- | ------ | --------------------- |
| `/chat`                | POST   | AI chatbot query      |
| `/health`              | GET    | Health check          |
| `/api/create-order`    | POST   | Create Razorpay order |
| `/api/verify-payment`  | POST   | Verify payment        |
| `/api/payment/balance` | GET    | Get wallet balance    |

### File Structure Reference

**Key Files:**

- `src/screens/Chatbot/index.jsx` - Main chatbot
- `railway-backend/simplified_langgraph_app.py` - Backend API
- `railway-backend/astrology_tools.py` - Tool definitions
- `payment-backend/server.js` - Payment API
- `src/Api/index.js` - API endpoints config

### Version History

- **v1.6** (Current) - Structured outputs, improved tool calling
- **v1.5** - Payment integration
- **v1.4** - Video/audio consultation
- **v1.3** - Multi-language support
- **v1.2** - Matchmaking feature
- **v1.1** - Birth chart generation
- **v1.0** - Initial release with AI chatbot

---

**Document End**

_This technical specification document provides a comprehensive overview of the current Laxmi Astrology AI application architecture, technologies, and implementation details._
