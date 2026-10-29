# Supabase migration plan

The current app uses `window.claude.use('db')` and Firestore-like calls such as `db.collection(name).doc(id).set(...)`, `update(...)`, `get()`, `delete()`, and `onSnapshot(...)`. GitHub Pages cannot provide these APIs. A safe migration requires both database schema and frontend authentication/data-access changes.

## Data areas identified in the existing source

- `users`: accounts, role, PIN hash, name, and photo.
- `profiles`: student profile/application information.
- `subjects`: subject catalogue and teacher assignment.
- `admit`, `enrollment`, `grades`: year-specific subject admission, enrollment options, and grades.
- `attendance`: attendance entries.
- `payments`, `receipts`: payment records and receipt images/data.
- `applications`: enrollment requests.
- `settings`: active school year and registrar settings.
- `pins`: encrypted legacy PIN vault records.
- `log`: audit events.

## Required sequence

1. Connect the intended Supabase project and inspect its existing tables/migrations.
2. Design relational tables, constraints, and a data export/import process for existing records.
3. Replace the legacy client-side ID/PIN flow with Supabase Auth and a protected role mapping.
4. Enable RLS and verify policies for anonymous users, students, teachers, and registrar/admins.
5. Replace whole-collection subscriptions with scoped queries and Realtime subscriptions only where necessary.
6. Move privileged account creation, PIN resets, and other registrar-only operations into server-side Edge Functions.
7. Move receipts to a private Supabase Storage bucket with authenticated access.
8. Test enrollment, grades, attendance, payments, receipts, exports, and role boundaries.
9. Deploy the migrated frontend only after security tests pass.

## Do not

- Put a Supabase service-role/secret key in `index.html`.
- Grant broad access to every authenticated user.
- import legacy PINs as trusted authentication credentials without a deliberate reset/migration policy.
- Use real student records until policies and workflows have been verified.
