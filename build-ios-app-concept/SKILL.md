---
name: build-ios-app-concept
description: Create, implement, build, launch, and verify a programmatic UIKit iPhone application from an explicitly approved AppSpec and visual design. Use only after the idea, feature-specific permission copy, permission-review matrix, data inventory, screens, seamless startup/loading experience, permission UX, visual system, and app icon are approved. Build the smallest permission-centered iOS 15 application with all required permissions, configurable privacy-aware AppMetrica, low mutable static state, Lock Screen widget, notification service/content extensions, Privacy Manifest and entitlement validation, and a minimal Yandex Cloud backend with one version-resolved cloudSyncEnabled feature flag. Exclude iPad, Mac, Mac Catalyst, Designed for iPhone/iPad on Mac, visionOS, Apple Vision, storyboards, XIBs, and test targets.
---

# Build iOS App Concept

## Goal

Implement an explicitly design-approved `AppSpec.md` as the smallest working portrait-only iPhone application. Treat the specification and approved design artifacts as the source of truth. Exercise every required authorization through a real, reviewable function and write only the supporting navigation, state, configuration, analytics, privacy, extensions, networking, error handling, and verification code needed to make those functions work.

Do not redesign the product, silently rewrite permission copy, or add unapproved features. Minimize total source files, types, lines, and long-lived state without obscuring resource ownership. This skill verifies local implementation quality but does not replace the later App Store preparation workflow.

## Required handoff

Require:

- `AppSpec.md` with `idea-approved` and `design-status: approved`, or equivalent explicit statuses recorded in the file.
- Approved portrait mockups, screen inventory, seamless startup appearance and timing, permission-to-screen mapping, visual system, accessibility behavior, widget and notification layouts, and app-icon source.
- For optional user-supplied widget code, approved active and inactive states for every affected Home Screen, Lock Screen, and iOS 18 Control Widget family, the exact `cloudSyncEnabled` mapping, tap deep links, accessibility labels, compile-condition name, and an honest split between scaffolded work and deferred source integration.
- Exact feature-specific permission and pre-permission copy for all required authorization categories.
- Approved permission-review matrix, data inventory, AppMetrica and ATT behavior, extension roles, and Yandex Cloud backend contract with the single version-resolved `cloudSyncEnabled` flag.
- Destination parent directory or an existing project path.
- Product/module name and approved lowercase ASCII product slug. Derive the main identifier as `com.idev.<product-slug>` rather than accepting another default prefix.

If idea or design approval is missing, stop and propose `generate-ios-app-ideas` or `design-ios-app-concept` as appropriate. Do not perform the missing approval inside this implementation skill.

Before editing, compare the specification, mockups, permission matrix, data inventory, backend contract, and extension roles. Report contradictions instead of resolving them silently. Preserve accepted decisions unless a concrete implementation, platform, privacy, or review conflict requires renewed user approval.

For an incremental change to an existing application, require explicit approval of the affected rendered screen or state before editing. Do not treat a general request to implement as approval of an unseen visual design, and do not backfill design approval after implementation. Return the delta to `design-ios-app-concept` and stop when that approval is missing.

Ask only for missing blocking inputs. Ask the user to create or select a distinct AppMetrica application for the final bundle identifier and provide its SDK API key; treat a missing key as non-blocking `pending` during initial implementation. Never invent a key or silently reuse one from another application. Request Apple Developer Team details only when device signing or capabilities require them. Reuse an authenticated Yandex Cloud account and explicitly selected cloud/folder when available; otherwise create deployable Yandex Cloud sources and mark deployment credentials as `pending` without silently substituting another provider. Treat privacy-policy, support, Terms, and EULA values as non-blocking `pending` items for the App Store preparation workflow.

Do not require the user to create an empty Xcode project. Create the directory and targets after inspecting the destination. Never overwrite a non-empty directory, existing project, signing settings, or user-owned files without resolving the ambiguity first.

## Permission implementation contract

