# Publishing EduManage to Google Play

The app is a Trusted Web Activity wrapping https://edumanageja.com. Every push to `main`
builds `EduManage.apk` (sideload) and `EduManage.aab` (Play upload) and attaches both to a GitHub Release.

## 1. Create a NEW upload key (the old key was committed to this repo; treat it as compromised)
```
keytool -genkeypair -v -keystore upload.jks -alias upload -keyalg RSA -keysize 2048 -validity 10000
keytool -list -v -keystore upload.jks -alias upload      # note the SHA-256
base64 -w0 upload.jks > upload_b64.txt                   # PowerShell: [Convert]::ToBase64String([IO.File]::ReadAllBytes("upload.jks"))
```
Set these GitHub repository secrets: `KEYSTORE_BASE64` (contents of the base64 file),
`KEYSTORE_PASSWORD`, `KEY_ALIAS` (`upload`), `KEY_PASSWORD`. Keep `upload.jks` and its passwords
backed up privately. Never commit them.

## 2. Play Console
1. Create a developer account (https://play.google.com/console). Personal accounts created after
   Nov 2023 must run a closed test with 12+ testers for 14 days before production access.
2. Create the app, enrol in Play App Signing, upload `EduManage.aab` from the latest GitHub Release.
3. Store listing assets are in `store-listing/` (512px icon, 1024x500 feature graphic).
   You still need 2+ phone screenshots and the short/full descriptions.
4. Complete App content: privacy policy URL, Data safety, content rating, target audience
   (the app handles student data, so answer the Families/children questions carefully), ads = none.

## 3. Digital Asset Links (removes the browser address bar)
Play Console > Setup > App signing shows the "App signing key certificate" SHA-256.
Add it AND the upload-key SHA-256 to `public/.well-known/assetlinks.json` in the website repo
(package `com.edumanage.app`), alongside the existing fingerprint, then deploy the website.
