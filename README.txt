PRIVEXA — FINAL REBUILT PACKAGE
===============================
Files included:
- index.html  : Supabase-connected responsive website
- setup.sql   : shared database, access policies, storage buckets and migrations
- README.txt  : this installation guide

FEATURES IN THIS BUILD
----------------------
- Shared Supabase-backed content across browsers/devices.
- Official PRIVEXA homepage shows only admin-published content/folders.
- Customer content is kept off the main homepage and shown on each customer's own creator link.
- Customer registration/login, dashboard, profile settings, unique slug/public page, WhatsApp contact.
- Customer upload/publish/edit/unpublish/delete for content and folders.
- PDFs/documents, images, videos, website URLs, text/portfolio content, multiple additional links.
- Public/free items and individually priced paid items/folders.
- UPI ID/payee and QR image URL/upload settings.
- Payment transaction reference and screenshot proof submission; admin approve/reject controls.
- Admin-only official publishing, customer management, content/folder management, payment review and UPI settings.
- Three-day access period, extend-by-three-days, disable/enable customer, lifetime access request and approval.
- Private storage bucket for paid uploads/proofs and public storage bucket for public files.
- Schema cache refresh at end of SQL.

INSTALLATION
------------
1. Back up your GitHub repository and current Supabase schema/data first.
2. Open the SQL Editor for the correct Supabase project (project reference in the provided browser key: qqpiypgxpavwppprpjxkd).
3. Paste and run the ENTIRE setup.sql in one query. If it reports an error, stop and resolve the error before continuing; do not assume later statements ran.
4. Register or sign in to the website with the admin email you control. Supabase Auth must contain this user and the profile must exist.
5. In Supabase SQL Editor, run this separate admin promotion query:

   UPDATE public.profiles
   SET role = 'admin'
   WHERE id = (
     SELECT id FROM auth.users
     WHERE lower(email) = lower('Monumuhammed92@gmail.com')
   );

   SELECT id, display_name, role
   FROM public.profiles
   WHERE lower(display_name) = lower('Monumuhammed92@gmail.com') OR role = 'admin';

   Better verification is to run:
   SELECT u.email, p.id, p.role
   FROM auth.users u JOIN public.profiles p ON p.id = u.id
   WHERE lower(u.email) = lower('Monumuhammed92@gmail.com');

6. Sign out and sign in again so the app reloads the current role.
7. Replace index.html in your GitHub repository (Privexa/index.html) with this index.html, commit/push, and wait for GitHub Pages deployment.
8. In Admin > UPI settings, set the UPI ID/payee and upload the QR image or enter its image URL.
9. Test with a separate customer account/browser: confirm the main homepage only shows admin content, the customer's own page shows only their content, public files open, paid access stays locked until approval, and access expiry/disable/lifetime flows behave as intended.

IMPORTANT LIMITATIONS / SECURITY
--------------------------------
- This package has not been run against your live Supabase project or deployed by this assistant. SQL must complete successfully and the UI/payment flows need live testing.
- The Supabase publishable/anon key may be in HTML; NEVER place a service-role key or database password in HTML.
- Keep Row Level Security enabled. The UI is not a substitute for secure server-side policies. Review/test policies before accepting payments or storing sensitive files.
- UPI payments are not automatically verified by a bank API. Admin must manually verify the UTR/reference and screenshot before approval.
- Existing browser-local content will not migrate automatically; re-upload it after setup.
- Admin passwords are not embedded in the HTML or SQL. Use Supabase Auth to set/reset the password.
- The access_requests table and profiles columns (is_disabled, access_expires_at, lifetime_access) are included in setup.sql to address the schema errors seen previously.
