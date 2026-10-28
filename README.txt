PRIVEXA — Access Approval Update
================================

Files to upload
---------------
1. index.html — upload to the root of your GitHub Pages repository, replacing the current index.html.
2. setup.sql — run in the SQL Editor of Supabase project qqpiypgxpavwppprpjxkd.
3. README.txt — instructions.

What this update changes
------------------------
- Admin-owned PRIVEXA folders/content: access requests are reviewed by the PRIVEXA admin/content owner.
- Customer-owned folders/content: the customer who owns the content sees incoming requests in Dashboard > Payments and can approve/reject them.
- A pending request stays locked. Approval grants access only to the approved content item or folder. Approval of a folder also grants access to its contained files; separate content outside that folder remains locked.
- Buyers can see their own request status. Sellers can see incoming requests for their content.
- Private Storage files remain private. The SQL adds a SELECT policy for the uploader/admin and buyers with an approved item/folder request.
- Removes the known legacy `profiles_update_self_or_admin` policy that directly queried `profiles` and can cause infinite recursion. Recreates profile policies using a SECURITY DEFINER admin helper.

Install steps
-------------
1. Back up the Supabase database first.
2. Open Supabase > SQL Editor for project qqpiypgxpavwppprpjxkd.
3. Paste and run the complete setup.sql file. If it reports an error, stop and send a screenshot of the exact error; do not ignore it.
4. Confirm the intended verified admin profile is still assigned role='admin'. Do not promote an account unless you have verified its email.
5. Replace index.html in the GitHub repository root with this package's index.html and commit the change.
6. Refresh the website in a private/incognito browser, sign in, and test using two accounts: one content owner and one buyer.

Test sequence
-------------
1. Owner publishes a paid folder or item and configures their UPI details.
2. Buyer signs in and submits payment reference + screenshot.
3. Owner opens Dashboard > Payments and reviews the incoming request.
4. Owner approves the request after independently verifying payment.
5. Buyer refreshes the page and opens the approved item/folder.
6. Test that a separate paid item without approval remains locked.
7. Repeat with PRIVEXA-admin-owned content; the admin approves it.

Important
---------
- UPI payments are not automatically verified by this site. Verify transaction references independently before approving.
- Never place a service_role key or database password in index.html.
- The SQL repairs the policies known from the current package and the reported recursion issue; it cannot be authenticated end-to-end tested against your live project from this environment. Back up first and report any SQL error exactly.
