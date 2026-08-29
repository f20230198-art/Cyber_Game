# TECHCORP // BREACH

A cinematic, mobile-first cybersecurity CTF for live workshops. Four simulated
vulnerabilities, four flags, one fictional company.

**Everything is simulated client-side.** There is no backend, no database, no real
exploit, and no user data. It is safe and legal to deploy publicly.

---

## Deploy

Single static file. No build step, no dependencies.

**GitHub Pages** — push this folder to a repo, then Settings → Pages → deploy from `main` / root.

**Netlify** — drag the folder onto app.netlify.com/drop.

**Vercel** — `npx vercel --prod` in this folder.

**Local test** — `python -m http.server 8000`, then open `http://localhost:8000`.

Then generate a QR code pointing at the URL and put it on the projector.

---

## The four stages

| # | Vulnerability | What the player does | Flag |
|---|---|---|---|
| 1 | SQL Injection | Types `' OR '1'='1` into the login form | `GDG{sql_1nj3ct10n_101}` |
| 2 | IDOR | Edits the fake URL bar to `?id=1` | `GDG{1d0r_c0unt1ng_up_fr0m_0n3}` |
| 3 | Information Disclosure | Reads the page source and finds the flag in an HTML comment | `GDG{h1dd3n_1n_pl41n_s1ght}` |
| 4 | Broken Access Control | Strips `disabled` from `#reqAdmin` | `GDG{cl13nt_s1d3_1s_n0t_s3cur1ty}` |

Stage 1 accepts nine injection patterns (`' OR 1=1 --`, `admin'--`, `') OR ('1'='1`,
double-quote variants, comment terminators, etc.), so players who know the technique
aren't fighting a single hardcoded string. Ordinary logins — including names with
apostrophes like `O'Brien` — correctly fail.

Stage 2 accepts both `?id=1` and `?id=0`, and nudges with a "getting warmer" toast
for IDs between 2 and 12.

---

## Running it in a workshop

**Every stage works on a stock iPhone or Android — no laptop, no DevTools, no cable.**

Stages 3 and 4 normally require developer tools, which iOS Safari does not provide.
Both have an in-app route to the same lesson:

- **Stage 3** — a `</> VIEW PAGE SOURCE` button renders the page's real HTML, with the
  flag sitting in a real (muted, unhighlighted) comment. Players still have to read
  source to find it.
- **Stage 4** — press and hold the inspect bar for one second to strip the `disabled`
  attribute off the real button, exactly as deleting it in DevTools would.

Players on a laptop can still use real DevTools and `view-source:` for both — the
two paths coexist, and the hints mention both.

The hint button gives three escalating hints per stage and starts glowing after 25
seconds of inactivity. The third hint is always the literal answer, so nobody stalls
out and drops.

**Sound is off by default** — the speaker icon in the top bar toggles it. Leave it off
for a room full of phones.

## Notes

- The "247 people breached today" counter is simulated from the date plus localStorage.
  It grows through the day and is consistent for everyone in the room. No backend, no
  third-party counter service, no privacy surface.
- Matrix rain is throttled to ~13fps on a half-resolution canvas so it stays smooth on
  older phones.
- `prefers-reduced-motion` disables the rain, the sweep, and all transitions.
- Only external request is Google Fonts; the page degrades to system mono/sans if it
  is blocked.

## Customising

Flags live in the `FLAGS` object at the top of the `<script>`. If you change Flag 3,
you must also change it in the HTML comment above the "Kitchen Update" post —
otherwise verification will never succeed.

Accent colour is the `--acc` CSS custom property in `:root`. Swap it for `#00d4ff`
for the cyan variant.
