OVHAN GLOBAL ADMIN — ADVERTISEMENTS + CUSTOMER NOTIFICATIONS

This package contains the private admin website update with its existing administrator login, product catalogue, image uploads, size-level stock management, stock alerts, inventory history, order management and slideshow advertisement manager.

CUSTOMER NOTIFICATIONS
- Admins can publish a title and message from Customer notifications.
- The announcement action calls public.publish_ovhan_announcement(p_title, p_body).
- Order status changes create a notification using orders.user_id and orders.status.
- Customers who have disabled notifications are excluded from new announcements and automatic order-status alerts.
- Customer notifications are stored in the existing public.customer_notifications table.

DATABASE STATUS
The compatible notification SQL and advertisement SQL have already been run successfully in the Supabase project. Notifications_Setup.sql is included for reference/recovery and matches the existing customer_notifications and notification_preferences tables. Do not run an older Notifications_Setup.sql from a different package that creates user_notifications/read_at/body; that schema is not used by this update.

DEPLOYMENT
1. Back up the currently deployed admin website.
2. Deploy index.html and logo-cart-plane.png together to the private admin website.
3. Deploy the matching Ovhan_Global_Customer_Notifications_Update package to the customer website.
4. Sign in as an authorised admin, open Customer notifications and send a test announcement.
5. Sign in to a customer account, open Settings, and enable Notifications. The preference is saved to Supabase.
6. Keep the customer marketplace open and signed in to receive live updates. Change a test order status to verify the automatic order notification.

LIMITATION
Live notifications work while the customer marketplace is open and signed in. Background push delivery while the browser is closed is not included; that requires a service worker and push subscription/server delivery.

SECURITY
The announcement function checks public.is_ovhan_admin(). No service-role key is used in either website. Keep the admin site private and do not place a Supabase service-role key in browser code.

TEST STATUS
The source has been updated to use the confirmed database column names. This package has not yet been deployed or tested against the live website/browser session.


MULTIPLE PRODUCT PICTURES (NEW)
--------------------------------
Before using the multiple-picture feature, run this once in Supabase SQL Editor:

ALTER TABLE public.products
ADD COLUMN IF NOT EXISTS images jsonb NOT NULL DEFAULT '[]'::jsonb;

The admin stores the full product gallery in products.images (an array of public image URLs).
The first URL is also stored in products.image_url so older storefront code continues to show
the main product picture. Existing products with only image_url are automatically carried
into the gallery when you open and save them.

In Add/Edit Product, choose several files in the Product pictures field. Remove any preview
with its × button. Existing pictures remain unless individually removed. Images upload to the
existing product-images Supabase Storage bucket, with a 5 MB per-image limit.

NOTE: This ZIP updates the admin dashboard. The customer storefront must also be updated to
read products.images and show a gallery/carousel on the product detail page to let shoppers
browse all pictures. Until that storefront change is made, the main picture remains compatible
through image_url.
