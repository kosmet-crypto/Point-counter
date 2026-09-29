# Point: notes for Claude

Point is a points/progress tracker. One web page (`index.html`, vanilla JS, data in `localStorage`)
is shipped three ways: GitHub Pages (web), PWA (`manifest.json`, `sw.js`), and an Android app
(`android/`, a WebView wrapper whose APK GitHub Actions builds and publishes as a Release on every push to `main`).

The owner talks in Serbian (Cyrillic); answer in Serbian. Code, comments and commit messages are in English.

## Privacy (important)
- The owner's real name and email must never appear in this repo. Commit as
  `Claude <noreply@anthropic.com>` (e.g. `git -c user.name=Claude -c user.email=noreply@anthropic.com commit ...`),
  never with a name or email taken from git config, the session or anywhere else.
- The repo was recreated to remove old history. Never push commits from older clones or branches
  whose history does not start at `b9757de` ("Point: points and progress tracker ...").
  Check with `git merge-base --is-ancestor b9757de HEAD` before pushing.
- Merging a PR through GitHub records the owner's GitHub profile name as author/committer of the
  merge. Tell the owner if a merge would do that and they have not changed their profile name.

## Workflow
- Work on a branch, open a PR, wait for the `build` check, and merge only when the owner says so
  ("спој"). Never force-push `main`.
- Before pushing, test the page in headless Chromium (Playwright is preinstalled): load `index.html`,
  exercise the change, and check for page errors.
- The Android SDK is not reachable from the dev container; the PR's CI build is the Gradle build.
  Java can be compile-checked with `javac --release 17` against Robolectric's `android-all` jar from
  Maven Central plus a small stub for `androidx.webkit.WebViewAssetLoader`.

## Rules that keep updates working
- **Page changes (`index.html`)**
  - Bump `VERSION` in `sw.js` so PWA caches refresh.
  - The Android app downloads `index.html` from `main` on start (`WebUpdater`). If the page starts
    calling a new `PointAndroid` bridge method, raise `<meta name="point-native-api">` in `index.html`
    and `WebUpdater.NATIVE_API` together; older apps then keep their page until the APK is updated.
  - Keep `PointAndroid.ready()` being called after the first render. Without it the app rolls a
    downloaded page back to the bundled one.
- **Version numbers:** `versionCode` = Actions `run_number + 10` (`android/app/build.gradle`), and the
  release tag is `v1.0.<run_number + 10>` (`BUILD_OFFSET` in `.github/workflows/android.yml`). The
  in-app update check compares the number after the last dot with the installed `versionCode`, so
  keep both in sync.
- **APK offers:** CI fingerprints `android/` into `BuildConfig.NATIVE_HASH` and the release notes
  (`native: <hash>`). The app only offers an APK when that fingerprint changes; web-only changes
  arrive through `WebUpdater`.
- **Signing:** `android/app/point.keystore` must never change. A different key means installed apps
  cannot update and the owner would have to reinstall (and restore a backup).

## Keeping sessions cheap
- Batch work: do the remaining small items together in one PR, with one CI build and one merge.
- Verify in the browser with numbers (DOM values, counts, page errors); take a screenshot only when
  the layout changes. Screenshots are the most expensive step.
- Keep replies short: what was done and what the owner should try.
- Do not watch CI live or poll it; report back once the build has finished.
