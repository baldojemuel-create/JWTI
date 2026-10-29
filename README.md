# John Wesley Training Institute Portal

Repository: https://github.com/baldojemuel-create/JWTI

## Live website

The site homepage is now a Supabase-connected portal MVP. GitHub Pages deployment runs are available under [Actions](https://github.com/baldojemuel-create/JWTI/actions). Check the latest run for deployment status and the published URL.

## Supabase project

- Project: **John Wesley Training Institute Portal**
- Project ref: `emylgveeogmtbimcscdz`
- Region: Singapore (`ap-southeast-1`)
- Project URL: https://emylgveeogmtbimcscdz.supabase.co

The initial schema is in place for profiles, subjects, teacher assignments, enrollments, grades, attendance, payments, applications, school settings, and audit logs. RLS is enabled for all public tables. Receipt Storage bucket `private-receipts` is private. The Supabase security advisor reported no security findings after setup.

## Current portal MVP

- Email/password sign-in and student account creation using Supabase Auth.
- Profile editing.
- Student enrollment requests and application submissions.
- Student grade, attendance, and payment views.
- Teacher subject assignment view and grade/attendance entry.
- Registrar/admin views for profiles, applications, payments, subject creation, and teacher assignments.
- Database-enforced row-level access controls.

This is a migration MVP, **not full feature parity with the original portal**. Some workflows from the legacy app (including legacy ID/PIN account migration, full profile/enrollment forms, bulk enrollment, reports, receipt uploads, audit logging, and administrative account provisioning) still require implementation and testing. Do not use production student data until the remaining workflows and role boundaries are verified.

The original Claude Artifact HTML has been preserved at [`legacy-claude-artifact.html`](./legacy-claude-artifact.html) for reference. It uses `window.claude.use('db')`, which is unavailable on GitHub Pages and is not used by the new homepage.

## First-time setup

1. Open the deployed site and create a student account with the email address you will use as the initial administrator.
2. Confirm the email if Supabase Auth asks you to.
3. Sign in once so the student's profile row is created.
4. In the Supabase Dashboard, open **SQL Editor** for this project and run the following after replacing the email with your own account email:

   ```sql
   update public.profiles
   set role = 'admin'
   where email = 'YOUR-ADMIN-EMAIL-HERE';
   ```

5. Sign out and sign back in so the portal loads the new role.
6. For each teacher, have them create a normal account and sign in once. Then, as the administrator, promote that exact email from the Supabase SQL Editor with `update public.profiles set role = 'teacher' where email = 'TEACHER-EMAIL-HERE';`. The teacher should sign out and back in, then you can assign their subjects from the portal. This is a temporary manual provisioning step; a secure staff-provisioning workflow still needs to be built. **Do not make public sign-up accept staff roles.**

Only run the role-promotion query for an account you control. Role changes must remain an administrator-only operation.

## Security requirements before real use

- Never put a Supabase service-role or secret key in browser code. The public publishable key in the frontend is intended for browser use only with RLS correctly configured.
- Students must only access their own profile, grades, enrollment, attendance, and payment records.
- Teachers must only access records for assigned subjects.
- Validate privileged registrar/admin operations on the server.
- Keep receipts in private Storage; do not expose PIN vault records, signatures, or audit logs to public users.
- Test authentication, RLS boundaries, and all write operations before importing production records.
