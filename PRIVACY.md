# StockWise Privacy Policy

**Effective date:** 2026-05-08
**Last updated:** 2026-05-08

StockWise ("we", "us", "the app") is an offline-first grocery and pantry planning app for iOS and Android. This policy explains what data the app handles, where it lives, and the choices you have. We've tried to keep it short and free of legalese — if anything is unclear, open an issue at https://github.com/amoghsa/Stockwise/issues.

## Summary, in one paragraph

Your shopping data — items, lists, pantry stock, events, purchase history, household size — stays on your device. We do not run a backend that stores it, and we do not sync it to the cloud. The app uses Google Firebase (Analytics, Crashlytics, Performance Monitoring) to understand how the app is used and to find crashes, and Google AdMob to show ads in the free tier. Analytics is opt-out: a single toggle in Settings → Data & Privacy turns it off. You can wipe everything the app stored, on device and in analytics, with **Delete All Data** in Settings.

## What stays only on your device

The following is stored locally (Apple SwiftData on iOS, Room on Android) and never sent to a server we operate:

- Grocery items, categories, and the lists you build
- Pantry inventory state (in stock / running low / out)
- Purchase history and consumption-rate estimates the app derives from it
- Events you create (parties, trips) and any guest counts you enter
- Household composition you set during onboarding (number of adults, number of children — counts only, no names or identities)
- Notification preferences and shopping-frequency settings
- Recommendation feedback (whether you accepted or dismissed a suggestion)

The app currently has no account system and no cross-device sync. If you uninstall the app or tap **Delete All Data**, this information is gone.

## What is sent off the device, and to whom

### Google Firebase — Analytics, Crashlytics, Performance Monitoring

