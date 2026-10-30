OVHAN GLOBAL — WEBSITE REPAIR

Files
- Ovhan_Global_repaired.html: updated single-file storefront.

Changes in this repair
1. Language switching now includes the previously missed hero section, benefits, category labels, product/category vocabulary, common product controls and revised privacy copy for Swahili, Luganda, French and Arabic. The existing dynamic-text observer continues translating newly rendered content such as product cards and cart rows.
2. Replaced the profile camera emoji with a pencil/edit SVG icon. The existing profile-photo upload control and handler are preserved.
3. Removed visible demo/sample rating claims and replaced outdated demo wording in the storefront and privacy copy. Existing browsing, search, category filters, sorting, currency display, product details, wishlist, cart, sign-in/sign-up, profile-photo upload, order submission and order-history flows are retained.

Important implementation boundary
- The catalogue still contains starter product records and illustrative images/prices. They are not verified live inventory. Real product records, stock levels, confirmed prices, promotion schedules and current currency rates must come from the product/inventory backend. This repair does not invent real stock or payment credentials.
- Customer authentication, profile photo storage and order saving already call Supabase in this file. They depend on the matching Supabase tables, storage bucket and Row Level Security policies being configured. Online card/mobile-money payment is not collected by this website; a payment provider and secure server-side integration are required.
- No unrelated design or existing Supabase settings were intentionally replaced.

Validation
- Inline JavaScript syntax checked successfully with Node.js.
- A full live browser test against the deployed website and Supabase project was not available here. Keep a backup and test the deployed page on its normal HTTPS address before replacing the live copy.

Deployment
1. Back up the current website HTML.
2. Extract the ZIP.
3. Replace the deployed HTML with Ovhan_Global_repaired.html (rename to index.html only if your hosting expects that filename).
4. Test each language across the hero, menu, marketplace, products, cart, profile and settings; test photo upload, authentication and order saving on the live site.
