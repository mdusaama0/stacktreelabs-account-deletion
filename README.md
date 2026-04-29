# stacktreelabs-account-deletion

Public account & data deletion request page for Stacktree Labs apps.
Required by Google Play's data deletion policy — the URL goes in the
Play Console's "Data safety" form.

## What it does

A single-page form where any user can request deletion of their
account and gameplay data from any Stacktree Labs app (currently
**Arrow Drift**; the dropdown is set up to grow as more apps ship).

Backend: **Web3Forms** ([web3forms.com](https://web3forms.com)) — a
no-server email forwarder. Submissions are emailed to
`musaama0@gmail.com` via the access key embedded in `index.html`.

## Live URL

```
https://<your-github-username>.github.io/<repo-name>/
```

(Filled in after the first deploy.)

Plug this URL into:
- Google Play Console → **App content → Data safety → Data deletion URL**.
- Optionally surface it in your app's Settings or Profile screen.

## How deployment works

Push to `main` → `.github/workflows/pages.yml` runs:

1. Stages `index.html` into `_site/`.
2. Uploads as a Pages artifact.
3. Publishes via `actions/deploy-pages@v4`.

Manual redeploys: **Actions tab → Deploy to Pages → Run workflow**.

## One-time GitHub setup

1. Create the repo on GitHub and push `main`.
2. Settings → Pages → Build and deployment → Source: **GitHub Actions**.
3. Save. First deploy fires on the next push (or run the workflow manually).

## Updating the form

Edit `index.html`, commit, push to `main`. The workflow republishes
within ~30 seconds.

Common edits:

- **Add a new app to the dropdown** → search for `<select id="appName">`
  and add a `<option value="New App">New App</option>` line.
- **Change the destination email** → search for `WEB3FORMS_KEY` and the
  hardcoded `musaama0@gmail.com` strings.
- **Tweak processing-time copy** → search for "30 days".
- **Adjust retention notice** → the `.retention-card` block near the
  bottom of the body.

## Web3Forms note

The access key in `index.html` is tied to `musaama0@gmail.com`. If you
rotate it (e.g. for a different inbox or because the current key is
ever leaked publicly):

1. Visit https://web3forms.com/#start
2. Generate a new access key.
3. Replace the `WEB3FORMS_KEY` constant inside the `<script>` block
   near the bottom of `index.html`.

The free tier allows 250 submissions/month — more than enough for
deletion requests at app launch.

## Test before going live

After the first deploy:
1. Open the live URL.
2. Fill the form with your own data (Name, Email, App).
3. Submit — you should see the success modal.
4. Check the destination inbox within ~1 minute.

If nothing arrives, check Web3Forms' dashboard for the submission and
verify the access key.
