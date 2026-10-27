PRIVEXA PDF UPLOAD FIX

Files
- index.html: improved upload error handling and save flow
- storage_upload_fix.sql: storage bucket configuration and upload policies

Install
1. Open the Supabase dashboard for the same project configured in index.html.
2. Back up your current setup and run storage_upload_fix.sql in SQL Editor.
3. Replace index.html in your GitHub repository with this index.html and commit the change.
4. Wait for GitHub Pages to deploy. Hard-refresh the website, sign in again, then try a small PDF (under 10 MB) first.
5. Select the file, enter a title, and click Save. The form will display a specific error if Storage or the content-row insert fails.

Notes
- The website currently enforces a 50 MB client/bucket limit.
- Public/free files use privexa-public. Paid files or files in paid folders use privexa-private.
- Uploads require a signed-in user and a profile whose role is correctly set for admin publishing.
- This SQL only repairs Storage upload configuration/policies; it does not grant admin role or change content table policies.
- Do not put a Supabase service-role key or database password in HTML.
