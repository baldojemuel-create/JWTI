# John Wesley Training Institute Portal

Repository: https://github.com/baldojemuel-create/JWTI

## Hosting

A GitHub Actions workflow is configured at `.github/workflows/pages.yml` to publish this repository to GitHub Pages when commits are pushed to `main`. In **Settings → Pages**, select **GitHub Actions** as the deployment source. Then check the Actions tab for the deployment URL and result.

## Supabase project

- Project: **John Wesley Training Institute Portal**
- Project ref: `emylgveeogmtbimcscdz`
- Region: Singapore (`ap-southeast-1`)
- Project URL: https://emylgveeogmtbimcscdz.supabase.co

An initial relational schema has been created for profiles, subjects, teacher assignments, enrollments, grades, attendance, payments, applications, school settings, and audit logs. Row Level Security is enabled on all public tables. A private `private-receipts` Storage bucket has also been created.

## Important: frontend migration is still required

The existing `index.html` uses Claude Artifact's `window.claude.use('db')` and `onSnapshot` APIs. GitHub Pages does not provide these APIs, and the current page has **not yet been migrated to Supabase**. Publishing the static file alone will not make login, student records, grades, attendance, enrollment, payments, or receipts work.

The existing app uses custom student IDs and PINs plus document-style collection records. The new schema uses Supabase Auth UUIDs and relational tables, so the data layer and authentication UI must be refactored together. Do not use a client-side shim that gives every logged-in user access to all records.

### Security requirements before real use

- Use Supabase Auth identities; never treat client-side role/ID/PIN checks as authorization.
- Enable and test row-level security (RLS) on every exposed table.
- Students must only access their own profile, grades, enrollment, attendance, and payment records.
- Teachers must only access records for their assigned subjects.
- Validate registrar/admin actions on the server.
- Never put a Supabase service-role or secret key in browser code. A publishable key is designed for client use when RLS is correctly configured.
- Keep receipts in the private Storage bucket; do not expose PIN vault records, application signatures, or audit logs to public users.
- Test role boundaries and all write operations before importing real student data.

## Current status

- [x] Supabase project created.
- [x] Initial relational schema and RLS policies applied.
- [x] Private receipt Storage bucket created.
- [x] Foreign-key indexes added.
- [x] Security advisor checked (no security findings reported at the time of setup).
- [x] GitHub Pages deployment workflow added.
- [ ] Refactor frontend authentication and data access.
- [ ] Add secure registrar/admin account provisioning.
- [ ] Test all portal workflows and RLS boundaries.
- [ ] Confirm the GitHub Pages deployment succeeds.
