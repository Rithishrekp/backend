# 📊 ServisGo Website - Complete Project Analysis

## 🎯 Project Overview

**Project Name:** ServisGo - Service Booking Platform  
**Type:** Full-Stack Web Application  
**Architecture:** MERN-like Stack (PostgreSQL instead of MongoDB)  
**Status:** In Development - Frontend & Backend Integration Phase

---

## 🏗️ Project Structure

```
website_building/
├── backend/           # Node.js + Express + PostgreSQL
│   ├── config/        # Database configuration
│   ├── controllers/   # Business logic
│   ├── routes/        # API endpoints
│   ├── server.js      # Entry point
│   └── initDb.js      # Database initialization
│
└── frontend/          # React + Vite
    ├── src/
    │   ├── pages/     # All page components
    │   │   ├── public/    # Public pages (17 pages)
    │   │   ├── customer/  # Customer dashboard (12 pages)
    │   │   ├── provider/  # Provider dashboard (13 pages)
    │   │   └── admin/     # Admin dashboard (7 pages)
    │   ├── components/    # Reusable components
    │   ├── api.js         # API integration layer
    │   ├── App.jsx        # Main routing
    │   └── index.css      # Global styles
    └── public/
```

---

## 🔧 Technology Stack

### **Backend**
- **Runtime:** Node.js (ES6 Modules)
- **Framework:** Express.js v5.2.1
- **Database:** PostgreSQL
- **Authentication:** JWT (jsonwebtoken v9.0.3)
- **Password Hashing:** bcryptjs v3.0.3
- **Environment:** dotenv v17.2.3
- **CORS:** cors v2.8.6
- **Dev Tool:** nodemon v3.1.11

### **Frontend**
- **Framework:** React v19.2.0
- **Build Tool:** Vite v7.2.4
- **Routing:** React Router DOM v7.13.0
- **Icons:** Lucide React v0.563.0
- **Styling:** Vanilla CSS (Custom Design System)

### **Database**
- **DBMS:** PostgreSQL
- **Host:** localhost:5432
- **Database Name:** website_building_db
- **User:** postgres

---

## 📊 Database Schema

### **Tables:**

#### 1. **users**
```sql
- id (SERIAL PRIMARY KEY)
- name (VARCHAR(100))
- email (VARCHAR(100) UNIQUE)
- password (VARCHAR(255)) -- Hashed with bcrypt
- role (VARCHAR(20)) -- 'customer', 'provider', 'admin'
- created_at (TIMESTAMP)
```

#### 2. **services**
```sql
- id (SERIAL PRIMARY KEY)
- provider_id (INTEGER) -- References users(id)
- title (VARCHAR(100))
- description (TEXT)
- price (DECIMAL(10, 2))
- category (VARCHAR(50))
- image_url (TEXT)
- created_at (TIMESTAMP)
```

#### 3. **bookings**
```sql
- id (SERIAL PRIMARY KEY)
- user_id (INTEGER) -- References users(id)
- service_id (INTEGER) -- References services(id)
- booking_date (DATE)
- status (VARCHAR(20)) -- 'pending', 'confirmed', 'completed'
- created_at (TIMESTAMP)
```

---

## 🔌 Backend API Endpoints

### **Base URL:** `http://localhost:5000/api`

### **Authentication Routes** (`/api/auth`)
| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| POST | `/auth/register` | Register new user | `{ name, email, password, role }` |
| POST | `/auth/login` | Login user | `{ email, password }` |

**Response Format:**
```json
{
  "id": 1,
  "name": "User Name",
  "email": "user@example.com",
  "role": "customer|provider|admin",
  "token": "JWT_TOKEN"
}
```

