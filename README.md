# Ovhan Global — Supabase Authentication Setup

This package connects the existing storefront to Supabase Auth for email/password sign-up, sign-in, persistent browser sessions, logout, and routing to `#marketplace` after successful sign-in or successful sign-up request. Full name and phone are saved as Auth metadata and a trigger creates a matching `profiles` row.

## 1. Add public credentials
In Supabase Dashboard, open Project Settings → API (or Data API / API Keys). Copy your Project URL and the Publishable key (`sb_publishable_...`) or legacy `anon` key. In `index.html`, replace the two placeholders near `SUPABASE SETUP` with those values. **Never use `service_role` or `sb_secret_...` in browser code.**

## 2. Run SQL
Open Supabase Dashboard → SQL Editor → New query, paste all of `supabase-setup.sql`, and run it. This creates the `profiles` table, row-level security policies, and a trigger to create a profile row for each new Auth user. Supabase Auth stores passwords; do not create a password table.

## 3. Set Auth URLs
In Authentication → URL Configuration, set Site URL to your deployed site and add the deployed URL to Redirect URLs. Test via localhost or a deployed HTTPS URL rather than opening `index.html` directly as a `file://` URL.

## 4. Email confirmation
If email confirmation is enabled, sign-up routes the user to the marketplace and asks them to confirm their email; Supabase may not create a signed-in session until confirmation. If confirmation is disabled, a new user can sign in immediately (only use this setting for controlled testing; keep confirmation enabled for a public store).

## 5. Test
1. Sign up with a test email. Confirm it if requested.
2. Sign in and confirm the marketplace opens.
3. Open Menu → My profile → Log out and confirm it returns to Sign in.
4. Refresh while signed in and verify the session persists.

## Scope and security
This activates Supabase Auth only. Products remain sample data and orders are still prepared for WhatsApp. Profile pictures remain browser-local. Account deletion is intentionally not implemented client-side: it requires a verified server-side Edge Function before deleting an Auth user. Never store service-role keys in this website.


## Added: guest cart, signed-in ordering, and order tracking
- Visitors can browse the marketplace and use the cart (add, decrease quantity, remove).
- Checkout is blocked unless a customer has a valid Supabase Auth session.
- After an authenticated customer submits delivery details, the order is inserted into `public.orders`, then a WhatsApp message opens with the saved order reference.
- Profile → **Track my orders** lists the signed-in customer's orders and status.
- The SQL now also creates `public.orders` with row-level security. Customers can only insert and read their own orders; customers cannot change order status from the browser.

If you already ran an earlier version of this SQL, run the full updated SQL again. It uses `create table if not exists` and recreates the policies safely for this setup. For a real production business, add an admin-only workflow for changing order statuses and validate prices/product availability server-side before taking payment. This demo currently records item details and totals supplied by the browser, so it is not a payment-grade checkout.
