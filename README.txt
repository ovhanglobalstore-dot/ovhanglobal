OVHAN GLOBAL — LIVE MARKETPLACE CATALOGUE UPDATE
================================================

PACKAGE CONTENTS
----------------
1. index.html  — Updated customer storefront.
2. README.txt  — This guide.

WHAT WAS CORRECTED
------------------
The previous storefront contained a fixed list of sample/demo products in its
JavaScript. It did not read the products that you added through the separate
Ovhan Global Admin website. This is why a product could be saved in the admin
site and still not appear in the customer marketplace.

This update:
- Removes the hard-coded sample/demo product catalogue.
- Loads active products directly from the Supabase public.products table.
- Reads the product name, category, description, price, currency, stock and
  image information saved by the admin site.
- Displays the image from image_url or, if needed, from the product-images
  Storage bucket using image_path.
- Supports the existing BIGINT product IDs without converting them to numbers.
- Refreshes both the marketplace home grid and the Explore Products page after
  the database response arrives.
- Shows a useful loading, empty-catalogue or Supabase error message instead of
  silently showing demo products. A Retry control is shown when loading fails.
- Removes obsolete sample-product IDs from saved cart/wishlist data when the
  live catalogue is loaded. Real products in the database remain untouched.

WHY YOUR UPLOADED PRODUCT WAS NOT SHOWING
-----------------------------------------
The storefront HTML had no Supabase query to public.products. It only displayed
its own hard-coded JavaScript array. Therefore, even a successful admin upload
could not appear in that version of the marketplace.

The new storefront now reads from the same products table used by the admin
website. I could inspect the storefront code and identify this mismatch, but I
did not directly inspect or change your live Supabase database row. If the
product still does not appear after deployment, check the conditions below.

DEPLOY THE UPDATED STOREFRONT
-----------------------------
1. Download and extract this ZIP file.
2. Back up the current customer storefront index.html.
3. Replace the customer website's existing index.html with this package's
   index.html.
4. Deploy the updated files to the same Cloudflare Pages project used by the
   customer storefront (not the separate admin website).
5. Open the marketplace and refresh it. If an old version remains cached,
   close the tab and reopen the deployed site, or perform a hard refresh.

CHECK THE PRODUCT IN THE ADMIN SITE
----------------------------------
If your product still does not appear after the updated storefront is live:

1. Open the separate Ovhan Global Admin website.
2. Confirm the product was saved successfully and appears in its Products list.
3. Confirm that the product is Active / is_active = true. Inactive products
   are intentionally hidden from public customers.
4. Confirm the product was saved in the same Supabase project configured in
   the customer storefront:
   https://lewdjmdeukcekzwgwavo.supabase.co
5. Confirm the row is in public.products and has a valid name, category and
   price. The storefront reads the existing products table; it does not create
   or duplicate product records.
6. Confirm the product image is either a public image_url or an image_path in
   the product-images Storage bucket.

SUPABASE ACCESS EXPECTATIONS
----------------------------
The existing setup uses the public read policy for active products. The
storefront query reads only rows where is_active = true. The public/anon role
must be allowed to SELECT active rows in public.products, and the product
images bucket must allow public viewing. Product creation, editing and deletion
remain admin-only; this storefront update does not weaken those permissions.

No SQL migration is included or required by this HTML change. Do not rerun or
replace your existing database setup just for this storefront correction.

IF THE PRODUCT STILL DOES NOT APPEAR
------------------------------------
The marketplace now displays the Supabase error message and a Retry control
when it cannot load the catalogue. Send a screenshot of that message, together
with a screenshot of the product row in the Admin Products list (hide private
customer details). The exact error will show whether the remaining issue is a
missing column, RLS permission, inactive product, wrong Supabase project, or
image path problem.

PRESERVED FUNCTIONALITY
-----------------------
This update is limited to the storefront product catalogue connection. It
preserves the existing Supabase authentication setup, profile photo features,
site navigation, language interface, cart and ordering flow, profile, and other
existing pages. No service-role or secret key has been added to the browser.

TESTING PERFORMED
-----------------
- Confirmed the hard-coded demo product records were removed from the live
  product data source.
- Confirmed the storefront now queries public.products for active products.
- Confirmed image_url and product-images/image_path handling is present.
- Confirmed product IDs are treated as strings to support BIGINT IDs safely.
- Checked the inline JavaScript syntax with Node.js (node --check).

A live browser session and direct database row inspection were not available
for this package build, so final confirmation must be made on the deployed
Cloudflare Pages site using your actual Supabase data.
