PRIVEXA ULTIMATE REBUILD
========================

Included:
- Supabase shared database content, authentication and cloud storage.
- Official homepage shows only content/folders marked is_admin_content=true.
- Customer content is excluded from the homepage and appears only on that customer's own public creator link.
- Customer dashboards, unique public links, content upload, multiple links per item, folders, edit/publish/delete, WhatsApp.
- Per-item and per-folder pricing, UPI transaction reference + screenshot and admin approve/reject.
- Admin publishing, customer management, payment settings, QR image URL or QR file upload.
- 3-day customer trial fields, admin extend-by-3-days, disable/enable, grant lifetime access.
- Customer lifetime access request with UTR and screenshot; admin can approve/reject.
- Public/free uploads and private storage for paid uploads and proof screenshots.

SETUP
1. Back up your GitHub repository.
2. Open Supabase SQL Editor for project qqpiypgxpavwppprpjxkd.
3. Run the ENTIRE setup.sql. If you previously ran an earlier version, this script includes migration statements; if SQL stops with an error, fix that error before deploying.
4. Register/sign in with Monumuhammed92@gmail.com.
5. Run:
   UPDATE public.profiles SET role='admin'
   WHERE id=(SELECT id FROM auth.users WHERE lower(email)=lower('Monumuhammed92@gmail.com'));
6. Sign out and back in.
7. Replace index.html in the GitHub repository with this package's index.html and commit.
8. In Admin > Payment settings, set UPI ID/payee and upload the QR image or supply a QR image URL.
9. Test a customer account, its 3-day trial, creator-only page, and lifetime request flow in a second browser.

SECURITY / LIMITATIONS
- Admin account passwords are never embedded in HTML/SQL. Supabase Auth handles passwords.
- The publishable key is browser-safe only when database Row Level Security policies remain enabled.
- Admin must manually verify UPI/UTR and screenshot; the site cannot confirm payment with the bank.
- The browser can upload a QR image to the public storage bucket; the database policies must have run successfully.
- Customer disabling/expiry is enforced in the UI/public-page route in addition to profile flags; for strict API-level entitlement enforcement, additional RLS policies or server-side authorization should be added and tested before commercial launch.
- Existing localStorage content does not migrate automatically.
- The lifetime access amount is not hardcoded; set/communicate the price and verify payment manually before approving.
- This package has not been connected to or tested against the live Supabase project by this assistant.
