# Dice Roller

Add up to 20 dice (d4, d6, d8, d10, d12, d20), roll them with an animation, and keep a timestamped history of every roll, with each die's individual result and the total.

Runs in any browser, installs to a phone home screen as a PWA, and can be wrapped for the App Store and Google Play with Capacitor.

## Project layout

```
www/                     The whole app (this is what gets deployed and wrapped)
  index.html             UI, logic and styles in one file
  manifest.webmanifest   PWA install info
  sw.js                  Service worker for offline use
  icons/                 App icons
resources/               1024px icon + splash source art for native builds
capacitor.config.json    Native wrapper config
.github/workflows/       Auto-deploys www/ to GitHub Pages
```

## Run it locally

No build step. Open `www/index.html` in a browser, or serve it (needed to test install/offline):

```bash
npm install
npm run serve
```

## Change the dice limit

In `www/index.html`, edit the single constant near the top of the script:

```js
const MAX_DICE = 20;
```

## Publish on GitHub Pages

1. Push this repo to GitHub (branch `main`).
2. In the repo go to **Settings > Pages** and set **Source** to **GitHub Actions**.
3. Each push to `main` redeploys. The app will be at `https://<your-username>.github.io/<repo-name>/`.

On a phone, open that URL and choose **Add to Home Screen** to install it.

When you ship a change, bump `CACHE_VERSION` in `www/sw.js` so installed copies refresh.

## Build the iOS and Android apps (Capacitor)

Prerequisites: Node 20+, Android Studio (for Android), Xcode on a Mac (for iOS).

```bash
npm install

# One-time: change the app ID in capacitor.config.json first (e.g. com.yourname.diceroller).
# It must be unique and can't be changed easily after you publish.

npx cap add android
npx cap add ios          # Mac only

npm run assets           # generates native icons/splash from resources/
npm run cap:android      # sync + open Android Studio
npm run cap:ios          # sync + open Xcode
```

After editing anything in `www/`, run `npx cap sync` to copy it into the native projects. Commit the generated `android/` and `ios/` folders.

### Store submission

- **Google Play:** one-time $25 developer fee. Build a signed app bundle (.aab) in Android Studio (Build > Generate Signed Bundle). Keep your keystore safe and out of git.
- **Apple App Store:** $99/year Apple Developer Program. Archive and upload from Xcode (Product > Archive).

## Known limitations

- Roll history lives in memory and resets when the app is closed. Persisting it (localStorage or `@capacitor/preferences`) is the natural next feature.
- Fonts load from Google Fonts. Offline, the app falls back to system fonts. Self-host the font files if you want identical offline typography.
- d10 rolls 1 to 10.