Implement all 10 authorization categories through visible actions. One coherent feature may cover several permissions; do not create ten disconnected demo buttons. Use the exact approved purpose copy and trigger paths from `AppSpec.md`.

| Capability | Framework and request | Required configuration | Minimum real behavior |
|---|---|---|---|
| Bluetooth | CoreBluetooth with an instance-owned retained `CBCentralManager` | `NSBluetoothAlwaysUsageDescription` | Scan for compatible peripherals and expose an honest concept-specific result; connect or disconnect when the accepted hardware flow supports it |
| Camera | AVFoundation and `AVCaptureDevice.requestAccess(for: .video)` | `NSCameraUsageDescription` | Capture a photo, video, or scan used by the accepted feature |
| Contacts | Contacts with an instance-owned `CNContactStore` | `NSContactsUsageDescription` | Use authorized contact-store data for the accepted feature; a picker-only flow does not justify full Contacts authorization |
| Face ID | LocalAuthentication with an on-demand `LAContext` | `NSFaceIDUsageDescription` | Protect or confirm a meaningful action with passcode and unavailable handling |
| Location | CoreLocation with an instance-owned `CLLocationManager` and when-in-use request | `NSLocationWhenInUseUsageDescription` | Use current location for the accepted nearby or current-location result |
| Microphone | AVFAudio or AVFoundation with an iOS-15-compatible record-permission API | `NSMicrophoneUsageDescription` | Record, stop, save, play, replace, and delete or otherwise use a short clip with visible state |
| Photo-library read | PhotoKit `.readWrite` authorization | `NSPhotoLibraryUsageDescription` | Directly browse or process authorized library content; `PHPicker` alone does not justify full read access |
| Photo-library add | PhotoKit `.addOnly` authorization | `NSPhotoLibraryAddUsageDescription` | Save generated or captured media and show an explicit standard UIKit success or failure alert |
| Advertising tracking | AppTrackingTransparency after the approved explanatory action | `NSUserTrackingUsageDescription` | Gate the approved advertising, attribution, or personalization behavior and keep the app useful when denied |
| Push notifications | UserNotifications authorization with approved options | No usage-description key | Show the approved explanation, notification preferences, and a concept-relevant local or remote notification flow |

Apply these rules:

- Request access only after the user selects its visible feature; never request all permissions at launch.
- Put static purpose strings in the processed Info.plist and localized `InfoPlist.strings`; do not attempt to replace them at runtime.
- Handle `notDetermined`, authorized or limited, denied, restricted, unavailable, and unknown states as applicable. After denial, explain the disabled behavior and offer a deliberate Settings action without pressure or loops.
- Keep controls and status synchronized for microphone recording, saving, saved, playback, replace, and delete states.
- Use direct PhotoKit access only where the approved concept genuinely requires the corresponding read or add authorization.
- Never upload contacts, precise location, photos, recordings, or identifiers merely to demonstrate a permission.
- Keep the runtime behavior, purpose copy, data inventory, ATT state, AppMetrica modules, backend payloads, privacy declarations, and reviewer instructions mutually consistent.
- Return to specification approval if a privacy-preserving picker fully satisfies the feature and makes a required direct-access permission unjustifiable.

Treat notification authorization and APNs registration as separate lifecycle steps:

- After the user grants notification access, call `UIApplication.registerForRemoteNotifications()` on the main thread immediately; do not wait for another launch.
- On every cold process launch, query `UNUserNotificationCenter.getNotificationSettings`. Re-register with APNs for `.authorized`, `.provisional`, or `.ephemeral`; do nothing for `.notDetermined` or `.denied`. This startup check must never present the authorization prompt.
- Treat `didRegisterForRemoteNotificationsWithDeviceToken` as repeatable. Convert and forward every latest token to the approved synchronization path; never skip APNs registration because a cached token exists. Keep server transmission subject to the approved data inventory and `cloudSyncEnabled` behavior.

## Workflow

### Phase 1: Validate the handoff

