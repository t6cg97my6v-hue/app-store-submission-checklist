# app-store-submission-checklist
Free iOS App Store submission and rejection prevention checklist for Expo and React Native developers.
This open-source checklist is maintained by AppCheckHQ, an App Store readiness scanner for Expo and React Native projects. It condenses the [AppCheckHQ App Store Submission Checklist]

((https://appcheckhq.com/guides/expo-app-store-checklist)) and [React Native Security Checklist]
(https://appcheckhq.com/guides/react-native-app-store-security-checklist) into a practical GitHub checklist you can work through before submitting.

App Store Submission Checklist for Expo & React Native
A free, practical checklist for developers preparing an iOS app for App Review. Covers release setup, permissions, privacy, security, purchases, reviewer access and production testing.

Based on the AppCheckHQ submission guide and security guide, both inspected on 12 October 2026 and marked updated October 2026. This is a condensed, paraphrased resource; each section links to its source.

A successful build or TestFlight test does not establish submission readiness. Completing this checklist does not guarantee Apple approval or prove that an app has no vulnerabilities. Requirements change: consult the current documentation relevant to your app.

How to use this checklist
Copy it into your release notes or a GitHub issue, work through the sections that apply, and record why any conditional check does not apply.

Project review means inspecting configuration, dependencies or code. Static tools can help identify patterns, but you still need to resolve findings.
Manual verification means checking the built app, a device, a backend or App Store Connect.
1. Release configuration
Project review


Use your own reverse-domain iOS bundle identifier rather than a placeholder.

Set an app version and increase the build number for each upload, manually or through EAS version management.

Keep the production EAS profile free of development-client, internal-distribution and simulator settings.

Check that production code does not import expo-dev-client.

If using EAS Update, configure runtimeVersion for compatible updates.

Check Expo and React Native dependency compatibility with npx expo install --check.

Configure the app icon and splash screen; the submission guide specifies a 1024 × 1024 PNG icon.

Remove production references to local development endpoints and unguarded debug or mock flags.
Manual verification


Match the bundle identifier to the Apple Developer App ID and App Store Connect record.

Set production endpoints and public configuration through the intended build profile or EAS environment.

Confirm the SDK can build with Apple's currently required Xcode version.
Source: Release configuration.

2. Permissions
Project review


Provide the relevant iOS usage descriptions for installed packages that access protected resources.

Inspect config plugins as well as your own configuration: plugins can generate or override native permissions.

Remove permissions for features the app no longer provides.

Request background location only when a core feature needs it.

If using push notifications, configure the expo-notifications plugin.
Manual verification


Explain the actual feature in each permission description, using clear language.

Request access when the user reaches the relevant feature.

Test denial of each permission and confirm the app handles it usefully.

Inspect the generated native configuration or built app to confirm the intended descriptions ship.
Sources: Submission permissions, Security permissions.

3. Secrets, storage and network security
Project review


Keep privileged credentials out of client code, configuration and the generated bundle, including payment secrets, service-role keys, private keys and administrative credentials.

Treat EXPO_PUBLIC_ values as public configuration, never as a place for private credentials.

Move privileged operations to an authenticated server-side environment.

Use appropriate secure storage for sensitive credentials; review use of expo-secure-store instead of ordinary storage.

Remove sensitive values from logs and avoid placing authentication tokens in deep-link URLs.

Use HTTPS and review App Transport Security exceptions; do not disable ATS globally.
Manual verification


Inspect the built bundle for sensitive values and rotate credentials if they were exposed.

Review backend authentication, authorisation and database access rules separately from the mobile interface.
Public client keys may be intended for inclusion in an app. Assess the privileges a credential grants rather than assuming every API key is secret.

Sources: Release configuration, Authentication, API secrets, Network and storage.

4. Privacy disclosures and manifests
Project review


Inventory SDKs and backend services that process user data, including analytics, crash reporting, attribution and advertising.

Review native dependencies for applicable privacy-manifest and SDK-signature requirements.

Check required-reason API declarations and choose approved reasons that reflect actual use.

For Expo, review expo.ios.privacyManifests where applicable.
Manual verification


Publish a reachable privacy policy and enter its URL in App Store Connect.

Compare App Privacy answers with provider documentation and actual production data collection and network behaviour.

Revisit disclosures when dependencies or data use change.

Inspect the built app and available Xcode privacy reports, and resolve App Store Connect validation warnings or errors.
Manifest requirements depend on the APIs and SDKs included; copying another app's declarations is not a substitute for checking your own dependencies.

Sources: Submission privacy, Privacy disclosures, Privacy manifests.

5. Tracking and advertising — if applicable
Project review


If requesting ATT permission, configure NSUserTrackingUsageDescription.

Replace production advertising test identifiers and configure the AdMob app ID if using AdMob.
Manual verification


Determine whether the app or its SDKs track users under Apple's definition.

Where ATT is required, obtain permission before tracking and respect denial.

Test both allowed and denied consent states against actual SDK behaviour.

Align tracking disclosures with the production implementation.
An advertising SDK alone does not automatically require ATT; actual tracking behaviour matters. Displaying a prompt alone does not establish compliance.

Sources: Advertising, App Tracking Transparency, Monetisation SDKs.

6. Accounts and sign-in — if applicable
Manual verification


If users can create accounts, allow them to start account deletion within the app.

If offering social or third-party sign-in, assess guideline 4.8 and provide an equivalent qualifying option unless an exemption applies. Sign in with Apple is a common approach.

Test registration, sign-in, sign-out and account deletion end to end.
Sources: Authentication and account deletion, Production testing.

7. Purchases and subscriptions — if applicable
Project review


Review digital-content payment flows against Apple's applicable in-app purchase rules. The guide notes that digital goods and subscriptions generally require Apple's purchase system.

Provide a visible restore action for subscriptions or non-consumable purchases.

Determine premium access from entitlements and handle lookup failures.

Handle purchase cancellation and errors without leaving the interface stuck.

If using RevenueCat, check the intended platform public key rather than a Test Store or secret key.
Manual verification


Confirm products are ready for submission and attached to the version being reviewed in App Store Connect.

Show subscription price, billing period, terms and privacy-policy links on the paywall.

Test sandbox purchasing, cancellation and restoration on a physical device.
Source: In-app purchases and subscriptions.

8. Store listing and reviewer access
Project review


Review ios.config.usesNonExemptEncryption and the linked export-compliance guidance for your app.
Manual verification


Provide screenshots for required device sizes that show the current interface.

Make the description, keywords and promotional text reflect the app's actual features.

Confirm support and privacy-policy URLs work.

Complete the age-rating questionnaire.

For sign-in apps, provide a working reviewer demo account with required data already populated in App Review Information.

Explain unusual features, hardware dependencies and purchase locations in review notes.

Keep required backends and feature flags available to the reviewer.
Sources: App Store Connect metadata, Reviewer access.

9. Test the build you will submit
Manual verification


Install the production build through TestFlight on a physical device.

Test a cold start without cached data or a running development server.

Check offline behaviour and error states.

If supported, confirm notifications and deep links reach the intended screen.

Test the production advertising and payment configuration.

Attach the same build you tested to the submission.

Resolve applicable checklist items or record a reason for leaving them.

Read the current App Review Guidelines sections relevant to your app's category.
Sources: Production build checks, Final check, Monetisation SDKs.

Built with AI?
Apply the same checklist. The guide highlights dependency and plugin changes, leftover test keys or demo credentials, unused permissions and incomplete purchase restoration as things to inspect in fast AI-assisted development. Review package.json, app configuration, eas.json, environment files and paywall code.

Source: Built your app with AI?.

Optional automated check
You can complete this resource manually. AppCheckHQ also offers a browser-based static scan of supported Expo and React Native project files. According to the submission guide, source analysis runs locally in the browser. A scan can flag supported patterns; App Store Connect settings, privacy answers, product approval, reviewer access and runtime behaviour still need manual verification.

AppCheck is a readiness tool, not a replacement for Apple review or a professional security audit.

Sources: What AppCheck checks, Scanner limitations.

Sources and further reading
This README summarises the two articles listed on the AppCheckHQ guides page when inspected on 12 October 2026:

Expo App Store Submission Checklist
React Native App Store Security Checklist
The guides link to these official references. They are provided for checking current requirements, rather than as separately verified sources for this summary:

Apple App Review Guidelines
Apple App Privacy details
Apple account deletion guidance
Apple third-party SDK requirements
Apple privacy manifests
Apple required-reason APIs
Expo permissions
Expo privacy manifests
Expo environment variables
Expo iOS submission
Independent resource; AppCheck states that it is not affiliated with Apple, Google or Expo.
