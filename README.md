# Mr. Karzan English

Separate Next.js 16 application for the academy, based on the supplied homepage design and content. It lives in its own folder and leaves the Huda app at the workspace root unchanged.

## Run locally

1. Use Node.js 22 or newer and run `npm install` in this folder.
2. In Supabase, create a project. From its Connect/API Keys panel, copy the **Project URL** and **publishable key** into `.env.local` as `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`.
3. Apply every SQL file in `supabase/migrations/` in filename order (`202611070001` through `202611070006`). The seed migration adds the three sample courses and English/Sorani public copy.
4. In Supabase Authentication URL Configuration, set the Site URL to `http://localhost:3123` and allow `http://localhost:3123/auth/callback` as a redirect URL.
5. Verify the owner account and assign it the `owner` role using the SQL comment at the end of the first migration.
6. Restart the local server and open <http://localhost:3123>.

The public page renders with local fallback content before Supabase is configured. Auth, admin, course, and lesson functions require the database migrations and an owner account.

For sign-up, users create an account with email and password and confirm through the email link. Supabase's Auth redirect allow-list must include the callback URL above. Keep email confirmation enabled for a public site and configure a reliable SMTP sender before launch. Never put a Supabase `service_role` secret in a `NEXT_PUBLIC_` variable or share it in chat. See the [Supabase Next.js Auth guide](https://supabase.com/docs/guides/auth/quickstarts/nextjs) and [Auth redirect URL guide](https://supabase.com/docs/guides/auth/redirect-urls).

## Implemented

- Responsive, black-and-blue English/Sorani landing page based on the supplied HTML, including its teacher portrait, keyboard focus, reduced-motion support, RTL switching, and saved language preference. Course prices are absent from the homepage.
- Supabase Auth registration, sign in/out, email verification callback, password reset, and account profile settings.
- Supabase schema and RLS for accounts, roles, editable site content, courses and lessons, video assets, prices, enrollments, payments, progress, announcements, notifications, and audit logs.
- Owner editor for translations, home sections, academy/contact details, and uploaded site images.
- Public course catalogue and details, with optional visible IQD prices on course detail pages only.
- Admin course/chapter/section/lesson editing, publication and ordering, plus IQD price management.
- Cloudflare Stream direct TUS uploads, processing status sync/deletion, authorized short-lived playback tokens, a student watermark, and playback progress writes.
- Staff student directory search, course-access review, manual access grants/revocations, progress summary, and account suspension/reactivation. Staff actions are audited.
- Standard security headers, origin checks on write routes, permission checks, and database RLS.

## Not ready for launch

No Supabase project, Cloudflare account, domain, email sender, or merchant credentials are configured here. Cloudflare upload/playback code cannot be verified against a live account until its credentials and origin are supplied. Payment checkout, verified provider callbacks, receipts/refunds, coupons/subscriptions, payment settings, admin-role management, announcements UI, and the full revenue/activity overview are not implemented. Do not accept real payments or promise paid course access in this build.

The video watermark discourages link sharing but cannot prevent screen recording. Cloudflare Stream signed playback is not DRM. Use provider sandbox accounts and complete account/access review before launch.

## Local checks

Run `npm run typecheck`, `npm run lint`, and `npm run build` from this folder. Apply the Supabase migrations to a test project separately; they have not been run against a live database in this workspace.

See [Architecture and provider decisions](docs/ARCHITECTURE.md) for the database outline, provider comparison, official Iraq payment references, cost assumptions, and launch setup.
