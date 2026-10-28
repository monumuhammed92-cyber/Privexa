PRIVEXA ACCESS APPROVAL + APPROVED CONTENT UPDATE

Included files:
- index.html: updated website interface
- setup.sql: existing database setup/policy script from the prior update

CHANGES IN THIS UPDATE
1. Added an Approved content option in the signed-in navigation and dashboard.
2. Approved folder/content payment requests appear in the customer's Approved content area.
3. Customers can reopen approved content/folders without submitting another payment request.
4. Supabase Auth is configured to persist the session across page reloads and refresh tokens automatically.
5. Login and Create account buttons are hidden while signed in; Sign out is the visible way to end a session.
6. Admin and content-owner approval remains the source of the approved status.

INSTALL
1. Back up your current GitHub repository and database.
2. Replace the root index.html in your GitHub Pages repository with this index.html.
3. Commit/push and wait for GitHub Pages to deploy.
4. Do not rerun setup.sql if you already successfully ran the previous package's SQL, unless you review it and need the schema/policies. The SQL file is included for reference/setup.
5. Test using two separate accounts: a content owner/admin and a buyer. Submit one request, approve it, then check Dashboard > Approved content and reopen the same item. Confirm no duplicate request is created.

NOTES
- Persistent login survives refreshes on the same browser/device, but users may need to sign in again after clearing site data, using another browser/device, token/security changes, or an expired/revoked session. Do not disable secure session expiry.
- Approval access is remembered in payment_requests. Private file URLs are temporary signed URLs and are regenerated when opening approved content.
- This package has not been tested against your live Supabase database. Verify table policies allow the buyer to read their own approved payment_requests and the approved target metadata, while keeping private storage protected.
- Never place a Supabase service-role key or database password in index.html.


HOTFIX — "JSON object requested, multiple (or no) rows returned"
This version changes strict single-row PostgREST lookups to maybeSingle() so
a missing row is handled as an empty result instead of causing the page-level
PGRST116 error. If the message persists, a query may be returning duplicate
rows; check the Supabase table and unique constraints rather than disabling RLS.

Deployment:
1. Back up the current repository index.html.
2. Replace the root index.html with this package's index.html.
3. Commit/push and reload the website.
4. Test Explore, Approved content, opening a folder, and Admin.


HOTFIX V2 — persistent PGRST116 row-count error
The earlier hotfix allowed zero rows but did not prevent a maybeSingle() query
from erroring when duplicate rows matched. This version applies limit(1) to
single-result lookups so duplicate legacy matches do not crash the page.
This is a compatibility mitigation, not a substitute for cleaning duplicates.
After deploying, inspect duplicate profiles.slug and payment_requests records
and add suitable uniqueness constraints only after reviewing existing data.

Deploy the index.html from this V2 package to the repository root and hard-refresh.
Do not run setup.sql again just to install this HTML hotfix.


APPROVED ACCESS UPDATE
The dashboard's Approved content tab clearly lists both approved official PRIVEXA
content/folders and approved customer-owned content/folders. The open buttons
reuse the saved approved payment request. Approved folders open their included
published items. Buyers should not need to submit a second payment request for
the same approved target.

Install by replacing the repository root index.html with this version. The SQL
file is included as reference; do not rerun the full setup script just to install
this HTML update. For private files, the live Supabase Storage RLS policy must
allow the buyer when a matching payment_requests row has status='approved'.
