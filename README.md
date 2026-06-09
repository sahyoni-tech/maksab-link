# maksab.link — universal-link host

Tiny static site that powers **Android App Links / iOS Universal Links** for the
Maksab mobile app (repo: `sahyoni-tech/maksab`). Served by **GitHub Pages** on the
custom domain `maksab.link`.

Its whole job: when someone scans a printed QR sticker or taps a shared deal link
(`https://maksab.link/branch/<id>` or `/deal/<id>`), the OS opens the installed
app directly. If the app isn't installed, the browser shows a small fallback page.

## Files

| File | Purpose |
|---|---|
| `.well-known/assetlinks.json` | Android verification — proves this domain belongs to the app. Contains the app `package_name` + the **SHA-256** of the signing cert. |
| `.well-known/apple-app-site-association` | iOS verification (stub — fill the Apple Team ID when iOS ships). |
| `index.html` | Landing shown when someone visits `maksab.link` directly. |
| `404.html` | Catch-all fallback for `/branch/*` and `/deal/*` when the app isn't installed (GitHub Pages serves it for unmatched paths). |
| `CNAME` | GitHub Pages custom-domain binding (`maksab.link`). |

## Updating the Android SHA-256

`assetlinks.json` must list the SHA-256 fingerprint of the certificate that signs
the **installed** APK. With EAS:

```sh
eas credentials -p android        # pick the build profile → copy "SHA256 Fingerprint"
```

(or copy it from the project's **Credentials** page on expo.dev). Paste it into
`assetlinks.json` → `sha256_cert_fingerprints`. If the app later goes to the Play
Store, **add the Play App Signing SHA-256 too** (Google re-signs the app), as a
second entry in the array.

After changing any file here, **rebuild the native app** so Google re-checks the
association.

## DNS

`maksab.link` is registered at the registrar; its DNS points the apex at GitHub
Pages, with the custom domain set in this repo's Pages settings. `assetlinks.json`
must be reachable over **HTTPS** with content-type `application/json`.
