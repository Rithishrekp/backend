# Why Am I Not Visible to Customers?

If you have created a Provider account and set yourself as "Available", you might still not appear on the Customer "Services" page. This is usually because **you haven't added any Services yet**.

## The Logic
The website displays **Services**, not just Providers. If a Provider has no Services listed, there is nothing to show to the Customer.

`Provider Account` + `Available Status` + `At Least 1 Service` = **Visible to Customer**

## How to Fix It (Step-by-Step)

1.  **Log in as Provider**
    - Go to `/provider/login`

2.  **Add a Service**
    - Click on **"My Services"** in the left sidebar (under the "Manage" section).
    - Link: `/provider/services`
    - Click the **"Add Service"** or **"Create Service"** button.
    - Fill in the details:
        - **Title**: e.g., "Deep Home Cleaning"
        - **Price**: e.g., "500"
        - **Category**: Select the correct category.
    - Click **"Create Service"**.

3.  **Go Online**
    - Go back to the **Dashboard**.
    - Click the **"Set as Available"** button so it turns **Green** ("Available Now").

4.  **Verify as Customer**
    - Open a new Incognito window.
    - Login as a **Customer**.
    - Go to "Services".
    - You should now see your "Deep Home Cleaning" service listed!

## Troubleshooting
- **Still not visible?**
    - Ensure your "Available" toggle is Green.
    - Ensure you refreshed the Customer page.
    - Check if you accidentally created the service under a different account.