Create a compact implementation checklist from `AppSpec.md`: feature and permission mappings, exact copy, denial behavior, data flow, screen placement, extension roles, Yandex Cloud payloads and resource reuse, `cloudSyncEnabled` behavior, analytics and tracking behavior, app icon, platform settings, and pending external credentials. For optional imported widget code, separately mark approved design, compile scaffold, shared-flag propagation, imported source, deep-link destination, and device verification as complete or pending. Resolve only blocking contradictions with the user.

### Phase 2: Create the project

Use repository instructions and its existing project generator when present. Otherwise prefer an installed generator such as XcodeGen or Tuist; if none exists, use available local tooling. Do not install tools or dependencies outside normal project resolution without approval.

Create:

- Main programmatic UIKit Swift target, iPhone iOS 15.0.
- Lock Screen WidgetKit extension, iOS 16.0 because Lock Screen families are unavailable on iOS 15.
- Notification Service Extension, iOS 15.0.
- Programmatic Notification Content Extension, iOS 15.0.
- Shared App Group only where the approved app and widget exchange a compact snapshot.

Use this identifier family exactly:

```text
Main app:                    com.idev.<product-slug>
Lock Screen widget:          com.idev.<product-slug>.widget
Notification service:        com.idev.<product-slug>.notification-service
Notification content:        com.idev.<product-slug>.notification-content
App Group:                    group.com.idev.<product-slug>
```

For every target set the supported Apple platforms as narrowly as the generator permits:

```text
TARGETED_DEVICE_FAMILY = 1
SUPPORTED_PLATFORMS = iphoneos iphonesimulator
SUPPORTS_MACCATALYST = NO
SUPPORTS_MAC_DESIGNED_FOR_IPHONE_IPAD = NO
SUPPORTS_XR_DESIGNED_FOR_IPHONE_IPAD = NO
```

Restrict the main Info.plist to `UIInterfaceOrientationPortrait`. Do not add iPad-specific orientation keys, landscape or upside-down modes, Mac, Catalyst, visionOS, Apple Vision, watchOS, tvOS, or companion targets.

Configure the required static iOS launch representation through `UILaunchScreen` in Info.plist, using the approved background and optional approved artwork. Do not use a launch storyboard. Put shared launch artwork in one ordinary imageset with appropriate 1x, 2x, and 3x or vector representations, and use that same named asset in both `UILaunchScreen` and the programmatic startup view. Do not try to load `AppIcon.appiconset` as a normal runtime image. When the approved startup design reuses the app icon, derive the launch imageset from the same approved source without redrawing the artwork; keep the original AppIcon source unmasked and apply transparent corner treatment only to the separate launch copy when explicitly approved. Make both stages visually identical so the user perceives one continuous startup screen.

Create all application and extension UI programmatically. Do not add storyboard or XIB files, `UIMainStoryboardFile`, scene storyboard names, or `NSExtensionMainStoryboard`. Prefer `NSExtensionPrincipalClass` for the notification content extension; if the active toolchain makes the approved programmatic approach impossible, stop and report the conflict.

Do not create unit-test or UI-test targets, test files, fixtures, mocks, or test-only infrastructure. Do not run unit or UI tests. Verification consists of builds, launch, backend smoke checks, and direct inspection of the real flows.

Create explicit entitlement files for capabilities the code uses. When a generator owns the project, declare entitlement values in its source specification, such as `project.yml` under `entitlements.properties`; regenerate and inspect emitted files instead of patching only generated output. Configure matching App Group values for the app and widget and Push Notifications for the main app where required. Treat provisioning profiles and signed entitlements as unverified until signing assets are available.

Add `PrivacyInfo.xcprivacy` to every shipped app-owned target that directly uses required-reason APIs. Declare applicable standard `UserDefaults` and App Group reasons; a widget using App Group defaults needs its own manifest with the applicable reason such as `1C8F.1`. Inspect resolved SDK privacy manifests rather than assuming the main app or a dependency covers every target.