### **Service Routes** (`/api/services`)
| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/services` | Get all services | - |
| POST | `/services` | Create new service | `{ title, description, price, category, imageUrl, providerId }` |

### **Booking Routes** (`/api/bookings`)
| Method | Endpoint | Description | Query Params |
|--------|----------|-------------|--------------|
| POST | `/bookings` | Create booking | Body: `{ userId, serviceId, date }` |
| GET | `/bookings/:userId` | Get user bookings | `?role=customer|provider` |

**Booking Logic:**
- **For Customers:** Returns bookings made by the user
- **For Providers:** Returns bookings for services they own (via JOIN with services table)

---

## 🎨 Frontend Architecture

### **Routing Structure**

#### **Public Routes** (No authentication required)
- `/` - Home page
- `/services` - All services listing
- `/services/category/:categoryId` - Services by category
- `/services/:serviceId` - Service details
- `/how-it-works` - How it works page
- `/about` - About page
- `/faq` - FAQ page
- `/contact` - Contact page
- `/blog` - Blog listing
- `/blog/:blogId` - Blog details
- `/become-provider` - Provider signup
- `/login` - Login page ✅ **Connected to Backend**
- `/signup` - Signup page ⚠️ **Not yet connected**
- `/forgot-password` - Password recovery
- `/terms` - Terms of service
- `/privacy` - Privacy policy
- `/cancellation-policy` - Cancellation policy

#### **Customer Routes** (`/customer/*`)
Protected routes for customers:
- `/customer` - Dashboard
- `/customer/book-service` - Book a service
- `/customer/schedule` - Schedule booking
- `/customer/address` - Address selection
- `/customer/checkout` - Checkout
- `/customer/confirmation` - Booking confirmation
- `/customer/bookings` - My bookings
- `/customer/bookings/:bookingId` - Booking details
- `/customer/profile` - Profile settings
- `/customer/addresses` - Saved addresses
- `/customer/payments` - Payment history
- `/customer/notifications` - Notifications

#### **Provider Routes** (`/provider/*`)
Protected routes for service providers:
- `/provider/onboarding` - Provider onboarding
- `/provider` - Dashboard ✅ **Connected to Backend**
- `/provider/job-requests` - Job requests
- `/provider/jobs/:jobId` - Job details
- `/provider/active-jobs` - Active jobs
- `/provider/completed-jobs` - Completed jobs
- `/provider/availability` - Availability management
- `/provider/services` - Service management
- `/provider/earnings` - Earnings overview
- `/provider/payout` - Payout settings
- `/provider/profile` - Profile settings
- `/provider/reviews` - Reviews
- `/provider/support` - Support

#### **Admin Routes** (`/admin/*`)
Protected routes for administrators:
- `/admin` - Dashboard
- `/admin/users` - User management
- `/admin/services` - Service management
- `/admin/bookings` - Booking management
- `/admin/reports` - Reports
- `/admin/blog` - Blog management
- `/admin/faq` - FAQ management

---

## 🔗 Frontend-Backend Integration

### **API Integration Layer** (`src/api.js`)

#### **Implemented Functions:**

1. **`checkBackendHealth()`**
   - Checks if backend is running
   - Endpoint: `GET http://localhost:5000/`

2. **`loginUser(credentials)`** ✅ **CONNECTED**
   - Authenticates user
   - Endpoint: `POST /api/auth/login`
   - Used in: `Login.jsx`
   - Flow:
     ```
     User enters credentials → API call → Backend validates → 
     Returns user data + JWT token → Store in localStorage → 
     Redirect based on role (customer/provider/admin)
     ```

3. **`registerUser(userData)`** ⚠️ **NOT YET CONNECTED**
   - Registers new user
   - Endpoint: `POST /api/auth/register`
   - Status: Function exists but not used in SignUp.jsx

4. **`getBookings(userId, role, token)`** ✅ **CONNECTED**
   - Fetches user bookings
   - Endpoint: `GET /api/bookings/:userId?role=provider|customer`
   - Used in: `ProviderDashboard.jsx`
   - Includes JWT token in Authorization header

---

## ✅ What's Working (Connected)

### **1. Login System** ✅
**File:** `frontend/src/pages/public/Login.jsx`

**Flow:**
```
1. User enters email & password
2. Frontend calls loginUser() from api.js
3. Backend validates credentials (authController.js)
4. Backend returns user data + JWT token
5. Frontend stores in localStorage:
   - localStorage.setItem('user', JSON.stringify(data))
   - localStorage.setItem('token', data.token)
6. Redirect based on role:
   - Provider → /provider
   - Admin → /admin
   - Customer → /customer
```

**Backend Controller:** `backend/controllers/authController.js`
- Validates email/password
- Compares hashed password using bcrypt
- Generates JWT token (30-day expiry)
- Returns user object with token

### **2. Provider Dashboard** ✅
**File:** `frontend/src/pages/provider/Dashboard.jsx`

**Flow:**
```
1. Component loads
2. Retrieves user from localStorage
3. Calls getBookings(userId, 'provider', token)
4. Backend queries bookings for services owned by provider
5. Displays bookings with:
   - Service name
   - Customer name
   - Booking date
   - Status
```

**Backend Controller:** `backend/controllers/bookingController.js`
- Joins bookings, services, and users tables
- Filters by provider_id
- Returns booking details with customer info

---

## ⚠️ What's NOT Connected Yet

### **1. SignUp Page** ❌
**File:** `frontend/src/pages/public/SignUp.jsx`

**Issue:** Form doesn't call `registerUser()` API
- No form submission handler
- No state management for form data
- Role selector exists but not integrated

**What needs to be done:**
```javascript
// Add state management
const [formData, setFormData] = useState({
  name: '',
  email: '',
  password: '',
  role: 'customer'
});

// Add form handler
const handleSubmit = async (e) => {
  e.preventDefault();
  try {
    const data = await registerUser(formData);
    localStorage.setItem('user', JSON.stringify(data));
    localStorage.setItem('token', data.token);
    // Redirect based on role
  } catch (error) {
    // Handle error
  }
};
```

### **2. Customer Dashboard** ❌
**File:** `frontend/src/pages/customer/Dashboard.jsx`

**Issue:** Not fetching bookings from backend
- Currently uses mock data
- Should call `getBookings(userId, 'customer', token)`

### **3. Service Listing Pages** ❌
**Files:** 
- `AllServices.jsx`
- `ServiceCategory.jsx`
- `ServiceDetails.jsx`

**Issue:** Not fetching services from backend
- Need to add API function to fetch services
- Backend endpoint exists: `GET /api/services`

### **4. Booking Creation** ❌
**Files:**
- `BookService.jsx`
- `Schedule.jsx`
- `Checkout.jsx`

**Issue:** Not creating bookings in backend
- Need to integrate with `POST /api/bookings`
- Backend endpoint exists but not connected

---

## 🔐 Authentication & Authorization

### **Current Implementation:**

1. **JWT Token Generation:**
   - Secret: `process.env.JWT_SECRET` (fallback: 'secret123')
   - Expiry: 30 days
   - Payload: `{ id, role }`

2. **Storage:**
   - User data: `localStorage.getItem('user')`
   - Token: `localStorage.getItem('token')`

3. **Role-Based Routing:**
   - Login redirects based on role
   - Separate dashboards for customer/provider/admin

### **Missing:**
- ❌ Protected route middleware
- ❌ Token verification on frontend
- ❌ Auto-logout on token expiry
- ❌ Backend middleware to verify JWT on protected routes

---

## 🎨 Design System

### **CSS Variables** (from `index.css`)
- Custom color palette
- Spacing system
- Typography scale
- Border radius tokens
- Transition timings
- Gradient definitions

### **Component Patterns:**
- Card-based layouts
- Form inputs with icons
- Button variants (primary, secondary, ghost)
- Badge components
- Modal/overlay patterns

---

## 🚀 How to Run

### **Backend:**
```bash
cd backend
npm install
node initDb.js  # Initialize database (first time only)
npm run dev     # Runs on http://localhost:5000
```

### **Frontend:**
```bash
cd frontend
npm install
npm run dev     # Runs on http://localhost:5173 (Vite default)
```

### **Database Setup:**
1. Install PostgreSQL
2. Create user 'postgres' with password '418Kp418'
3. Run `node initDb.js` to create database and tables

---

## 📝 Summary of Integration Status

### ✅ **Fully Connected:**
1. Login authentication
2. Provider dashboard bookings
3. Database connection
4. JWT token generation

### ⚠️ **Partially Connected:**
1. SignUp page (backend ready, frontend not integrated)
2. Customer dashboard (needs booking integration)

### ❌ **Not Connected:**
1. Service listing/details pages
2. Booking creation flow
3. Service creation (provider)
4. Admin functionality
5. Profile updates
6. Password reset
7. Payment integration

---

## 🎯 Next Steps for Full Integration

### **Priority 1: Complete Authentication**
1. Connect SignUp page to backend
2. Add protected route guards
3. Implement token verification

### **Priority 2: Core Features**
1. Connect service listing to backend
2. Implement booking creation flow
3. Connect customer dashboard

### **Priority 3: Provider Features**
1. Service creation/management
2. Job request handling
3. Earnings tracking

### **Priority 4: Admin Panel**
1. User management
2. Service approval
3. Booking oversight
4. Analytics/reports

---

## 🔍 Key Observations

### **Strengths:**
✅ Clean separation of concerns  
✅ RESTful API design  
✅ Role-based architecture  
✅ Modern React patterns  
✅ Comprehensive page structure  
✅ Database properly normalized  

### **Areas for Improvement:**
⚠️ Missing authentication middleware  
⚠️ No error boundaries  
⚠️ Limited form validation  
⚠️ No loading states on many pages  
⚠️ Missing API error handling  
⚠️ No environment config for frontend API URL  

---

## 📊 Project Statistics

- **Total Pages:** 49 (17 public + 12 customer + 13 provider + 7 admin)
- **Backend Controllers:** 3 (auth, booking, service)
- **API Endpoints:** 7
- **Database Tables:** 3
- **Lines of Code (approx):** 
  - Backend: ~500 lines
  - Frontend: ~5000+ lines

---

**Last Updated:** January 30, 2026  
**Status:** Active Development - Integration Phase
