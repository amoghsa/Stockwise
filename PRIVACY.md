# StockWise Privacy Policy

**Effective date:** 2026-05-08
**Last updated:** 2026-05-13

StockWise ("we", "us", "the app") is an offline-first grocery and pantry planning app for iOS and Android. This policy explains what data the app handles, where it lives, and the choices you have. We've tried to keep it short and free of legalese — if anything is unclear, open an issue at https://github.com/amoghsa/Stockwise/issues.

## Summary, in one paragraph

Your shopping data — items, lists, pantry stock, events, purchase history, prices and budgets, household size — stays on your device. We do not run a backend that stores it, and we do not sync it to the cloud. The app uses Google Firebase (Analytics, Crashlytics, Performance Monitoring) to understand how the app is used and to find crashes, and Google AdMob to show ads in the free tier. **Analytics is off by default** — nothing is sent to Firebase until you flip the toggle in Settings → Data & Privacy → Help Improve StockWise. While that toggle is off, AdMob ads are also fully suppressed, so the free tier behaves like a fully offline build until you opt in. The app has an optional **voice quick-add** feature; if you use it, audio is processed by your device's speech-recognition service (on-device when supported by the OS) and only the resulting text reaches the app. You can clear pantry tracking with **Reset Pantry Activity** in Settings, or uninstall the app to remove every locally stored record.

## What stays only on your device

The following is stored locally (Apple SwiftData on iOS, Room on Android) and never sent to a server we operate:

- Grocery items, categories, and the lists you build
- Pantry inventory state (in stock / running low / out)
- Purchase history and consumption-rate estimates the app derives from it
- **Prices, receipt totals, tax amounts, and any monthly / weekly / per-category budget caps** you choose to track, plus your selected currency code
- Events you create (parties, trips) and any guest counts you enter
- Household composition you set during onboarding (number of adults, number of children — counts only, no names or identities) and the optional household name
- Notification preferences and shopping-frequency settings
- Recommendation feedback (whether you accepted or dismissed a suggestion)
- **Free-text fields you enter** — item names, item notes, custom item names, list names, event titles and notes, and the optional store label you can attach to a shopping trip
- A handful of local app preferences (e.g., whether you've reviewed the notification permission prompt, the expiry timestamp of a rewarded-ad Insights unlock, your starter-item picks during onboarding)

The app currently has no account system and no cross-device sync. To clear pantry activity (stock states, purchase history, recommendation feedback, shopping sessions) while keeping your catalog, household profile, stores, and budgets, use **Settings → Reset Pantry Activity**. To remove every locally stored record, uninstall the app.

## What is sent off the device, and to whom

### Google Firebase — Analytics, Crashlytics, Performance Monitoring

Used (when you opt in) to measure activation, retention, feature usage, crashes, and slow screens. The full list of events the app emits is published in the [analytics events doc](https://github.com/amoghsa/Stockwise/blob/main/docs/analytics-events.md) in the source repository. Examples: onboarding completion, screen views, suggestion-acceptance and dismissal counts, stock-state changes, shopping-session start/save/complete, ad impressions and clicks, paywall views, rewarded-ad views, purchase success/failure, analytics-consent changes, and data-deletion requests.

Along with each event, Firebase receives the standard SDK context: a Firebase-generated install ID (an opaque random identifier, not your name or email), device model, OS version, app version, language, country (derived from IP — the IP itself is not retained by Firebase Analytics for advertising), and a coarse session timestamp. We attach a small set of anonymous **user properties**: `is_premium`, `household_size` (a small bucket, e.g. "3"), `account_type` (currently always `"guest"` — there are no accounts today), `app_theme`, and `analytics_consent`.

We do **not** send: your item names, list contents, pantry contents, event titles, the contents of any free-text field, your precise location, or your device advertising identifier via Analytics. Where an analytics event references a category, it sends the fixed taxonomy value (e.g., "Produce", "Dairy", "Vacation/Away") — never the user-entered name. The one exception is `purchase_failed`, which includes a localized StoreKit error reason supplied by the operating system (not user content).

Crashlytics may include a stack trace, the device state at the moment of the crash, and a Crashlytics-generated installation UUID. To make crashes interpretable, Crashlytics also receives the same anonymous user properties listed above (premium status, household-size bucket, theme, consent flag) as custom keys on each crash report. Performance Monitoring reports timing for app startup, screen rendering, and network calls.

You can turn all of the above off with **Settings → Data & Privacy → Help Improve StockWise**. When the toggle is off, Analytics collection is disabled (`setAnalyticsCollectionEnabled(false)`), Firebase's ad-storage / ad-user-data / ad-personalization consent flags are all set to `denied`, and existing locally-buffered events that have not yet been uploaded are dropped. **Turning this toggle off also fully suppresses AdMob** — no ads will be requested or shown while consent is off, even on the free tier.

Firebase's own privacy documentation: https://firebase.google.com/support/privacy

### Google AdMob — advertising (free-tier users who have opted in to analytics)

Free-tier users who have turned on **Help Improve StockWise** see occasional native ads on Home and Pantry, and can opt into a rewarded video ad to unlock the Insights screen for 24 hours. Ads are served by Google AdMob. (If the analytics toggle is off, no ads load at all.)

Before AdMob requests an ad, the app shows an in-app explainer and then the iOS App Tracking Transparency (ATT) system prompt — both presented contextually the first time an ad surface appears, not at launch. Your answer to the ATT prompt determines whether AdMob receives your device's advertising identifier:

- **If you allow tracking:** AdMob may receive your iOS IDFA (or Android Advertising ID) for ad measurement and personalization.
- **If you decline tracking:** ads are served in non-personalized mode and the IDFA is not provided.

In addition to the IDFA (when granted), the AdMob SDK collects information described in Google's mobile-ads SDK disclosure, which can include coarse (city / region-level) location derived from your IP address, device type, OS, network info, and the ad unit being filled. StockWise does not request or use device GPS for ads or for any other purpose.

We do not pass any of your shopping data to AdMob. We send AdMob the ad unit identifier and the SDK's standard request payload — nothing else.

AdMob privacy & data disclosures: https://support.google.com/admob/answer/6128543

If you'd rather not see ads at all, the **Remove Ads** one-time in-app purchase disables every ad surface and the rewarded prompts.

### Apple App Store / Google Play — purchases

The **Remove Ads** purchase (a single non-consumable, the only IAP the app sells today) is processed by Apple StoreKit on iOS and Google Play Billing on Android. We never see your card number; we receive only a confirmation that the purchase succeeded and a product identifier (e.g. `com.amoghagrawal.stockwise.removeads`), which we store on device to suppress ads. Apple and Google's own privacy policies cover the payment leg.

## Voice quick-add (microphone)

The Home tab includes an optional **voice quick-add** button (small mic icon next to the text input). The first time you tap it, the operating system asks you to grant microphone permission; if you decline, the feature is disabled and the rest of the app works normally.

When you use it, StockWise calls the Android `SpeechRecognizer` API:

- On Android 12 (API 31) and newer, the app first checks whether **on-device speech recognition** is available (`SpeechRecognizer.createOnDeviceSpeechRecognizer`). When it is, audio is transcribed locally on your device and never leaves it.
- When on-device recognition is unavailable, the app falls back to the standard `SpeechRecognizer` flow with `EXTRA_PREFER_OFFLINE = true`. On devices configured to use Google's speech service, audio may be sent to Google Speech Services for transcription per Google's terms; on other devices it is handled by whichever speech-recognition app the OS routes to. Refer to Google's privacy policy at https://policies.google.com/privacy for details on Google Speech Services.

StockWise itself never writes the audio stream to disk, never uploads it to a backend we operate, and never shares it with third parties beyond the OS speech-recognition service described above. Only the **transcribed text string** is returned to the app, where it is parsed to extract an item name and quantity and then handled like any other entry on the quick-add bar.

The microphone is engaged **only while you hold or tap the voice button on Home**. Closing the dialog, navigating away, or denying the runtime permission all release the microphone immediately.

## Notifications

StockWise sends reminders for: a weekly "time to plan your shop" prompt, low-stock follow-ups the day after you mark items out, event-prep reminders the day before an event you've added, and a one-shot travel-reduction notice when a vacation event begins.

**All reminders are scheduled locally on your device.** StockWise does not run a push-notification server, has no remote message channel, and never sends notification content (event titles, item names, etc.) off the device. Before the iOS system permission dialog appears, the app shows an in-app explainer so you know what the reminders are for; you can decline either prompt and use the app without notifications.

## Exporting your data

Settings → Data & Privacy → **Export My Data** builds a JSON file containing a snapshot of every model the app stores (items, lists, pantry, events, purchase history, prices, budgets, household name, store labels, notes). The file is handed to the iOS share sheet so you control where it goes — Files, Mail, AirDrop, iMessage, etc. StockWise itself does not upload the export anywhere. There is no companion import flow at this time, so the export is best thought of as a personal backup format for your own use.

## What the app does **not** collect

To be explicit, because some of these are common in similar apps:

- **No precise location.** The app does not request foreground or background location permission, does not track your in-store movement, and does not build maps from your phone. (The product spec includes a forward-looking idea around aisle-based routing — that feature is not implemented today, and if it ever ships it will be opt-in and described in an updated version of this policy before launch.)
- **No contacts, photos, calendar, or health data.** Microphone access is requested only for the optional voice quick-add feature described above, and only while that feature is actively in use.
- **No barcode or receipt scanning** in the current release.
- **No account, no email, no password, no name.** The app runs as a local "Guest" profile.
- **No third-party analytics besides Firebase.** No Facebook SDK, no Mixpanel, no Segment.

## Children

StockWise is a household utility intended for adults. The "children" count in household setup is just a number used by the recommendation engine to scale quantities — it is not a profile of a child and is not associated with any identity. We do not knowingly collect personal information from anyone under the age of 13 (or under 16 in jurisdictions where that threshold applies), and the app provides no mechanism for a child to create an account because there are no accounts at all.

## Your choices and rights

- **Turn off analytics:** Settings → Data & Privacy → Help Improve StockWise.
- **Reset advertising identifier:** iOS Settings → Privacy & Security → Tracking; or Android Settings → Privacy → Ads.
- **Reset pantry activity:** Settings → Reset Pantry Activity. Clears stock states, purchase history, recommendation feedback, shopping sessions, and active grocery lists while preserving your catalog, household profile, stores, and budgets. This also calls the analytics SDK's reset so the Firebase install ID and user properties are regenerated going forward.
- **Remove everything:** uninstall the app. This deletes the local database in its entirety. Any analytics already received by Firebase before uninstall is retained per Firebase's own retention policy unless you separately request deletion (see below).
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