Include the approved square 1024×1024 icon source in an `AppIcon` asset set and configure the main target to use it. Do not substitute an SF Symbol, add small text, pre-round, or mask the source.

### Phase 3: Implement the approved application

Default to simple MVC. Prefer one feature-oriented `UIViewController` and extract only instance-owned resource adapters or deterministic helpers that materially reduce complexity, such as `AppConfiguration`, `APIClient`, `AppMetricaAnalyticsClient`, permission framework adapters, Codable models, and pure formatters.

Create one composition root in `SceneDelegate` or an instance-owned `AppFactory` and inject dependencies through initializers. Avoid repositories, use-case layers, coordinators, reactive state stores, service locators, protocol-per-type abstractions, and dependency containers unless the approved design truly needs them.

Implement the approved visual system with programmatic Auto Layout, Dynamic Type text styles, semantic or explicitly approved colors, accessible UIKit controls, deliberate VoiceOver grouping and order, and non-color status cues. Match the approved screen inventory and states; do not interpret supporting code as permission to broaden the MVP.

Use the approved programmatic startup view as the initial root while required local initialization and the `cloudSyncEnabled` request run concurrently. Keep the same background, named artwork asset, sizing, and placement as `UILaunchScreen`; do not add permissions, ATT prompts, advertisements, retry controls, or unrelated onboarding. Replace it with the main interface without an app-controlled fade or other animation unless that transition was explicitly approved. Show it once per cold process launch, not on foreground resume.

Default the app-controlled interval from the first programmatic startup frame to a 0.7-second minimum and a 4-second maximum, or use the explicitly approved values from `AppSpec.md`. Apply this deterministic gate:

- If initialization and the flag response finish before the minimum, retain the startup view until the minimum expires.
- If they finish between the minimum and maximum, reveal the main interface immediately.
- If the deadline expires first, stop waiting, use the last flag cached for the current app version or `true` before that version's first successful fetch, and reveal the main interface. Do not add launch retries or keep the main interface blocked.

Do not cancel the single in-flight configuration request solely because the startup deadline expires. Let it finish within its ordinary request timeout; on a later success, update the current-version cache, effective flag, and already approved main-screen status without blocking or re-presenting startup. Do not add polling, a second request, or a new UI state for this case.

Keep the gate instance-owned and completion-driven. Avoid observers, global state, polling, and artificial work. Treat the system-controlled time before the programmatic first frame as outside the app-controlled interval.

### Phase 4: Build, launch, and verify

Run the verification section below. Fix implementation drift and build or launch failures without adding product scope.

### Phase 5: Record the handoff

Update the release handoff manifest and report verified behavior, configuration instructions, and external limitations. Do not claim App Store submission readiness while policy, production backend, signing, APNs, or device-only items remain pending.

## Architecture and lifetime rules

Minimize process-wide and long-lived state:

- Do not create app-owned singletons, global mutable variables, mutable `static var`, service locators, or global caches.
- Do not use `NotificationCenter` observers, KVO, Combine state pipelines, or permanent event buses for application state.
- Do not use `URLSession.shared`; inject a configured `URLSession`.
- Allow immutable `static let` constants only when they hold no runtime state.
- Isolate unavoidable framework global entry points such as `UNUserNotificationCenter.current()`, AppTrackingTransparency, or AppMetrica inside small injected adapters.
- Prefer delegates, target-action, scoped closures, and async functions. Capture owners weakly when callbacks can outlive them.
- Give Bluetooth, location, camera, audio, and other delegate-based resources an explicit instance owner and lifecycle. Stop scanning, recording, location updates, timers, and sessions when the feature ends.

Audit app-owned source for mutable static state, singleton declarations or `.shared` calls, observers, KVO, long-lived subscriptions, uncancelled timers, and delegate or closure lifetime leaks. Document required framework globals instead of disguising them. Enforce no unnecessary app-owned process state, not an impossible claim of zero static memory.

