# Secret Santa — second family (13 people)

A separate GitHub Pages + Supabase app. This copy has no data or pairings from the original family.

## Members
Steve, Linda, William, Nicole, Emma, Charlotte, James, Joshua, Anna, Josiah, Meg, Alma, Malcolm.

## Setup
1. Create **public** GitHub repository `secret-santa-second-family` (or change the name as desired).
2. Upload the **contents** of this directory to the repository root, including `.github/workflows/deploy.yml`. Do not upload any Supabase secrets or private historical CSVs.
3. Create a **new Supabase project** for this family. In SQL Editor run `supabase/schema.sql` once. This database is independent from the first family's.
4. Under Supabase Authentication -> Providers -> Email, turn off **Confirm Email** only if you accept signup without email ownership verification. The organizer must verify account identities before approval.
5. In GitHub repository Settings -> Secrets and variables -> Actions -> **Variables**, create `VITE_SUPABASE_URL` = the new Supabase URL and `VITE_SUPABASE_PUBLISHABLE_KEY` = its publishable key. Never add `sb_secret_` or service-role keys.
6. In repository Settings -> Pages choose **GitHub Actions**. Run workflow **Deploy Family Secret Santa** (Actions tab).
7. Set Supabase Authentication -> URL Configuration -> Site URL and Redirect URLs to the actual GitHub Pages URL, `https://USERNAME.github.io/REPO/`.
8. One of these 13 people (e.g. Steve) registers first. Supabase creates their profile. Promote their account via Supabase SQL Editor (replace example email):

```sql
update public.profiles
set is_admin = true, approved = true
where id = (select id from auth.users where email = 'ORGANIZER_EMAIL_HERE');
```

9. All other members sign up, then the organizer approves each one. Add permanent family restrictions before drawing names.

## Existing rules
- Each approved account participates (so avoid approving an outside administrator account who does not intend to participate).
- Prevents self-gifts, exclusion rules, and repeats from the immediately preceding calendar year.
- Drawings persist in `assignments`, but the organizer UI shows only assignment counts and any imported historical records, not the full current drawing.
- Thanksgiving cutoff follows **America/Denver**; change the time zone in `src/main.jsx` and `supabase/schema.sql` if this family lives elsewhere.
- This version has **no previous pairing history** imported. If they have old assignments, import them privately into `historical_assignments`.
- GitHub repository is public on a free plan; data remain in Supabase protected by database access rules. Test with a non-admin account before a real drawing.
