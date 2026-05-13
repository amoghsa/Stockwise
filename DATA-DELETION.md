# StockWise — Data Deletion

**Effective date:** 2026-05-13
**Last updated:** 2026-05-13

This page describes every way to delete data associated with your use of StockWise (iOS and Android). It is published in addition to the full [Privacy Policy](./PRIVACY.md) so that the deletion options are easy to find and to satisfy Google Play's user-data-deletion requirement (see https://support.google.com/googleplay/android-developer/answer/13327111).

If you have any trouble deleting data, email **amogh.agrawal@boomi.com** or open an issue at https://github.com/amoghsa/Stockwise/issues — we'll respond within 30 days.

## Does StockWise have accounts?

**No.** StockWise has no sign-up, no login, no username, no password, and no profile that lives on a server we operate. The app runs as a local "Guest" profile on each device. There is therefore no account to delete — only on-device data and a small amount of pseudonymous telemetry sent to Google's services when you have opted in.

## What data is associated with you, and how to delete each kind

### 1. On-device data (everything you actually entered)

Stored locally in the app's sandbox (Apple SwiftData on iOS, Android Room on Android). This includes your grocery items, lists, pantry state, purchase history, prices, budgets, events, household composition, store labels, custom categories and zones, notification preferences, and all free-text fields.

You have two ways to delete this data:

- **Scoped reset — Settings → Reset Pantry Activity.** Clears stock states, purchase history, recommendation feedback, shopping sessions, and active grocery lists. **Preserves your catalog, household profile, stores, budgets, and custom categories/zones** so you don't have to rebuild them. This is the right option for most users.
- **Full removal — uninstall the app.** Removes the local database in its entirety, including every preference and the Remove Ads purchase-state flag on this device. (The Remove Ads entitlement itself is held by Apple / Google and is restored when you reinstall.)

There is intentionally no in-app "delete absolutely everything" button — uninstall is the supported full-wipe path.

### 2. Firebase telemetry (only if you opted in)

If you turned on **Settings → Data & Privacy → Help Improve StockWise**, Firebase Analytics, Crashlytics, and Performance Monitoring receive pseudonymous event and crash records keyed to a Firebase-generated install ID. No item names, list contents, free-text fields, or precise location are sent — see the [Privacy Policy](./PRIVACY.md#what-is-sent-off-the-device-and-to-whom) for the full list of fields.

To delete this data:

1. **Stop further collection** by toggling off **Settings → Data & Privacy → Help Improve StockWise** on every device where you used the app. This sets Firebase's ad-storage / ad-user-data / ad-personalization consent flags to `denied` and drops locally-buffered events that have not yet been uploaded.
2. **Request deletion of already-collected events** by emailing **amogh.agrawal@boomi.com** with:
   - The approximate dates you used the app
   - The device platform (iOS or Android) and, if possible, the app version
   - The country you were in when using the app (helps narrow the Firebase project region)

   We will forward the request to Firebase and confirm to you when it is complete. Firebase's own retention policy automatically expires event-level data after up to 14 months and unprocessed crash records after 90 days.

There is no install-ID we can show you in-app — Firebase exposes it only to its own SDKs — which is why the email step requires the approximate-dates information instead.

### 3. AdMob (only if you opted in to analytics and you are on the free tier)

When ads are enabled, AdMob receives the standard mobile-ads SDK payload described in https://support.google.com/admob/answer/6128543 — coarse IP-derived location, device type, OS, ad unit, and (only if you allowed tracking via the iOS App Tracking Transparency prompt or the Android equivalent) your device advertising identifier.

To delete or limit this data:

- **Reset / disable the advertising identifier** at the OS level:
  - iOS → Settings → Privacy & Security → Tracking → toggle "Allow Apps to Request to Track" off, or revoke StockWise specifically.
  - Android → Settings → Privacy → Ads → "Delete advertising ID" / "Reset advertising ID".
- **Stop ads from loading at all** by either turning off **Help Improve StockWise** (ads are fully suppressed when analytics consent is off) or by buying the one-time **Remove Ads** in-app purchase.
- **Request deletion of any IDFA-linked data already held by AdMob** through Google's privacy controls at https://myaccount.google.com/data-and-privacy, or contact us and we'll forward a request on your behalf.

### 4. Purchases (Remove Ads)

The single non-consumable IAP (`com.amoghagrawal.stockwise.removeads`) is processed by Apple StoreKit on iOS and Google Play Billing on Android. We never see your payment instrument — only a confirmation that the purchase succeeded.

- The on-device "is premium" flag is removed when you uninstall the app.
- The purchase record itself lives with Apple / Google. To remove it, use Apple's purchase history controls or contact Google Play support. We cannot delete an App Store / Play purchase record on your behalf.

### 5. Voice quick-add audio

StockWise never stores audio. Audio is processed transiently by the operating system's `SpeechRecognizer` API (on-device when supported by the OS); only the transcribed text is returned to the app. There is therefore no audio data for us to delete. If the OS's speech-recognition service retains any audio, that retention is governed by the OS provider (e.g., Google for Google Speech Services — see https://policies.google.com/privacy).

## What happens to data after deletion

| Source | After your action |
| --- | --- |
| On-device database | Wiped immediately (Reset Pantry Activity is scoped; uninstall is full) |
| Firebase Analytics events | Deleted from Firebase's systems within Google's standard deletion SLA after we forward your request; otherwise auto-expired per Firebase retention |
| Crashlytics reports | Same as Analytics; auto-expires per Crashlytics retention if not explicitly deleted |
| AdMob data | Subject to Google's account-wide data deletion controls |
| Apple / Google purchase records | Governed by Apple / Google; we have no ability to delete |

## Contact

For deletion requests we cannot fulfill in-app — primarily Firebase event records — email **amogh.agrawal@boomi.com** with the information listed in §2 above. We will acknowledge within 7 days and confirm completion within 30 days.

For all other privacy questions, see the full [Privacy Policy](./PRIVACY.md) or open an issue at https://github.com/amoghsa/Stockwise/issues.
