PRIVEXA — COMPLETE REBUILD PACKAGE
==================================
Contents
--------
index.html  Responsive dark-themed website frontend.
setup.sql   Supabase database, RLS policies, storage buckets, access controls and security hardening.
README.txt  Setup and troubleshooting instructions.

FEATURES INCLUDED
-----------------
- Supabase Auth registration/login; shared data across browsers/devices.
- PRIVEXA Admin homepage content separated from customer creator pages.
- Each customer has a unique public page slug and their own dashboard.
- Customer content does not appear on the official homepage.
- Admin content/folder management, publish/unpublish, editing and deletion.
- Customer content/folder management and multiple additional links per content item.
- PDF/documents, image, video, audio, ZIP/other file uploads (50 MB bucket limit), external URLs and text/portfolio content.
- Actual file uploads to Supabase Storage; private bucket for paid content/payment proofs and public bucket for public files/QR images.
- Free items and individually paid/locked content or folders.
- Customer-owned UPI ID, payee name and QR image/URL. Buyer payment instructions use the owner of that specific item/folder.
- Buyer UTR/reference and payment screenshot submission; seller/content owner or admin can review requests.
- Separate admin UPI settings for official PRIVEXA content and lifetime-access requests.
- Customer account management: registration date, days joined, trial end date, days remaining, immediate trial expiry, extend trial, disable/enable, lifetime-access grants and role management.
- Configurable lifetime-access price displayed to customers; customers can submit UPI reference and payment screenshot after trial expiry, and admin approves/rejects requests.
- Admin customer management, content management, payment review, access management and site payment settings.
- Row Level Security (RLS) policies and server-side database checks for ownership and access.
- Responsive mobile-friendly layout.

IMPORTANT SECURITY NOTES
-----------------------
1. Never put a Supabase service_role key, database password, or account password in index.html or any ZIP.
2. The frontend contains only the project URL and publishable key. Publishable keys are designed for browser use when RLS is correctly configured.
3. Payment confirmation is manual. This website does not verify bank/UPI payments automatically.
4. Paid files should be uploaded to the private bucket. Public-bucket files are public by design.
5. Customer role/access fields are protected by SQL trigger hardening in this package. Do not remove the trigger or disable RLS.

INSTALLATION
------------
A. BACK UP FIRST
1. Open Supabase Dashboard for project qqpiypgxpavwppprpjxkd.
2. If the project already contains production data, export/backup it before applying SQL.
3. This setup is designed to create/upgrade the listed tables and policies, but review any custom policies before replacing an existing production setup.

B. RUN SQL
1. Supabase Dashboard -> SQL Editor -> New query.
2. Open setup.sql from this package, copy all its contents and run it.
3. If the SQL editor reports an error, stop and fix the first error before continuing; do not repeatedly run partial snippets at random.
4. Wait for the query to finish successfully. The script sends a PostgREST schema-cache reload notification.

C. CREATE ADMIN ACCOUNT
1. Open Authentication -> Users or use the website's Create account page to register the intended admin email.
2. Sign in once so the public.profiles row is created.
3. In Supabase SQL Editor, run the following one-time promotion query, replacing the email only if you intentionally use another admin email:

   UPDATE public.profiles
   SET role = 'admin', is_disabled = false, lifetime_access = true, access_expires_at = NULL
   WHERE id = (SELECT id FROM auth.users WHERE lower(email)=lower('Monumuhammed92@gmail.com'));

4. Verify exactly one row was updated. If zero rows were updated, the email has not registered in this Supabase project yet. Do not share your password.
5. Sign out and sign in again, then open the Admin button.

D. DEPLOY THE HTML
1. Upload index.html to the root of the GitHub Pages repository for PRIVEXA, replacing the old index.html.
2. Commit the change and wait for GitHub Pages to deploy.
3. Keep the same public website URL; customers can use the same URL from different devices.

E. FIRST TEST
1. Register a normal customer account using a different email.
2. Confirm the customer sees a personal dashboard and a unique public page link.
3. Upload a small PDF first. Wait for the success message; check the Storage bucket and content list before leaving the page.
4. Publish a customer item and confirm it appears only on that customer's public page, not on the main homepage.
5. As admin, publish an official item and confirm it appears on the main homepage.
6. Configure customer UPI and QR, create a paid item, then test a payment request using a test amount. Approve it from the seller/admin account.
7. Verify locked content is not visible before approval and becomes available only after approval.

UPLOAD TROUBLESHOOTING
---------------------
- Check that setup.sql ran successfully and both privexa-public and privexa-private buckets exist.
- Bucket limit in this setup is 50 MB per file. Large videos may require increasing the limit in Supabase Storage and checking your plan limits.
- Check browser internet access and Supabase project status.
- If you see "Failed to fetch", verify SUPABASE_URL in index.html is exactly:
  https://qqpiypgxpavwppprpjxkd.supabase.co
- If you see a missing table/column error, make sure the entire setup.sql was run successfully and refresh the website after the schema cache reload.
- If upload succeeds but saving metadata fails, the uploaded file may remain orphaned in Storage; check the content list and Storage before trying repeatedly.
- If admin access is denied, confirm the admin user exists under Authentication -> Users and that the promotion query updated one profile row. Then sign out/in.
- For permission errors, inspect Supabase Logs and RLS policies rather than disabling RLS.

KNOWN LIMITATIONS
-----------------
- UPI payment proofs are manually reviewed; automatic bank reconciliation is not included.
- GitHub Pages hosts a static frontend. Supabase supplies authentication, database, and file storage.
- The public site URL uses hash-based routes for creator pages.
- Before production use, perform a complete test with separate admin, seller, and buyer accounts, including expired/disabled access and paid folders.


CUSTOMER TRIAL MANAGEMENT
--------------------------
- In Admin -> Manage customers, each account shows the registration timestamp, total days joined, trial end timestamp, remaining trial days, and status.
- Use 'End trial now' to expire a customer's trial immediately. Use 'Extend 3 days' to restart/extend access for three days.
- Set the Lifetime access price in the same customer-management panel. The value appears on the customer's Lifetime PRIVEXA access page.
- After trial expiry, a customer can open Request lifetime access, pay the displayed amount to the configured PRIVEXA UPI, submit UTR and screenshot, and wait for admin approval. Approving a request grants lifetime access.
- The days-joined display is based on the account profile creation timestamp; it does not mean the account was actively using the site each day.
- Re-run the full updated setup.sql after backing up existing data. The SQL trigger blocks customers from changing their own role or access/trial fields.
