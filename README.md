# WORK: Lift Log (iPhone)

The App Store version of WORK. `www/` is the app itself (one HTML file plus fonts). `ios/` is the native iPhone shell made with Capacitor 8, which adds rest-timer notifications, haptics, and keep-screen-awake.

- **Bundle ID:** `com.workliftlog.app`
- **Backend:** Supabase project `nirvqwuqmwjheilscyyi`, table `user_data` (one row per user and key, row-level security per user) and `delete_account()` for in-app account deletion
- **Test build on the web:** https://teesheetapp.github.io/Fitness-Tracker/lift-log/
- **Privacy / Terms / Support pages:** https://teesheetapp.github.io/Fitness-Tracker/lift-log/privacy.html (also `terms.html`, `support.html`)

## Making a change

1. Edit `www/index.html`.
2. Copy it to `teesheetapp/Fitness-Tracker` at `lift-log/index.html` to update the web test build.
3. Push here. The next cloud build picks it up (`npx cap sync ios` copies `www/` into the app).

## One-time setup to reach TestFlight

No Mac needed. Builds run on Capawesome Cloud.

### 1. Apple Developer account
Enroll at developer.apple.com/programs/enroll ($99/year). Wait for approval.

### 2. App ID
developer.apple.com → Certificates, Identifiers & Profiles → **Identifiers → +** → App IDs → App.
- Description: `WORK Lift Log`
- Bundle ID (Explicit): `com.workliftlog.app`
- No extra capabilities needed (local notifications don't require Push).

### 3. Distribution certificate (.p12)
1. Open the Capawesome iOS Certificate Generator (capawesome.io/tools/ios-certificate-generator/) and create a certificate signing request (CSR). Keep that browser tab open.
2. Apple portal → **Certificates → +** → **Apple Distribution** → upload the CSR → download the `.cer`.
3. Back in the generator, turn the `.cer` into a `.p12` with a password. Save the `.p12` and password somewhere safe.

### 4. Provisioning profile
Apple portal → **Profiles → +** → **App Store Connect** (Distribution) → App ID `com.workliftlog.app` → the certificate from step 3 → name `WORK App Store` → download the `.mobileprovision`.

### 5. App record in App Store Connect
appstoreconnect.apple.com → **Apps → + → New App**
- Platform: iOS · Name: `WORK: Lift Log` · Language: English (U.S.)
- Bundle ID: `com.workliftlog.app` · SKU: `work-lift-log`
- If the name is taken, try `WORK — Lift Log` or `WORK Lift Log`.

### 6. App Store Connect API key (lets the cloud upload builds)
App Store Connect → **Users and Access → Integrations → App Store Connect API → Team Keys → +**
- Name `Capawesome`, access **App Manager** → download the `.p8` (only downloadable once). Note the **Key ID** and **Issuer ID**.

### 7. Capawesome Cloud
console.cloud.capawesome.io (14-day free trial, then a paid plan)
1. Create an app and connect this GitHub repo.
2. **Signing certificates → Add**: platform iOS, the `.p12` + password, and the `.mobileprovision`.
3. **Destinations → App Store Connect**: the `.p8`, Key ID, Issuer ID.
4. **Builds → New build**: platform iOS, type App Store, branch `main` → when it finishes, deploy to the App Store Connect destination.

The build appears in App Store Connect → TestFlight after Apple processes it (10–30 minutes). Add yourself and testers under TestFlight.

### 8. Each new build
Bump the build number (`CURRENT_PROJECT_VERSION` in `ios/App/App.xcodeproj/project.pbxproj`). Apple rejects two uploads with the same number. For a new App Store version, also bump `MARKETING_VERSION` and `APP_VERSION` in `www/index.html`.

## Before the public release

Done: sign-up and reset emails send from `noreply@workliftlog.com` through Resend with 6-digit codes; Confirm email is on; leaked password protection is on; `support@workliftlog.com` forwards via ImprovMX; App Review demo account `review@workliftlog.com` is loaded.