## Configuration and AppMetrica

Keep mutable environment choices outside Swift source, preferably in `.xcconfig` values exposed through build-setting substitution:

```text
PRODUCT_BUNDLE_IDENTIFIER
DEVELOPMENT_TEAM
APP_GROUP_IDENTIFIER
APPMETRICA_API_KEY
API_BASE_URL
```

Read values once into an injected immutable `AppConfiguration`. Never bundle APNs private keys, access tokens, or other server secrets.

Before integration, consult current official AppMetrica iOS documentation and package sources. Select and pin a Swift Package version supporting iOS 15 and use its current module and activation API.

- Register or select one AppMetrica application matching the final `com.idev.<product-slug>` identifier. Obtain its SDK API key from AppMetrica Settings; do not use a Post API key. One AppMetrica account may contain multiple applications, but each generated iPhone application must use its own explicitly supplied SDK key unless the user deliberately requests cross-application reporting.
- Store the supplied key only in the application's `.xcconfig` configuration and expose it to the processed Info.plist through build-setting substitution. Do not hardcode it in Swift, copy it into `AppSpec.md`, a release manifest, skill files, logs, or user-facing UI.
- Isolate SDK calls in an instance-owned `AppMetricaAnalyticsClient`; do not expose an application singleton.
- Make `APPMETRICA_API_KEY` replaceable without Swift changes. Treat every user-supplied startup or runtime SDK key as valid and pass it to AppMetrica unchanged: do not trim, normalize, prevalidate, add empty-key fallbacks, or log and recover from invalid-key failures.
- Activate AppMetrica once from the application composition root. Explicitly disable SDK location tracking when location is not part of the approved AppMetrica data inventory, and enable advertising-identifier tracking only after ATT authorization.
- Keep selected modules, data sending, advertising support, and ATT-dependent behavior consistent with `AppSpec.md` and current official SDK controls.
- Report only useful product events such as screen open, feature action, permission outcome, and API result. Pass `nil` for AppMetrica event failure callbacks; do not add `NSLog`, return statuses, or other app-owned handling for activation, reporter creation, or event-delivery errors. Do not add a synthetic app-launch event. Never send contacts, precise location, media, recordings, advertising identifiers, secrets, or other sensitive payloads as custom event parameters.
- Record SDK-level collection separately from custom event parameters and document that changing the API key may require terminating and relaunching the process.
- Implement runtime API-key switching only when the user explicitly requests it. Provide a minimal `changeAPIKey(_:)` method with no return status, obtain the AppMetrica reporter directly from the unchanged supplied key, route subsequent custom events through it, and manage that reporter's session manually. State that the main SDK's automatic data remains associated with the original activation until the next process launch. Persist an override in `UserDefaults` only when approved; do not add a settings screen solely for key switching.

## Extensions

Implement all approved extensions as real, small targets:

- Lock Screen widget: display the approved glanceable state. Share only a compact Codable snapshot through App Group `UserDefaults` or a small file. Do not observe shared defaults; reload timelines explicitly after writes. On iOS 17 and later, mark the widget with a removable `containerBackground(..., for: .widget)` and retain an availability-gated iOS 16 fallback. Use a clear container when the approved Lock Screen design has no custom background. Do not set `containerBackgroundRemovable(false)` merely to silence WidgetKit's development warning because it can exclude the widget from contexts that require removable backgrounds.
- Notification Service Extension: enrich or transform remote content as approved, keep networking optional and time-bounded, and always call the content handler with the best available content.
- Notification Content Extension: render the approved compact programmatic UI and route actions through notification categories or deep links.

When `AppSpec.md` approves optional user-supplied or copied widget code, prepare it without making the current build depend on absent source:

- Use neutral names such as `ImportedWidget` and `ImportedControlWidget`; never retain source-application names in new type names, compile settings, assets, URLs, or user-facing copy.
- Keep the application-owned widget configuration unconditionally present in the `WidgetBundle`. Put imported widget references behind the Swift active compilation condition `IMPORTED_WIDGET_AVAILABLE`, and leave that condition undefined until the imported files and every referenced type are present. The default project must therefore compile and build before any imported code is copied.
- Add an availability-gated iOS 18 `ControlWidget` in the same optional branch only when it was approved. Do not raise the deployment target of the iOS 16 Lock Screen widget extension merely to add it.
- Keep compile availability and runtime activation independent. WidgetKit does not dynamically unregister configurations: when imported code is compiled, keep both local and imported configurations registered and render the approved active or neutral inactive state according to the shared `cloudSyncEnabled` value.
- Persist the resolved flag and the application marketing version to the shared App Group after applying the startup default, cached value, or network result. Give widgets a deterministic fallback matching `AppSpec.md`; do not let extensions fetch configuration from the network, observe defaults, or poll.
- After a changed effective value is written, explicitly reload only the affected WidgetKit timelines or controls. Avoid unnecessary reloads when the persisted value has not changed.
- Follow the exact approved mapping. For the established two-widget convention, `cloudSyncEnabled == true` activates `ImportedWidget` and `ImportedControlWidget` while the local widget displays its inactive placeholder; `false` activates the local widget and makes both imported configurations inactive. Use another mapping only when `AppSpec.md` explicitly approves it.
- Implement the approved inactive placeholder in every supported family. Keep compact families icon-only when required, provide the full VoiceOver label, and make taps open the application. Preserve the exact approved active icon and copy; do not retain letters, logos, or payment wording when the accepted design uses a neutral QR scanner and the action text `Сканирование QR-кода`.
- Preserve stable app-owned deep links for deferred actions, including distinct widget and control origins when approved. The project must still build if their destination feature has not been copied; opening the application is sufficient until that feature receives its own approved design and implementation. Do not fabricate or copy the deferred feature as part of widget scaffolding.

Give targets distinct bundle identifiers and link only their required frameworks and resources. Do not link the complete application graph into an extension.

## Minimal backend and client

Create the approved minimal backend beside the app in `backend/` for Yandex Cloud. Prefer one Python Cloud Function source file plus a pinned `requirements.txt` and one OpenAPI specification. Use only the required YDB dependency; do not add a framework or local server implementation.

Use one reusable serverless stack:

- Yandex API Gateway exposing exactly `GET /health`, `GET /config?appId=...`, and `POST /sync` unless the user explicitly approves another endpoint.
- One Python Cloud Function that validates compact Codable-compatible JSON and handles configuration and synchronization.
- One Serverless YDB database with application data partitioned by `appId`; reuse an existing user-approved mobile backend stack when available.
- One narrowly scoped runtime service account without static keys in the repository. Keep cloud, folder, function, gateway, database, and service-account IDs in deployment documentation, not Swift source.

Implement exactly one remote Boolean flag named `cloudSyncEnabled`. Store one optional numeric `disabledFromVersion` threshold per `appId`. Require the client to send its non-empty `CFBundleShortVersionString` as `appVersion` to `/config`; do not support a versionless public read. Return `true` below the threshold, `false` at or above it, and `true` for every version when no threshold exists. Parse and compare one to three numeric components rather than comparing strings, so `1.10` is newer than `1.2`. Do not target `CFBundleVersion` unless the user explicitly requests build-level control. Expose only public read access through `/config`, and set or clear the threshold only through the authenticated Yandex Cloud console, CLI, or an IAM-protected direct function invocation. Do not expose public writes or add an in-app switch. A configuration revision is optional; omit it when unused and do not increment it merely because the threshold changes unless client cache logic requires that behavior.

Fetch the flag once from the programmatic startup view with an injected configured `URLSession`, following the approved minimum/maximum startup gate. Cache the last successful value under a key scoped by marketing version so upgrades and downgrades cannot reuse another version's value; use true before the first successful fetch for that version or when its startup deadline expires without a version-scoped cache. Check it before every `/sync`, including APNs-token synchronization. When false, keep local permission-backed functions, score/state, widget sharing, and Bluetooth behavior available and show the approved compact disabled status. Do not use observers, polling, streaming, or a feature-flag SDK.

