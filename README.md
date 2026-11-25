# topherhooper.com

The front page for the domain: one static page that links to each project. No build and no
server of its own. It lives in `site/` and is served by Firebase Hosting as the site
`topherhooper-home` in the Google Cloud project `fluted-citizen-269819`, the same project as
World Wide War (play.topherhooper.com).

Until January 2025 this repo held a GPT-4 chatbot (React + FastAPI on Cloud Run). That code
was retired in November 2026 and is still in git history before the commit that removed it.

## Sign-in

The page shows "Sign in" or "Signed in as …" at the top right. Sign in opens Google's pop-up
right here, then asks World Wide War's server for a session cookie that covers all of
topherhooper.com, so play.topherhooper.com is signed in too; Sign out clears it everywhere.
`firebase.json` rewrites `/api/session` to that server. The Firebase SDK loads from Google's
CDN, so there is still no build. Where a pop-up cannot open (some phones, pop-up blockers) the
link falls back to a plain sign-in page at `play.topherhooper.com/signin`. How it works, and
the Google settings that have to list `topherhooper.com`, are in World Wide War's
`docs/deployment.md`, "Shared sign-in" and "Sign-in origins".

## Adding a project

Add one `<li>` card to `site/index.html`. Open `site/index.html` in a browser to check it.
Keep anything personal off this page: it is public.

## Deploying

```sh
npx firebase-tools login                       # once per computer
npx firebase-tools deploy --only hosting:topherhooper-home
```

## One-time setup (already done once it's live)

```sh
# Create the Hosting site
npx firebase-tools hosting:sites:create topherhooper-home --project fluted-citizen-269819
# Then, in the Firebase console, Hosting > topherhooper-home > Add custom domain:
# topherhooper.com, and www.topherhooper.com redirecting to it. Add the records it shows
# to the Cloud DNS zone `topherhooper-com` (see World Wide War docs/deployment.md, DNS).
```
