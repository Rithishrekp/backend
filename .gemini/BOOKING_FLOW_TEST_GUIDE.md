# Booking Flow Integration Test Guide

## Overview
This guide helps you test the complete booking flow between Customers and Service Providers, ensuring that bookings appear correctly on both dashboards and availability is respected.

## Pre-requisites
- Backend running on port 5000 (`npm run dev` in `backend` folder)
- Frontend running (`npm run dev` in `frontend` folder)
- Database updated (Schema includes `is_available` and `price` support)

## Step-by-Step Test Scenarios

### 1. Provider Availability & Setup
1.  **Log in as Provider**:
    - Go to `/provider/login`.
    - Login with a provider account (or create one).
2.  **Set Availability**:
    - On the Dashboard, check the "Set as Available" button.
    - Click it to turn **Green** ("Available Now").
3.  **Verify Status**:
    - If you are "Unavailable" (Grey), your services will **NOT** appear to customers.
    - Keep it "Available" for the next step.

### 2. Customer Booking
1.  **Log in as Customer**:
    - Open a **new Incognito window** (to keep provider session active).
    - Go to `/login` and login as a customer.
2.  **Browse Services**:
    - Go to "Services" page.
    - Search for a service offered by the provider from Step 1.
    - **Verify**: You should see the service.
    - *Optional Test*: specific provider visibility. Toggle the provider to "Unavailable" in the other window and refresh the customer view. The service should disappear. Toggle back to "Available".
3.  **Book a Service**:
    - Click on the service.
    - Select a Date.
    - Click "Confirm Booking".
    - You should be redirected to "My Bookings".

### 3. Verify Dashboards
1.  **Customer Dashboard**:
    - Go to `/customer/bookings` (or via Dashboard > My Bookings).
    - You should see the new booking with status "pending" (or default).
2.  **Provider Dashboard**:
    - Switch back to the **Provider** window.
    - Refresh the Dashboard.
    - Look at the **"Upcoming Jobs"** list.
    - **Verify**: You should see the new booking from the customer, with the correct **Service Name**, **Customer Name**, and **Price/Earnings**.

## Technical Details
- **Filtering**: `getAllServices` in backend only returns providers with `is_available = true`.
- **Linking**: Bookings are linked via `service_id` -> `provider_id`.
- **Data**: The Dashboard now pulls real naming and pricing data from the database.
