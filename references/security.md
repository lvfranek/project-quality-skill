# Security

Portfolio projects are public. Reviewers (and bots) will look at the code, and a leaked key or an open database is the worst possible first impression.

## Essential

### Secrets
- Scan the repo **and the git history** for secrets: `npx gitleaks detect` (or `git log -p | grep -iE "key|secret|token|password"`).
- A secret that was ever committed counts as leaked: **rotate it** in the provider's dashboard. Deleting it from the history is not enough.
- Everything with a public prefix (`NEXT_PUBLIC_`, `VITE_`, Angular `environment.ts`) ends up in the browser. Only truly public values there — never a service-role key, private API key or database password.
- Real values only in `.env.local` / hosting env settings. `.env.example` has placeholders.

### Backend & data access (Supabase example — adapt to Firebase/own API)
- The anon/publishable key is public by design. It is only safe with **Row Level Security enabled on every table** in exposed schemas.
- Policies are scoped to the user (`auth.uid() = user_id`) and separate for select/insert/update/delete. No `using (true)` on private data.
- Test it: with the anon key and a second test user, try to read/change another user's data. It must fail.
- Storage buckets have policies too. Public buckets only for truly public files.
- The `service_role` key is used only on the server, never in the client bundle.
- Run the Supabase **Security Advisor** (Dashboard → Advisors) and fix its findings.
- **Authorization happens on the server/database.** Route guards (Angular) and middleware (Next.js) are for UX, not protection — check permissions again where the data is read or changed.

### Rate limiting & abuse
- **Supabase Auth** already rate-limits sign-in, sign-up, OTP and password-reset emails. Check the values in Dashboard → Authentication → Rate Limits and set up your own SMTP for real email sending (the built-in one is very limited).
- **Own endpoints** (Next.js route handlers/server actions, edge functions, contact form, anything calling a paid API like OpenAI) are **not** covered by Supabase — they need their own rate limiting. For a portfolio project without meaningful real traffic, a simple in-memory rate limiter (no new account needed) is enough for Essential; Upstash Ratelimit and CAPTCHA are only needed once there is real public traffic (see Extra mile).
- Contact forms: rate limit + honeypot field.

### Input & output
- Validate all input on the server (e.g. `zod`); client validation is only UX.
- No user content into `innerHTML` / `dangerouslySetInnerHTML`; Angular `bypassSecurityTrust…` only with a documented reason.
- Error messages don't leak stack traces, SQL or internal details. Login errors are generic ("Email or password is incorrect").

### Security headers
Set via hosting config (`vercel.json`, `next.config` headers, `netlify.toml`):
- `Content-Security-Policy` (start with a working policy; avoid `unsafe-eval`)
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy` (disable camera, microphone, geolocation if unused)
- `frame-ancestors 'none'` in the CSP (or `X-Frame-Options: DENY`)
- HSTS (usually set by the host — verify)
Check the deployed site with securityheaders.com.

### Dependencies
- `npm audit` high/critical fixed or explained. Framework on a supported, patched version.

### Demo access
- If recruiters should try the app: a **"Try demo" button / guest login** in the app is better than credentials in the README. The demo account has limited rights and its data is reset regularly.

## Extra mile
- Strict CSP with nonces.
- GitHub secret scanning + push protection, Dependabot security updates, CodeQL workflow.
- Account deletion in the app (also helps with GDPR).
- Public sign-up: CAPTCHA (Cloudflare Turnstile or hCaptcha, supported by Supabase Auth) — worth it once there is real public traffic.
- Own endpoints: dedicated rate limiting infra (e.g. Upstash Ratelimit, or the hosting firewall/WAF rules) once the in-memory limiter isn't enough anymore.

## Report
Findings by severity (critical/high/medium/low), before/after. For every secret found: where, and confirmation that the user has rotated it (you can't do that yourself).
