# Supabase migration plan

## Project

- Project ref: `emylgveeogmtbimcscdz`
- Region: Singapore (`ap-southeast-1`)
- URL: `https://emylgveeogmtbimcscdz.supabase.co`

## Status

The initial database schema has been applied. It includes `profiles`, `subjects`, `teacher_subjects`, `enrollments`, `grades`, `attendance`, `payments`, `applications`, `school_settings`, and `audit_logs`. All tables in the exposed `public` schema have RLS enabled. Receipt uploads use a private Storage bucket named `private-receipts`.

The frontend is not yet migrated. The current app uses `window.claude.use('db')` and Firestore-like calls such as `db.collection(name).doc(id).set(...)`, `update(...)`, `get()`, `delete()`, and `onSnapshot(...)`. GitHub Pages cannot provide these APIs. The legacy ID/PIN and role-selector login must be replaced or supported by a carefully designed server-side authentication flow; the database schema alone does not make the existing page work.

## Data areas identified in the existing source

- `users`: accounts, role, PIN hash, name, and photo.
- `profiles`: student profile/application information.
- `subjects`: subject catalogue and teacher assignment.
- `admit`, `enrollment`, `grades`: year-specific subject admission, enrollment options, and grades.
- `attendance`: attendance entries.
- `payments`, `receipts`: payment records and receipt images/data.
- `applications`: enrollment requests, including signed application information.
- `settings`: active school year and registrar settings.
- `pins`: encrypted legacy PIN vault records.
- `log`: audit events.

## Required sequence

1. Map legacy fields and document IDs to the relational schema; define a controlled export/import plan if existing records need migration.
2. Implement Supabase Auth and a protected role mapping. Do not trust role values supplied by the browser or user-editable metadata.
3. Replace whole-collection subscriptions with scoped relational queries and Realtime subscriptions only where needed.
4. Implement controlled registrar/admin provisioning and PIN reset flows using server-side Edge Functions, if ID/PIN login must be preserved.
5. Enforce least-privilege RLS for anonymous users, students, teachers, and registrar/admins.
6. Move receipts to the private Storage bucket using authenticated uploads and downloads.
7. Test enrollment, grades, attendance, payments, receipts, reports, exports, and role-boundary cases.
8. Confirm the GitHub Pages Actions deployment and test the published site.
9. Import production student records only after security tests pass.

## Do not

- Put a Supabase service-role/secret key in `index.html`.
- Grant broad access to every authenticated user.
- Import legacy PINs as trusted authentication credentials without a deliberate reset/migration policy.
- Make application signatures or receipts public.
- Use real student records until the policies and workflows have been verified.