Let `/sync` carry one compact idea-specific snapshot, an installation identifier, lightweight bearer value, and APNs device token when practical. Never transmit contacts, precise location, photos, recordings, ATT status, IDFA, or other data excluded by the approved inventory.

Before cloud mutations, inspect the active `yc` profile, cloud, and folder. Ask the user to authenticate or activate billing only when required. Prefer budget guardrails such as a `10 RPS` gateway limit, one function instance per zone, two concurrent requests per zone, and small Serverless YDB storage/throughput limits; report the chosen values. Keep a production HTTPS URL in both app configurations after successful deployment. If credentials are unavailable, leave the Yandex Cloud deployment honestly `pending` rather than presenting a local stub as production.

Document reproducible deploy commands, resource names and IDs, `/health`, `/config`, `/sync` examples, and authenticated commands for changing `cloudSyncEnabled`. Never store OAuth tokens, IAM keys, APNs `.p8`, or other secrets. When Apple signing or APNs credentials are unavailable, keep push delivery explicitly unverified.

## Privacy and future App Store handoff

Keep permission strings, runtime behavior, AppMetrica, ATT, backend payloads, `PrivacyInfo.xcprivacy`, and the data inventory consistent. Treat mismatches as implementation blockers.

Do not add privacy-policy, support, Terms, or EULA screens unless the user explicitly requests them. Record missing URLs and future in-app entry points as `pending` for the separate App Store preparation skill. Never claim submission readiness while these or production/signing requirements remain pending.

## Verification

Verify in proportion to the available environment:

1. Generate the project, resolve pinned package dependencies, and build the main app and every extension for an available simulator.
2. Build Release without app-owned compiler warnings. Do not suppress warnings merely to pass.
3. Uninstall and reinstall the app, then inspect the first cold launch and a repeated cold launch to expose launch-snapshot or asset-cache differences. Capture or otherwise inspect the static system stage, programmatic continuation, and final main interface separately; verify identical background, artwork, size and placement across the first two stages, the approved transition, minimum duration, response-driven completion, maximum deadline and cached/default fallback. Do not accept only the startup screen or only the main hierarchy as success, and do not create test targets or permanent debug controls for these checks.
4. Deploy or update the Yandex Cloud backend, call `/health`, exercise `/config` and one real `/sync` round trip, verify `cloudSyncEnabled` below, at, and above `disabledFromVersion`, include a comparison such as `1.10` versus `1.2`, verify the no-threshold default, confirm version-scoped client caching and rejected public writes, and leave the rule at the user-approved final state. Do not change a configuration revision unless the implementation actually uses it.
5. Search for storyboards, XIBs, test targets, app-owned singletons, mutable static storage, observers, `URLSession.shared`, and unsupported platform settings; fix violations or explain unavoidable framework calls.
6. Exercise every permission feature from its approved visible trigger through authorized or simulator-available behavior and denial recovery. Capture screenshots and the accessibility hierarchy before and after important interactions. Retry a failed simulator interaction once, then classify the limitation honestly. Do not create XCTest or UI-test targets.
7. Verify source, localized, and processed Info.plist purpose strings against `AppSpec.md` verbatim.
8. With the user-supplied SDK key, inspect the processed Info.plist without printing the key, verify activation and approved product events triggered by real screen or feature actions, and record AppMetrica-console receipt as user/account-dependent when console access is unavailable. Do not test empty or malformed keys, add failure logging, or introduce synthetic app-launch events.
9. Inspect each app-owned target's Privacy Manifest and resolved SDK manifests against actual APIs and the approved data inventory.
10. Inspect source entitlements and generator configuration for App Group and Push. Verify that granting notification access registers with APNs immediately, every authorized cold launch registers again without presenting a prompt, denied or undetermined startup states do not register, and every token callback forwards the latest token. When signing exists, verify the lifecycle on a physical device and inspect provisioning profiles and signed products; otherwise list the exact pending Apple Developer steps.
11. Verify the exact `com.idev.<product-slug>` main and extension identifiers, `group.com.idev.<product-slug>` App Group, distinct extension deployment targets, `TARGETED_DEVICE_FAMILY = 1`, iPhone-only supported platforms, disabled Mac/Catalyst/Apple Vision compatibility, and portrait-only processed main-app Info.plist.
12. Verify the approved AppIcon is compiled without missing-icon warnings and inspect it at full size and a small Home Screen-like size.
13. On a physical device running iOS 17 or later, add the Lock Screen widget and verify its real data updates, deep link, approved appearance, and absence of WidgetKit's `Please adopt containerBackground API` development overlay. State which Bluetooth, camera, contacts, Face ID, location, microphone, PhotoKit, ATT, remote push, signing, or other hardware checks still require a physical device or external credentials.
14. For optional imported widget integration, first verify the ordinary build with `IMPORTED_WIDGET_AVAILABLE` undefined and no imported source. After the user supplies that source, define the condition, build again, and verify the approved true and false mappings for the local widget, imported widget, and iOS 18 control on supported physical devices. Verify shared App Group persistence, explicit timeline or control reloads, inactive placeholders, exact icon and copy, accessibility labels, and deep-link origin values. Keep source integration and destination-feature behavior marked `pending` until each actually exists.
15. When signing is available, archive Release for Generic iOS Device and inspect signed app and extension entitlements. Otherwise record archive validation as signing-dependent.

