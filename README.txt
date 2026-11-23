OVHAN GLOBAL CUSTOMER MARKETPLACE — SUPABASE NOTIFICATIONS UPDATE

This update adds the Notifications switch in Settings and connects it to the existing Supabase notification tables.

DATABASE TABLES USED
- public.customer_notifications: id, user_id, title, message, notification_type, is_read, created_at.
- public.notification_preferences: user_id, enabled, updated_at.
- public.orders: id, user_id, status (used by the automatic order-status trigger).

BEHAVIOUR
- The Notifications switch saves the customer's preference in Supabase, so it follows their account across devices.
- A missing preference row defaults to enabled, matching the database function.
- Order-status changes and admin announcements arrive live while the customer is signed in and the marketplace is open.
- Browser pop-up permission is optional; in-marketplace toast alerts can still appear while the page is open.
- Turning notifications off prevents new order-status notifications and excludes the customer from new announcements.

DEPLOYMENT
1. Back up the currently deployed customer website.
2. Upload this package's index.html to the customer website, replacing the previous index.html only after keeping a backup.
3. Keep the existing Supabase project URL and publishable/anon key configuration unchanged.
4. Sign in, open Settings, and test the Notifications switch. Check notification_preferences in Supabase to confirm the enabled value changes.
5. Keep the marketplace open and signed in while testing an order status change or admin announcement.

LIMITATION
This is live in-page/browser notification delivery. It does not provide background push notifications after the browser is closed.

DATABASE
The compatible notification and advertisement SQL setups have already been run successfully. Do not run an older package's SQL that creates a separate user_notifications table; this update uses customer_notifications.

TEST STATUS
The code has been aligned to the confirmed table/column names. It still needs deployment and live browser testing.
