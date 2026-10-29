# Supabase migration plan

## Project

- Project ref: `emylgveeogmtbimcscdz`
- Region: Singapore (`ap-southeast-1`)
- URL: `https://emylgveeogmtbimcscdz.supabase.co`

## Implemented

- Created relational tables for `profiles`, `subjects`, `teacher_subjects`, `enrollments`, `grades`, `attendance`, `payments`, `applications`, `school_settings`, and `audit_logs`.
- Enabled RLS on every table in the exposed `public` schema.
- Added student, teacher, and staff-scoped policies and a private `private-receipts` Storage bucket.
- Added indexes for frequently queried foreign keys.
- Added a browser frontend using Supabase Auth and the publishable key, and published it as `index.html`.
- Preserved the original source as `legacy-claude-artifact.html`.
- Checked the Supabase security advisor after setup; no security findings were returned.

## Current migration MVP

The current homepage supports email/password student sign-up and sign-in, profile editing, subject viewing, enrollment requests, application submissions, student grade/attendance/payment views, teacher grade and attendance entry, and basic registrar/admin subject, application, payment, profile, and assignment screens.

The current app has **not been verified as full feature parity** with the original portal. Some functions are not complete, including receipt file upload/download UI, bulk enrollment, complete legacy student profile forms, reports, audit event writes, staff account provisioning, and migration of existing users/records. The original app's `window.claude.use('db')` and `onSnapshot` calls are not available on GitHub Pages.

## Important next steps

1. Finish role-safe staff account provisioning. Do not allow users to set their own staff roles in public sign-up.
2. Add receipt upload/download UI using the private `private-receipts` bucket.
3. Map the original profile and enrollment form fields to the relational schema and implement the missing screens.
4. Decide whether legacy ID/PIN accounts will be reset or migrated. Do not treat legacy PIN hashes as Supabase credentials.
5. Add audit logging through trusted server-side operations.
6. Test RLS with separate student, teacher, registrar, and admin accounts, including attempts to read or update other users' records.
7. Verify the latest GitHub Pages deployment run and test the published URL.
8. Import real student records only after all security tests and workflows pass.

## Do not

- Put a Supabase service-role/secret key in `index.html`.
- Grant broad access to every authenticated user.
- Make application signatures or receipts public.
- Use production student data until role-boundary and workflow tests have passed.