Do not broaden the MVP while fixing verification failures.

## AppSpec conformance

Before handoff, compare the built application with every accepted item in `AppSpec.md` and the approved mockups.

For each authorization verify that the visible trigger exists on the approved screen, implemented behavior matches the approved feature, exact copy is present, denial and unavailable behavior matches the design, and accessed, stored, and transmitted data matches the inventory.

Compare screen structure, terminology, control behavior, visual states, accessibility, backend payloads, extension output, AppMetrica and ATT behavior, and the icon with the approved handoff. Do not mark implementation complete while an accepted item is missing or materially different. Fix implementation drift without adding unrelated features.

## Release handoff manifest

Create or update deterministic `Release/release-manifest.json` with:

- Product name, version, build number, and all bundle identifiers.
- App Group, capabilities, entitlement source, and verification status.
- Permission keys, exact localized copy, trigger paths, denial behavior, data handling, and reviewer instructions.
- Data inventory, tracking behavior, AppMetrica modules, and privacy status.
- Yandex Cloud resource identifiers, backend URL or `pending`, `/health`, version-aware `/config`, `/sync`, `cloudSyncEnabled` optional `disabledFromVersion` threshold and version-scoped cache behavior, and privacy-policy/support URLs or `pending` markers.
- Extension identifiers and roles.
- Optional imported-widget compile condition, approved local/imported runtime mapping, shared App Group keys, exact active and inactive presentation, deep links, and separate statuses for scaffold, imported source, destination feature, simulator verification, and physical-device verification.
- Deterministic application states intended for future App Store screenshots.
- Approved startup appearance, app-controlled minimum and maximum duration, initialization work, feature-flag deadline fallback, and cold-launch-only behavior.
- Device-only, production-backend, signing, APNs, hardware, policy, and archive checks that remain unresolved.

Never include AppMetrica API keys, APNs `.p8` files, access tokens, or other secrets.

## Handoff

Lead with what works. Provide clickable paths to the project, `AppSpec.md`, configuration, backend entry point, approved mockups, app-icon master, app-owned Privacy Manifests, permission localizations, and release manifest. Include exact build and backend-run commands, configurable values, verified targets, Release and archive status, AppSpec conformance, and remaining physical-device, production, policy, credential, or signing limitations.
