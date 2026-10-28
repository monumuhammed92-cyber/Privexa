PRIVEXA — Rebuilt website package
=================================

Files
-----
- index.html: website frontend based on your uploaded version, with standalone homepage filtering and per-content/per-folder UPI/QR fields.
- setup.sql: idempotent schema setup/repair script for the current frontend.
- README.txt: installation, admin setup, checks, and known limitations.

Install / update
----------------
1. In Supabase, open the SQL Editor for project qqpiypgxpavwppprpjxkd.
2. Back up your current database first.
3. Run setup.sql. Read any SQL error carefully; do not ignore failed statements.
4. Register or sign in to the intended admin account in PRIVEXA so it exists in auth.users.
5. In SQL Editor, run this one-time command only after verifying the email is yours:

   update public.profiles
   set role='admin', lifetime_access=true, access_expires_at=null, is_disabled=false
   where lower(email)=lower('Monumuhammed92@gmail.com');

   Confirm it updated exactly one row. If it updated zero rows, check the signed-in email and profile row; do not assign another user.
6. Sign out and sign in again.
7. Upload index.html to the root of the GitHub Pages repository, replacing the old index.html.
8. Open the site in a private/incognito browser and test login and upload.

Payment behaviour
-----------------
- Lifetime access uses the separate site_settings UPI/QR and lifetime_price.
- Paid content and folders have their own optional UPI ID, payee name and QR URL/file field; when blank, checkout falls back to the creator's profile UPI details (or admin site UPI as last fallback).
- Payments are manually verified; the site does not confirm UPI transactions with a bank.
- A buyer should submit the UTR/reference and screenshot. Do not approve without independently verifying payment.

Homepage and folder behaviour
-----------------------------
- The main homepage lists only published admin-owned folders and admin-owned content.
- Content attached to a folder is filtered out of the standalone homepage / creator page list. It is intended to appear when that folder is opened.
- Published locked folders remain visible so visitors can see the folder and request access.

Security and operations
-----------------------
- Never put a Supabase service_role key or database password in index.html.
- The browser uses the publishable/anon key only.
- RLS policies protect database writes. Admin role is assigned manually in SQL after the correct account exists.
- Public profile read is enabled so public creator pages can resolve a slug. Do not put private personal information in profiles.
- Private storage is not publicly readable. Signed URLs should be used for protected files.
- Do not delete or rename tables in production without a backup.

Important caveat
----------------
This package is a repair/rebuild based on the single HTML file you uploaded. I cannot run authenticated end-to-end tests against your live Supabase project from here. The SQL should be reviewed against your existing live schema before running, especially if you have legacy columns or constraints. After deployment, test: sign-up, admin promotion, public upload, private upload, folder nesting, payment request, seller approval, lifetime request and admin approval.
