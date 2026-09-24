# Point App

A fast, lightweight, client-side points and progress tracker designed for mobile and desktop web browsers. Built with vanilla JavaScript and modern CSS.

## Features
* Track multiple accounts and custom goals.
* Detailed statistics, paces, streaks, and charts.
* Complete data privacy — everything is saved locally in your browser (`localStorage`).
* Backup and restore capabilities.
* Installable as an app (PWA) and works offline after the first visit.

## Usage
Simply open the live GitHub Pages link to start tracking your points instantly!

### Install as an app
* **Android / Chrome:** open the link, then menu → *Install app* (or *Add to Home screen*).
* **iPhone / Safari:** open the link, tap *Share* → *Add to Home Screen*.

When changing `index.html` or the icons, bump `VERSION` in `sw.js` so installed copies pick up the update.

### Android app (APK)
Every change merged into `main` builds a new APK with GitHub Actions and publishes it as a release.
Always the newest version: https://github.com/kosmet-crypto/Point-counter/releases/latest/download/point.apk

1. Open the link on your Android phone and download `point.apk`.
2. Open the file. Android will ask to allow installs from your browser or file manager; allow it once.
3. Install. Newer APKs install over the old one and keep your data.

Updates:
* **Page updates (most changes):** on every start with internet, the app downloads the latest `index.html` from `main` and uses it from the next start. No APK, no taps. If a downloaded page fails to start, the app falls back to the version inside the APK.
* **APK updates (Android-side changes):** the app checks the latest release at most twice a day (or right away with **Check for updates** on the Accounts tab) and installs it from inside the app with one confirmation. The first time, Android asks you to allow Point to install updates.

When the page starts calling a new `PointAndroid` method, raise `<meta name="point-native-api">` in `index.html` and `WebUpdater.NATIVE_API` in the app, so older apps keep their page until the APK is updated.

The APK bundles `index.html`, so it works offline from the first launch. Its data is stored inside the app,
separately from the browser version, so use **Export backup** in the browser and **Import backup** in the app (Accounts tab) to move your data.
The Android project lives in `android/` (a small WebView wrapper). To build locally: `cd android && ./gradlew assembleRelease`.

