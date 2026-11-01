OVHAN GLOBAL — LIVE MARKETPLACE CATALOGUE UPDATE
================================================

PACKAGE CONTENTS
----------------
1. index.html — Customer marketplace, connected to the live Supabase catalogue.
2. README.txt — Deployment and verification guide.

WHAT THIS UPDATE FIXES
----------------------
The previous marketplace query requested legacy product fields that are not
present in the verified Supabase products table. In particular it requested
price, currency and image_path. That makes the complete SELECT fail, so the
marketplace receives no products even when the admin website successfully
saves them.

This version now reads the verified fields:
- id, name, category, description
- price_ugx and price_usd
- stock_quantity, image_url, is_active and created_at

The storefront uses price_ugx for display and all cart/order calculations in
UGX. It can fall back to price_usd converted to UGX if price_ugx is empty.
Product images are read from image_url, which the admin site saves after
uploading the image to the product-images bucket. No image_path column is
requested.

DEPLOY THE UPDATED MARKETPLACE
------------------------------
1. Download and extract this ZIP.
2. Back up the current customer marketplace index.html.
3. Replace the customer marketplace index.html with this package's index.html.
4. Deploy it to the customer storefront hosting project. Do not replace the
   separate admin website with this file.
5. Open the live marketplace and refresh it. If an old version is cached,
   close the browser tab and reopen the site, or clear that site's cached data.

SUPABASE EXPECTATIONS
---------------------
- The customer storefront and admin website must use the same Supabase project.
- Anonymous/public users must be allowed to SELECT active product rows through
  the existing public.products RLS policy.
- Public image viewing must be enabled for the product-images Storage bucket.
- Product management remains protected by the existing administrator-only RLS
  policies. This HTML update does not change database policies or run SQL.

IF A PRODUCT STILL DOES NOT APPEAR
----------------------------------
1. Confirm it appears in the admin site's Products list.
2. Confirm is_active is true.
3. Confirm the customer storefront is deployed with this updated index.html.
4. If the marketplace shows a Supabase error, send a screenshot of that exact
   message. It will help distinguish a public SELECT/RLS issue from a project
   mismatch or other database issue.
5. If the product appears without its image, check that image_url contains a
   valid public URL for an object in product-images.

PRESERVED FUNCTIONALITY
-----------------------
This update changes only the marketplace product query and field mapping.
It preserves the existing page layout, navigation, language interface,
Supabase authentication, profile features, cart, and ordering flow.

SECURITY
--------
Do not place a Supabase service_role or secret key in browser HTML. Continue to
use the publishable/anon key and enforce access through Supabase RLS.

Ovhan Global | Marketplace catalogue connection
