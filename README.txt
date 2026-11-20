OVHAN GLOBAL CUSTOMER MARKETPLACE — NOTIFICATIONS UPDATE

This update adds a Notifications toggle to Settings. When enabled by a signed-in customer, the browser asks permission to show notifications. The customer can turn the preference off again from Settings.

Notifications supported:
- Order status changes (for example, when an administrator changes an order status).
- Ovhan Global announcements sent from the private admin website.

IMPORTANT LIMITATION
Notifications are delivered live while the customer marketplace is open in a browser and the customer is signed in. This package does not yet include background push delivery when the browser is closed. That requires a service worker plus a push subscription/server delivery service.

DATABASE SETUP
1. Back up the currently deployed customer and admin websites.
2. Open the existing Supabase project's SQL Editor.
3. Run Advertisements_Setup.sql from this package. It includes the existing advertisement setup plus the notification database setup.
   Alternatively, if the advertisement setup has already been run, run Notifications_Setup.sql only.
4. Deploy index.html to the customer website.
5. Customers sign in, open Settings, and turn Notifications on. They must allow notifications in the browser permission prompt.

ADMIN
Deploy the matching admin package and use Customer notifications to send an announcement to customer accounts. Order status changes create an order notification automatically through the SQL trigger.

SECURITY
Customer notification rows are protected by row-level security. Customers can read/update only their own rows. Announcement fan-out is performed by a database function that checks the existing is_ovhan_admin() authorization function. No service-role key is used in either website.

TEST STATUS
Customer and admin JavaScript passed Node.js syntax checks. The SQL and browser notification flow have not been executed against the live Supabase project. Run the SQL before deploying/testing. Browser permission and live delivery depend on the customer's browser/device settings.
