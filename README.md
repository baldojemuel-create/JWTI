# John Wesley Training Institute Portal

This repository contains the portal's static website files.

## Publish with GitHub Pages

1. Open **Settings → Pages** for this repository.
2. Under **Build and deployment**, choose **GitHub Actions** as the source.
3. Open the **Actions** tab and watch **Deploy portal to GitHub Pages**.
4. After the workflow succeeds, open the site URL shown in the `github-pages` environment.

The included workflow redeploys the site when changes are pushed to `main`.

## Important: database migration is pending

The existing `index.html` uses Claude Artifact's `window.claude.use('db')` and `onSnapshot` APIs. GitHub Pages is static hosting and does not provide these APIs. Therefore, publishing the current file alone will not make login, student records, grades, attendance, enrollment, payments, or receipts work.

The current application also subscribes to whole collections and stores records in browser globals. Do not replace that with unrestricted browser access to a database.

### Security requirements before real use

- Use Supabase Auth for authenticated identities rather than trusting client-side ID/PIN checks.
- Enable row-level security (RLS) on every exposed table and apply least-privilege policies.
- Students must only access their own profile, grades, enrollment, and payment records.
- Teachers must only access records for their assigned subjects.
- Validate registrar/admin actions on the server.
- Never put Supabase service-role/secret keys in browser code.
- Keep receipts in a private Storage bucket; do not expose PIN-vault records or audit logs to public users.
- Test role boundaries and all write operations before importing real student data.

## Status

- GitHub Pages Actions workflow: added.
- Supabase project: none currently visible to the connected account.
- Schema, authentication, RLS policies, data import, and frontend data-access migration: pending.
