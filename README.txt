OVHAN GLOBAL — CUSTOMER STOREFRONT (CHECKOUT + SIZE STOCK DATABASE COMPATIBILITY)

This package contains the latest Jumia-inspired customer storefront update, including the customer-name greeting, profile icon, account/orders experience, and product size selection with per-size stock display.

DATABASE COMPATIBILITY UPDATES:
- Customer checkout saves order line items to public.orders.items as JSONB.
- Each item includes product_id, name, selected_size, quantity, unit_price, line_total and image.
- Checkout writes the order amount to public.orders.total_amount (matching the live database schema).
- Checkout writes payment_method, status='pending', and payment_status='pending'.
- Order history reads total_amount and items from public.orders.
- Selected clothing/shoe size is retained as selected_size so the Supabase stock trigger can deduct the matching size quantity.

REQUIRED SUPABASE SETUP:
- The orders table must contain items JSONB, payment_method TEXT, total_amount NUMERIC, currency, status and payment_status.
- The products table must contain sizes JSONB and stock_quantity.
- The matching Ovhan Global size/stock SQL setup must already have been run successfully.

IMPORTANT:
- Stock is reserved when a customer submits an order request, before WhatsApp confirmation/payment. The site does not collect payment online.
- Setting an order status to 'cancelled' returns the reserved stock, as configured by the SQL trigger.
- Existing orders created before the stock trigger was installed are not deducted retroactively.
- This package updates the customer checkout and order-history field names. It does not claim a live Supabase test; verify with a controlled test order after deployment.

DEPLOYMENT:
1. Back up the currently deployed customer index.html.
2. Replace it with the index.html in this ZIP.
3. Keep the existing Supabase project URL and anon key configuration unchanged.
4. Test with one low-risk test product/order and confirm products.stock_quantity (and products.sizes for size-managed products) decreases once.
5. Cancel the test order and confirm the reserved quantity is restored.
