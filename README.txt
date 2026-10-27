PRIVEXA SHARED DATABASE FIX — SETUP GUIDE

FILES
- index.html: Supabase-connected PRIVEXA frontend.
- setup.sql: database tables, Row Level Security policies, public file bucket and signup profile trigger.

SETUP STEPS
1. Open your Supabase project: https://supabase.com/dashboard/project/qqpiypgxpavwppprpjxkd
2. Open SQL Editor, create a new query, paste the full contents of setup.sql, and run it.
3. Open Authentication > Providers / Sign In settings. For easier testing you can disable email confirmation; for real use, keep email confirmation enabled.
4. Deploy index.html to your existing GitHub repository, replacing the old index.html. Wait for GitHub Pages to finish deploying.
5. Open the deployed site and register an account using your admin email. Confirm email if required, then sign in once.
6. In Supabase SQL Editor, run the admin promotion query below (replace the email with the email you registered):

update public.profiles set role = 'admin'
where id = (select id from auth.users where lower(email) = lower('YOUR_ADMIN_EMAIL'));

7. Sign out and sign in again, then open #/admin. The admin role should now be active.

WHAT THIS FIXES
- Published admin content comes from Supabase, so the homepage content is shared across browsers and devices.
- Customer accounts use Supabase Auth, not passwords saved in localStorage.
- Customer published content is listed on that customer's public slug page.
- Uploads for public content are stored in Supabase Storage.
- Database policies restrict content ownership and admin publishing.

IMPORTANT LIMITATIONS
- This is a shared-content/auth starter, not yet a full migration of every feature in the old prototype.
- It does not implement payment requests, UPI approvals, or secure paid/private downloads. The configured storage bucket is public, so upload only files meant to be public. Do not upload paid or private files into it.
- To securely sell content, add a private storage bucket, server-side entitlement checks/Edge Functions, and private signed URLs. A client-side “locked” label is not payment security.
- Keep only the publishable/anon key in index.html. Never expose a service-role key or database password.
- Back up your old index.html before replacing it. Existing localStorage content does not automatically migrate to Supabase; upload/recreate it using the new admin dashboard.
