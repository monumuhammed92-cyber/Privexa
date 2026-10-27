PRIVEXA — COMPLETE SUPABASE REBUILD
===================================
Files in this package
- index.html: responsive dark-themed website with shared Supabase data.
- setup.sql: database tables, RLS rules, storage buckets and admin bootstrap query.

FEATURES INCLUDED
- One GitHub Pages URL; shared published data from Supabase across browsers/devices.
- Supabase account registration, sign-in, sign-out and password-reset email.
- Customer dashboard and unique public creator page.
- Public folders and content: PDF/documents, images, videos, external links, text/portfolio.
- Multiple links per content item and editable content/folder metadata.
- Multiple additional links per content item (one per line: Label | https://example.com).
- Customer publishing/unpublishing, editing and deletion of own content/folders.
- Admin publishing to the main homepage, separate from customer public pages.
- Admin overview, customer profile/role management, all-content/folder controls.
- Per-item and per-folder prices; private storage for paid files and payment screenshots.
- UPI ID, payee name and optional QR image URL settings.
- Payment request with transaction reference and screenshot; admin approve/reject.
- WhatsApp contact link on public creator pages.
- Row Level Security and private storage policies intended to enforce paid-file access.

IMPORTANT SECURITY AND SETUP NOTES
- The admin password is NOT embedded in HTML, SQL or ZIP. Do not put passwords or service-role keys in browser code.
- The existing public/publishable key is included in index.html. That key is intended for browser use only; database RLS is essential.
- I cannot create your Supabase Auth user, run SQL in your live project, or deploy to GitHub from this package. You must complete the steps below.
- The browser prototype cannot verify UPI payments against a bank. Admin must independently verify the UTR and proof before approval.
- Public files go to the public bucket; paid files and payment proofs go to the private bucket. Do not manually make private storage public.
- Take a backup of your existing repository before replacing files.

SETUP STEPS
1. Open Supabase project:
   https://supabase.com/dashboard/project/qqpiypgxpavwppprpjxkd
2. Open SQL Editor -> New query. Paste the ENTIRE setup.sql file and run it.
   If your project already had an earlier PRIVEXA starter schema, this script includes migration columns and drops old starter policies.
3. Open Authentication -> Users. If Monumuhammed92@gmail.com is not already a user, register that email on the website using your chosen password. Confirm the email if Supabase requires confirmation. If the account already exists but the login fails, use the website's password reset option or reset it in Supabase Auth.
4. In Supabase SQL Editor, run this admin promotion query AFTER the account exists and its profile is created:

   update public.profiles
   set role = 'admin'
   where id = (
     select id from auth.users
     where lower(email) = lower('Monumuhammed92@gmail.com')
   );

   select id, display_name, role from public.profiles
   where lower(id::text) in (
     select lower(id::text) from auth.users
     where lower(email) = lower('Monumuhammed92@gmail.com')
   );

5. Sign out and sign in again so the role is refreshed. Open #/admin.
6. In the admin dashboard -> Payment settings, save your UPI ID, payee name and optional QR image URL.
7. Replace the index.html in your GitHub repository with this package's index.html. Commit the change and wait for GitHub Pages deployment.
8. Test the same link in a second browser/device. New data should appear across devices because it is stored in Supabase.

MIGRATION
Existing content saved in browser localStorage will not automatically transfer. Re-upload or re-create it in the new dashboard. The package does not migrate local files or old browser-local passwords.

TROUBLESHOOTING
- “Invalid login credentials”: confirm the exact email exists under Supabase Authentication -> Users and reset/set that account's password.
- “Database setup required” / “Failed to fetch”: confirm SQL was run in the matching project, the project is active, and the URL/key in index.html match that project.
- “Admin access required”: verify the admin promotion query affected one row, then sign out/in again.
- “Permission denied”: check that setup.sql ran fully and that RLS/storage policies were not manually changed.