Used to measure activation, retention, feature usage, crashes, and slow screens. The full list of events the app emits is published in the [analytics events doc](https://github.com/amoghsa/Stockwise/blob/main/docs/analytics-events.md) in the source repository. Examples: `app_opened`, `screen_viewed`, `item_added`, `suggestion_accepted`, `shopping_session_completed`, `purchase_completed`.

Along with each event, Firebase receives the standard SDK context: a Firebase-generated install ID (an opaque random identifier, not your name or email), device model, OS version, app version, language, country (derived from IP — the IP itself is not retained by Firebase Analytics for advertising), and a coarse session timestamp. We attach a small set of **user properties**: `is_premium`, `household_size` (a bucket, e.g. "3"), `account_type` ("guest"), `app_theme`, and `analytics_consent`.

We do **not** send: your item names, list contents, pantry contents, event names, the contents of any free-text field, your precise location, or device advertising identifiers via Analytics.

Crashlytics may include a stack trace, the device state at the moment of the crash, and a Crashlytics-generated installation UUID. Performance Monitoring reports timing for app startup, screen rendering, and network calls.

You can turn all of the above off with **Settings → Data & Privacy → Help Improve StockWise**. When the toggle is off, Analytics collection is disabled (`setAnalyticsCollectionEnabled(false)`); existing locally-buffered events that have not yet been uploaded are dropped.

Firebase's own privacy documentation: https://firebase.google.com/support/privacy

### Google AdMob — advertising (free-tier users only)

Free-tier users see occasional native ads on Home and Pantry, and may opt into rewarded video ads in exchange for unlocking premium-style insights for 24 hours. Ads are served by Google AdMob.

To request and serve an ad, the AdMob SDK collects information described in Google's mobile ads SDK disclosure, which can include device identifiers (IDFA on iOS only when you grant App Tracking Transparency permission; the Android Advertising ID on Android), device type, OS, coarse network info, and the ad unit being filled. On iOS, if you decline tracking in the system ATT prompt, ads are served in a non-personalized mode.

We do not pass any of your shopping data to AdMob. We send AdMob the ad unit identifier and the SDK's standard request payload — nothing else.

AdMob privacy & data disclosures: https://support.google.com/admob/answer/6128543

If you'd rather not see ads at all, the **Remove Ads** one-time in-app purchase disables every ad surface and the rewarded prompts.

### Apple App Store / Google Play — purchases

The **Remove Ads** purchase is processed by Apple StoreKit on iOS and Google Play Billing on Android. We never see your card number; we receive only a confirmation that the purchase succeeded and a product identifier (e.g. `remove_ads_lifetime`), which we store on device to suppress ads. Apple and Google's own privacy policies cover the payment leg.

## What the app does **not** collect

To be explicit, because some of these are common in similar apps:

- **No precise location.** The app does not request foreground or background location permission, does not track your in-store movement, and does not build maps from your phone. (The product spec includes a forward-looking idea around aisle-based routing — that feature is not implemented today, and if it ever ships it will be opt-in and described in an updated version of this policy before launch.)
- **No contacts, photos, microphone, calendar, or health data.**
- **No barcode or receipt scanning** in the current release.
- **No account, no email, no password, no name.** The app runs as a local "Guest" profile.
- **No third-party analytics besides Firebase.** No Facebook SDK, no Mixpanel, no Segment.

## Children

StockWise is a household utility intended for adults. The "children" count in household setup is just a number used by the recommendation engine to scale quantities — it is not a profile of a child and is not associated with any identity. We do not knowingly collect personal information from anyone under the age of 13 (or under 16 in jurisdictions where that threshold applies), and the app provides no mechanism for a child to create an account because there are no accounts at all.

## Your choices and rights

- **Turn off analytics:** Settings → Data & Privacy → Help Improve StockWise.
- **Reset advertising identifier:** iOS Settings → Privacy & Security → Tracking; or Android Settings → Privacy → Ads.
- **Delete everything:** Settings → Data & Privacy → Delete All Data. This wipes all local databases and calls the analytics SDK's reset, which clears the Firebase install ID and user properties going forward.
- **Uninstall the app:** removes the local database entirely. Any analytics already received by Firebase before uninstall is retained per Firebase's own retention policy unless you separately request deletion (see below).
- **Right of access / deletion (GDPR, UK GDPR, CCPA / CPRA, and similar regimes):** because we don't run a backend, the data we can access about you is essentially the Firebase install-ID-keyed analytics and crash records. To request deletion of those, email the address below with the approximate dates of use and the device platform; we will forward the deletion request to Firebase and confirm when complete.
- **"Do Not Sell or Share" (CCPA):** we do not sell personal information and do not share it for cross-context behavioral advertising. AdMob's ad serving is configured to run in non-personalized mode where required by user signals (ATT denied on iOS, GPC on web equivalents, applicable region defaults).

## Data retention

- **On-device data:** kept until you delete it or uninstall the app.
- **Firebase Analytics events:** retained per Firebase's default retention (currently up to 14 months for event-level data) unless you request deletion.
- **Crashlytics records:** retained per Firebase's policy (currently 90 days for unprocessed crashes, longer for issue records).
- **Purchase confirmations:** held by Apple / Google per their policies; we keep only the local "is premium" flag.

## Security

Local data sits in the OS's app sandbox, which is encrypted at rest on both iOS and Android by default once the device passcode is set. Network calls to Firebase and AdMob go over TLS. Because the app has no server and no account, there is no remote credential to steal.

## International transfers

If you use the app outside the United States, the analytics and crash data described above is processed by Google on infrastructure that may be located in the United States or other countries where Google operates. Google maintains Standard Contractual Clauses and adequacy mechanisms as described in its own privacy documentation.

## Changes to this policy

If we change what the app collects — for example, if we ever add cloud sync, accounts, or location features — we will update this document and bump the **Last updated** date at the top. Material changes will also be flagged inside the app on first launch after the change.

## Contact

- Issues and questions: https://github.com/amoghsa/Stockwise/issues
- Privacy contact: amogh.agrawal@boomi.com
