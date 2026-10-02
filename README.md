# Little Shop — setup

## 1. Supabase
1. Create a project at supabase.com.
2. SQL Editor → paste and run `supabase/schema.sql` (tables, RLS, `place_order`, sample products).
3. Project Settings → API: copy the **URL** and **anon key** into the top of `index.html`.

## 2. Google sign-in (Google Cloud Console)
1. console.cloud.google.com → create a project → **APIs & Services → OAuth consent screen** (External), add your email as a test user.
2. **Credentials → Create credentials → OAuth client ID → Web application**.
3. Authorized redirect URI: `https://YOUR-PROJECT.supabase.co/auth/v1/callback`
   (Authorized JS origins: your site URL, plus `http://localhost:3000` for dev).
4. Copy the Client ID and Secret → Supabase **Authentication → Providers → Google** → enable and paste.
5. Supabase **Authentication → URL Configuration**: set Site URL to your site and add it (and localhost) to Redirect URLs.

## 3. Mailgun
1. Create a Mailgun account and add/verify a sending domain (the sandbox domain only emails authorized recipients).
2. Copy your **Private API key**.
3. Deploy the function and set secrets (Supabase CLI):
   ```
   supabase login && supabase link --project-ref YOUR-REF
   supabase secrets set MAILGUN_API_KEY=key-xxx MAILGUN_DOMAIN=mg.yourdomain.com
   # EU region only: supabase secrets set MAILGUN_BASE_URL=https://api.eu.mailgun.net
   supabase functions deploy send-confirmation
   ```

## 4. Run
`npx serve -l 3000 .` then open http://localhost:3000, or deploy the folder to Netlify/Vercel/Cloudflare Pages.

## Notes
- Orders, line items and products live in Supabase; users see only their own orders (RLS).
- Totals are computed in the database, not trusted from the browser.
- Payments are not integrated — "Place order" records the order only. Stripe would be the natural next step.
