# Logica Coffee — recipe share redirect page

This folder holds the tiny static page that makes shared recipe links **clickable**.

When someone taps a link like
`https://logicacoffee.github.io/r/?d=<payload>` it opens this page, which
instantly bounces into the app via `logicacoffee://recipe?d=<payload>` (and offers
an App Store fallback for people who don't have the app yet). The app never needs
an entitlement for this — the page just converts the clickable https link into the
custom scheme the app already handles.

> The payload (`d`) is identical in the https link and the `logicacoffee://` link —
> the page only moves it from one to the other.

---

## One-time setup (free, ~5 minutes)

1. **Create a free GitHub organization** named `logicacoffee`
   → https://github.com/organizations/plan → choose **Free**.
   (Using an org instead of your personal account keeps your username out of every
   shared link, and an org *root* site keeps the door open for Universal Links later.)

2. In that org, **create a public repository named exactly** `logicacoffee.github.io`.
   (The name must match the org — that's what makes it serve at the domain root.)

3. **Add the redirect page**: copy the `r/` folder from here into that repo, so the
   repo contains `r/index.html` at its root.

4. **Enable Pages**: repo → **Settings → Pages** → *Build and deployment* →
   Source: **Deploy from a branch** → Branch: **main** / **/(root)** → **Save**.

5. Wait ~1 minute, then open
   `https://logicacoffee.github.io/r/?d=test` in a browser — you should see the
   "Recipe link looks broken" card (because `test` isn't a real payload). That means
   the page is live. ✅

6. **Add your real App Store link**: once the app has an App Store listing, edit
   `r/index.html` and replace `idREPLACE_WITH_APP_ID` with your numeric App ID
   (e.g. `id1234567890`).

---

## Keep the app's base URL in sync

The app builds share links from a single constant in
`Logica Coffee/ios-native-swift/Models/RecipeLink.swift`:

```swift
static let shareBaseURL = "https://logicacoffee.github.io/r/"
```

If you ever choose a **different org name** (or move to a custom domain), change that
one line to match, and you're done.

---

## Optional upgrades (later, not required)

- **Custom domain** (most professional, ~$10/yr): register e.g. `logica.coffee`,
  add a `CNAME` file to the Pages repo containing the domain, set it in
  Settings → Pages, then point `shareBaseURL` at `https://logica.coffee/r/`.
- **Universal Links** (removes the brief Safari flash + the "Open in app?" prompt):
  add an `apple-app-site-association` file at the repo root under `.well-known/`,
  plus the Associated Domains entitlement (`applinks:logicacoffee.github.io`) in the
  app. Only worth it if the flash bothers you.
