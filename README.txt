OVHAN GLOBAL — COMPLETE WEBSITE PACKAGE

Package contents

1.  index.html — customer-facing
    marketplace, now connected to the live Supabase product catalogue.
2.  admin.html — admin website for adding, editing,
    adjusting stock, viewing product images and deleting catalogue
    products.
3.  Ovhan_Global_Admin_README.txt — this setup and operating guide.
4.  supabase_setup.sql — the existing Supabase
    database, security-policy and product-image-bucket setup script,
    included for reference.

What was corrected

The customer marketplace previously displayed a hard-coded sample
catalogue and did not read the products saved by the admin website. The
connected marketplace now:

-   Reads active product records from public.products in the same
    Supabase project used by the admin website.
-   Displays the product name, category, description, price and uploaded
    image from the database.
-   Refreshes the catalogue when product records change through Supabase
    Realtime, where the project publication is enabled.
-   Uses the database UUID as the product identifier throughout product
    details, wishlist, cart and order-item creation.
-   Uses the marketplace’s exact category label for “Toys games and
    hobbies”, and maps the earlier comma-separated label to the
    marketplace category when reading existing records.
-   Does not show the old hard-coded sample products as if they were
    real inventory.

The admin website and marketplace have matching Supabase project URL and
publishable key settings. Product images use the product-images public
bucket configured by the supplied SQL setup.

Deployment

1.  Keep a backup of your currently hosted website and admin website
    before replacing anything.
2.  Deploy index.html as the
    customer-facing marketplace page. If your hosting expects the main
    page to be named index.html, rename a copy of this file to
    index.html when uploading it as the storefront entry page.
3.  Deploy admin.html separately as the admin page
    (for example, admin.html). Do not use the admin HTML as the customer
    storefront.
4.  Host both pages over HTTPS.
5.  Both files are configured for the same Supabase project. Do not
    replace the publishable key with a service_role or secret key.

Supabase setup checks

The SQL setup creates or configures:

-   public.products, including is_active, stock_quantity, image_url and
    image_path.
-   Admin allow-list and inventory movement tables/policies.
-   Public read access to active products only.
-   Admin-only product insert, update and delete permissions.
-   The public product-images storage bucket, with admin-only
    upload/update/delete permissions.
-   Supabase Realtime publication for products and inventory movements.

If this SQL setup was already run successfully for this project, do not
run it again just to deploy the updated HTML. If it has not been run,
review it and run it in Supabase Dashboard → SQL Editor as the project
owner. The admin account must also be authorized in public.admin_users
using its Supabase Auth user UUID, as described in the SQL comments.

How to verify a product upload

1.  Sign in to the admin website with an authorized, email-verified
    administrator account.
2.  Open Products & inventory and add a product.
3.  Enter its name, choose a category, set its price and stock, and
    optionally upload an image.
4.  Save the product. The admin should report that it was saved to
    Supabase.
5.  Open the customer marketplace in a new tab and refresh it once. The
    new active product should appear in the marketplace catalogue. When
    Realtime is available, later product changes should update the
    catalogue automatically.
6.  Try the product’s View and Add to cart actions. Confirm it also
    appears in the cart.

If a product still does not appear

Check these items in order:

-   The admin save completed without an error.
-   The product exists in the same Supabase project’s public.products
    table.
-   Its is_active value is true.
-   The marketplace page is the new
    index.html, not the older static
    marketplace HTML.
-   The marketplace and admin pages use the same Supabase project URL
    and publishable key.
-   The SQL policy Public can read active products exists on
    public.products and grants SELECT to anon and authenticated for
    active rows.
-   The site is served over HTTPS and the browser is not showing an old
    cached version; perform a hard refresh or clear the site cache.
-   If an image is missing but the product appears, check that the
    product-images bucket is public and the image_url field contains the
    generated public URL.

If the product query reports a permission or relation error, inspect the
Supabase browser console/network error and the table’s RLS policies. Do
not disable RLS as a workaround.

Email verification

The admin HTML checks that the signed-in user’s email is verified. In
Supabase Dashboard → Authentication → Providers → Email, enable Confirm
email and make sure email delivery is configured. This dashboard setting
cannot be enabled by changing the HTML alone.

Security notes

-   A Supabase publishable key is intended for browser use; database
    security must be enforced by Row Level Security.
-   Never place a Supabase service_role key or secret key in either HTML
    file.
-   Keep admin access limited to trusted user UUIDs in
    public.admin_users.
-   Use strong administrator passwords and enable additional account
    protection where available.

Testing and limitations

The JavaScript in both HTML files has been syntax-checked. The
product-loading path, table/field mapping, matching Supabase project
configuration and category mapping have been checked in the source
files. This package has not been connected to your live Supabase
database from this session, so a real browser upload and live
RLS/Realtime test still needs to be completed on your hosted site.

This update makes active products visible to the marketplace. It does
not add automatic inventory deduction or payment processing at checkout.
Those require separate server-side order and stock handling before
production use.
