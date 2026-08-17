---
name: build-ios-app-concept
description: Design, render, obtain approval for, create, implement, and review-readiness verify an accepted portrait-only iPhone app concept with feature-specific permission copy, permission-review matrix, and data inventory. Use to build the smallest releaseable programmatic UIKit iOS 15 project with credible use of every required permission, HIG/accessibility checks, simple MVC, configurable privacy-aware AppMetrica, Privacy Manifest and entitlement validation, low static state, an App Icon, Lock Screen widget, notification service/content extensions, and minimal working backend. Exclude iPad, Mac, Mac Catalyst, Designed for iPhone/iPad on Mac, visionOS, and Apple Vision. Also use when creating the Xcode project and backend from scratch.
---

# Build iOS App Concept

## Goal

Turn an accepted product concept into the smallest working and releaseable portrait-only iPhone iOS application that exercises every required permission through a real, reviewable user action and includes an approved app icon. Keep product scope centered on those permission-backed functions. Write only the supporting navigation, state, models, configuration, analytics, privacy surfaces, extensions, networking, error handling, and verification code needed to make them work. Keep the application understandable, buildable, accessible, and easy to reconfigure. Stop at defined approval gates instead of making product or privacy decisions silently.

## Permission implementation

Before designing or implementing permission-backed features, use the exact feature-specific purpose text approved during ideation; do not silently rewrite it. Confirm that every string names the implemented feature and its user benefit. If copy is missing, generic, or inconsistent with behavior, stop before design and propose corrected copy for user approval.

Ensure that all 10 authorization categories are actually reachable and exercised. Let one coherent product function cover several permissions where natural; do not add ten isolated demo features solely to request them.

### Required feature mapping

| Capability | Framework and request | Required configuration | Minimum real feature |
|---|---|---|---|
| Bluetooth | CoreBluetooth; retain an instance-owned `CBCentralManager`, whose use triggers authorization | `NSBluetoothAlwaysUsageDescription` | Scan for and display nearby compatible peripherals; allow connect/disconnect when possible |
| Camera | AVFoundation; `AVCaptureDevice.requestAccess(for: .video)` | `NSCameraUsageDescription` | Capture a photo, video, or scan used by the accepted concept |
| Contacts | Contacts; request access through an instance-owned `CNContactStore` only when the approved feature needs direct store access | `NSContactsUsageDescription` | Use authorized contact data for the accepted feature; a picker-only flow does not justify full Contacts authorization |
| Face ID | LocalAuthentication; evaluate an `LAContext` policy on demand | `NSFaceIDUsageDescription` | Protect or confirm a meaningful local action, with passcode/unavailable handling |
| Location | CoreLocation; instance-owned `CLLocationManager.requestWhenInUseAuthorization()` | `NSLocationWhenInUseUsageDescription` | Show or use a current concept-relevant nearby/current-location result |
| Microphone | AVFAudio/AVFoundation; use an iOS-15-compatible record-permission API | `NSMicrophoneUsageDescription` | Record, stop, play, replace, and delete or use a short audio clip; keep the current recording/playback state visible |
| Photo-library read | PhotoKit; request `.readWrite` authorization only when the approved feature needs direct library access | `NSPhotoLibraryUsageDescription` | Directly browse or process authorized library content; a PHPicker-only flow does not justify full read access |
| Photo-library add | PhotoKit; request `.addOnly` authorization | `NSPhotoLibraryAddUsageDescription` | Save a generated or captured image/video and show a prominent standard UIKit success/failure confirmation |
| Advertising tracking | AppTrackingTransparency; request through `ATTrackingManager` after an explanatory user action | `NSUserTrackingUsageDescription` | Gate a real advertising, attribution, or personalization behavior; demonstrate the behavioral difference and keep the app useful when denied |
| Push notifications | UserNotifications; `UNUserNotificationCenter.current().requestAuthorization` with accepted options | No usage-description key; iOS owns the system alert copy | Show a pre-permission explanation, notification preferences, and a concept-relevant notification flow |

### Permission UX rules

- Request a permission only after the user selects the feature that needs it. Do not fire all prompts at launch.
- Display the accepted feature-specific purpose text on the system prompt where iOS supports it. Put localized variants in `InfoPlist.strings`; do not attempt to replace static purpose strings at runtime.
- Handle `notDetermined`, authorized/limited, denied, restricted, unavailable, and unknown states as applicable.
- After denial, explain the disabled feature and offer a deliberate Settings action. Do not loop or pressure the user.
- Keep the core app usable when optional permissions are denied.
- Keep microphone state visible next to its primary control: distinguish no recording, recording, saving, saved, and playback states; update the button title/action together with the status and keep replace/delete actions discoverable.
- After a PhotoKit add operation completes, show an explicit standard UIKit confirmation or error alert. Do not rely only on a transient status label or assume Photos provides a system success toast.
- Never upload contacts, precise location, media, recordings, or identifiers merely to demonstrate a permission.
- Use system pickers when they improve privacy, but note that PHPicker may not trigger full photo-library authorization. Because this product explicitly demonstrates the approved read permission, keep a separate direct-read feature when that authorization must be exercised.
- Preserve the approved permission-review matrix in the implementation: trigger path, benefit, copy, denial fallback, accessed data, transmission, privacy-label implication, and reviewer instruction must match the built behavior.
- If a privacy-preserving picker or API fully satisfies the accepted product behavior without a permission that the concept requires, return to specification approval and either make the direct-access feature genuinely necessary or report that the permission cannot be justified. Do not add a decorative prompt.

### Minimum screen strategy

Use the fewest screens that remain understandable. Prefer one main view controller with compact sections, sheets, alerts, and Apple-provided pickers or controllers. Add a separate screen only when the permission-backed interaction cannot stay clear on the main screen. Permission status, denied recovery, loading, and error states are supporting UI and may share the same views.

Combine related capabilities into coherent flows. Do not create ten disconnected demo buttons, onboarding, profile, feed, or history screen unless the accepted permission-centered MVP genuinely needs them. Do not add a privacy/support screen in this implementation workflow unless the user explicitly requests it; leave App Store-facing policy and support surfaces to the separate App Store preparation workflow.

## Inputs and project ownership

Start from the accepted idea, name, permission copy, permission-review matrix, data inventory, review-risk verdict, extension roles, and backend outline. Preserve decisions already approved by the user.

Implement only the portrait-oriented iPhone portion of the accepted idea. Adapt any landscape-dependent flow to portrait rather than retaining rotation support. Do not create or retain iPad, macOS, Mac Catalyst, Designed for iPhone/iPad on Mac, visionOS, Apple Vision, watchOS, tvOS, or other companion-platform targets and product surfaces.

Treat the exact approved feature-specific purpose strings and their base language as required input. Localize them through `InfoPlist.strings` when the accepted languages require it. If one or more strings are unavailable or do not describe the actual implementation, stop before design and ask the user to approve corrected copy.

Ask only for missing blocking inputs:

- Destination parent directory or an existing project path.
- Product/module name and bundle identifier or bundle identifier prefix.
- AppMetrica API key when available; otherwise use a configurable empty placeholder.
- Apple Developer Team only when device signing or capabilities require it.
- Privacy-policy and support URLs are not blocking inputs for this implementation workflow. Record missing values as `pending` for the later App Store preparation workflow.

Do not require the user to create an empty Xcode project. Create the project directory and targets when given a destination. For a new project, prefer the repository's existing generator; otherwise use an installed generator such as XcodeGen or Tuist. If none exists, create a minimal `.xcodeproj` with available local tooling. Do not install tools or dependencies outside normal project resolution without approval.

Never overwrite a non-empty directory, an existing project, signing settings, or user-owned files without first inspecting them and receiving approval when the operation is ambiguous.

## Workflow

### Phase 1: Confirm the specification

Summarize the accepted concept in a compact specification:

- Product name and primary user scenario.
- The smallest permission-centered MVP features and their permission mapping.
- All 10 authorization categories, counting photo-library read and add access separately, with the exact approved purpose string quoted for each category.
- The approved permission-review matrix, including the exact trigger path, denial fallback, data handling, privacy-label implication, and reviewer instruction for each category.
- The consolidated data inventory for on-device state, backend payloads, APNs/installation identifiers, AppMetrica, and advertising tracking.
- Lock Screen widget, Notification Service Extension, and Notification Content Extension roles.
- Minimal client-server behavior.
- General-audience and App Store review-safety constraints inherited from the idea.
- Explicit iPhone-only, portrait-only platform and orientation scope.
- App icon direction tied to the accepted name and concept.
- Current review-risk verdict and the specific implementation choices that keep questionable permissions credible.

Resolve contradictions before designing. Do not reopen already accepted choices without a concrete technical conflict.

### Phase 2: Prepare and approve the design

Create the smallest coherent screen set that makes every permission-backed feature reachable through an intentional user action. Features may share a screen when that keeps the product clearer; do not create one screen per permission mechanically.

Do not introduce new product features during design. Supporting states, navigation, configuration, analytics, extension plumbing, backend interaction, and error handling are allowed when needed and must stay minimal.

Before rendering, run a compact HIG and accessibility preflight. Prefer native controls and navigation; establish a clear hierarchy and scan order; provide at least 44×44 point touch targets; use Dynamic Type-compatible text styles; define VoiceOver labels, values, hints, grouping, and focus order; verify semantic-color contrast and the accepted light/dark behavior; and cover long localized text plus loading, empty, error, offline, not-determined, denied, limited, and authorized states. Revise the design before asking for approval when any core flow depends on tiny targets, fixed text, color alone, hidden permissions, or inaccessible custom controls.

Provide:

- Navigation map and screen inventory.
- Low-fidelity wireframe for each screen or distinct state.
- A rendered portrait iPhone mockup for every distinct user-facing app screen. At minimum, always render the main screen. When several screens are simple, one readable portrait-oriented composite artboard is acceptable. Do not render or approve landscape variants.
- Main, empty, loading, error, permission-not-determined, denied, and authorized states where relevant.
- The exact action that triggers each system permission request.
- The already approved feature-specific purpose text reproduced verbatim.
- An accessibility specification covering Dynamic Type, VoiceOver names and order, grouping, touch targets, contrast, and non-color status cues.
- Widget layouts and notification-extension layouts.
- One square app-icon concept shown at useful preview sizes. Keep it recognizable without text, original, and consistent with the visual system. Standard Apple visual conventions, system colors, and SF Symbols may inform the in-app interface, but do not use an SF Symbol, Apple logo, Apple product glyph, or a confusingly similar mark as the app icon or logo.
- A compact visual system: colors, typography, spacing, buttons, cards, icons, and light/dark behavior.
- A mapping from every approved feature to a screen and interaction.

Generate the rendered mockup during the design phase with the available image-generation capability and display it inline in the conversation immediately after the textual specification. Do not substitute an ASCII wireframe for the rendered mockup. Treat the image as a review artifact rather than proof that the application has already been implemented. Keep every rendered element feasible in UIKit with programmatic Auto Layout and consistent with the stated screen inventory, interactions, and visual system.

End with a clear approval question about the textual design, rendered mockup, and app icon. **Stop and wait. Do not create the Xcode project, backend, source files, or dependencies until the user explicitly approves the visual design and icon.** If the user supplies a complete design and explicitly marks it approved, record that fact and continue.

### Phase 3: Create the project after approval

Create a multi-target Xcode project with:

- Main UIKit iPhone application target, deployment target iOS 15.0.
- Lock Screen WidgetKit extension, deployment target iOS 16.0 because Lock Screen widget families are unavailable on iOS 15.
- Notification Service Extension, deployment target iOS 15.0.
- Notification Content Extension, deployment target iOS 15.0.
- Shared App Group capability when the app and widget exchange data.

Create explicit entitlements files and populate the capabilities that the code actually uses. The main app and widget must contain the same approved App Group identifier when sharing data; Push Notifications must be enabled for the main app when remote notification behavior is part of the accepted concept. Do not treat generator settings or source-code identifiers as proof that a capability is enabled. When signing assets are available, verify the capabilities in the provisioning profiles and the code-signed archive. When they are unavailable, stop short of claiming device or App Store readiness and list the exact Apple Developer configuration still needed.

When a generator such as XcodeGen owns the project, declare every entitlement value in its source specification, for example under the target's `entitlements.properties` in `project.yml`. Treat that specification as the source of truth: regenerate the project and inspect the emitted `.entitlements` files after every capability change. Do not patch only a generated entitlement file that the next generator run can overwrite.

Apply the following settings to the main app and every embedded extension as the project generator permits:

- `TARGETED_DEVICE_FAMILY = 1`
- `SUPPORTED_PLATFORMS = iphoneos iphonesimulator`
- `SUPPORTS_MACCATALYST = NO`
- `SUPPORTS_MAC_DESIGNED_FOR_IPHONE_IPAD = NO`
- `SUPPORTS_XR_DESIGNED_FOR_IPHONE_IPAD = NO`

Do not create iPad, macOS, Mac Catalyst, visionOS, Apple Vision, watchOS, or tvOS targets. Do not leave Mac or Apple Vision compatibility destinations enabled.

Restrict the main application's `UISupportedInterfaceOrientations` array to `UIInterfaceOrientationPortrait`. Do not add landscape or upside-down orientations, orientation-specific iPad keys, or rotation-dependent UI. Prefer this Info.plist declaration over custom rotation code. Keep widget and notification-extension layouts portrait-oriented, but do not add application-only orientation keys to extension plists unless Apple documentation for that extension point requires them.

Do not create unit-test or UI-test targets, test files, fixtures, mocks, or test-only infrastructure. Do not run unit or UI tests. Verification for this skill is limited to building every application and extension target, launching the app, inspecting the required flows, and exercising the real backend smoke path.

Create all UI programmatically. Do not add storyboards, XIBs, `UIMainStoryboardFile`, `UIApplicationSceneManifest` storyboard names, or `NSExtensionMainStoryboard`. Prefer `NSExtensionPrincipalClass` for the programmatic notification content extension and verify that the selected toolchain accepts it. If the extension point or toolchain requires a storyboard, stop and explain the conflict instead of silently violating the requirement.

Add an app-owned `PrivacyInfo.xcprivacy` to every shipped target that directly uses a required-reason API; do not assume the main application's manifest covers its extensions. When the app uses both `UserDefaults.standard` and App Group defaults, declare the applicable standard-defaults and App Group reasons. A widget that reads App Group `UserDefaults` needs its own manifest with the applicable App Group reason, such as `1C8F.1`. Inspect every integrated SDK's privacy manifest and required-reason API declarations, including the resolved AppMetrica version. Keep the resulting data inventory aligned with the implementation; do not copy declarations from another app or infer that an SDK collects no data merely because it ships a manifest.

Add an asset catalog containing an `AppIcon` set and configure the main target to use it. Supply at least one approved square 1024×1024 default iOS icon source and let Xcode generate smaller variants when the active toolchain supports single-size icons. Do not pre-round or externally mask the artwork. Keep optional dark or tinted appearances out unless they are already approved or materially improve the product without adding much work.

### Phase 4: Implement the smallest architecture

Default to UIKit and simple MVC. A large feature-oriented `UIViewController` is acceptable. Extract only resource-owning or deterministic helpers that materially reduce complexity, such as:

- `AppConfiguration`
- `APIClient`
- `AnalyticsClient`
- Permission-specific system adapters
- Small Codable models and pure formatters

Create one composition root in `SceneDelegate` or an `AppFactory` instance and inject dependencies through initializers. Avoid protocol-per-type, repositories, use-case layers, coordinators, reactive state stores, and dependency containers unless the accepted design truly needs them.

Minimize total source files, types, and lines without obscuring ownership or leaking resources. Reuse UIKit and Apple-provided controllers. Prefer direct code over reusable abstractions that have only one caller. Supporting code is allowed; product features unrelated to required permissions are not.

Use Dynamic Type text styles through `preferredFont(forTextStyle:)` and `adjustsFontForContentSizeCategory`, semantic colors, accessible UIKit controls, and explicit accessibility labels/values/hints only where UIKit cannot infer them correctly. Group composite status or score elements deliberately, preserve a logical VoiceOver order, and announce important asynchronous status changes without producing repetitive noise.

## Implementation details

Apply the following rules only after the user has explicitly approved the design.

### Project defaults

- Main target: UIKit, Swift, portrait-only iPhone iOS 15.0.
- UI: programmatic views and Auto Layout only.
- Widget target: WidgetKit/SwiftUI, iOS 16.0 for Lock Screen families.
- Notification extensions: iOS 15.0, programmatic entry points.
- Dependency manager: Swift Package Manager.
- Architecture: small MVC with explicit instance ownership.
- Backend: preferably one source file, no third-party packages, and only `/health` plus one merged domain operation when practical.

Use repository instructions and an existing project generator when present. Preserve a generated project specification such as `project.yml` or `Project.swift` as the source of truth when using a generator.

### App icon

- Generate or draw one original, concept-relevant 1024×1024 square master and include it in the project, not only as a conversation preview.
- Prefer a simple geometric mark, strong contrast, centered primary content, no small text, and no pre-rounded corners. Follow the current Apple app-icon and asset-catalog requirements.
- Use standard Apple system colors and SF Symbols freely inside the interface where appropriate, but do not use SF Symbols or confusingly similar glyphs as the app icon or logo.
- Configure `ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon` or its generator equivalent and keep the asset catalog in the main target's resources.

### Suggested source ownership

Keep the file graph small. Start with this ownership model and collapse files when simpler:

```text
AppDelegate / SceneDelegate (composition root)
  -> AppConfiguration
  -> configured URLSession / APIClient
  -> AppMetricaAnalyticsClient
  -> MainViewController
     -> instance-owned permission resources
```

Use one view controller unless a second one materially simplifies a permission-backed flow. Add an `AppFactory` or separate adapter types only when ownership or framework delegate requirements justify them. Do not add layers only to make the architecture appear sophisticated.

### Configuration

Keep mutable environment choices outside Swift source. Prefer an `.xcconfig`-driven setup containing values such as:

```text
PRODUCT_BUNDLE_IDENTIFIER
DEVELOPMENT_TEAM
APP_GROUP_IDENTIFIER
APPMETRICA_API_KEY
API_BASE_URL
```

Expose required values to Info.plist with build-setting substitution and read them once into an injected immutable `AppConfiguration`. Never store secrets or APNs private keys in the app bundle.

### AppMetrica boundary

Before implementation, consult the current official AppMetrica iOS quick start and package repository. Select a version that supports iOS 15 and use the current module and activation API rather than relying on a memorized old import.

Keep the SDK's process-global activation inside one instance-owned adapter. The app may call a global API required by the SDK, but must not expose its own singleton. Make missing or invalid configuration observable in debug logs without crashing or sending sensitive values.

Select only the AppMetrica modules the accepted data and tracking behavior needs. Add an advertising-support module only when the approved ATT-backed feature genuinely uses advertising identifiers or attribution. Record the SDK's actual diagnostic, usage, identifier, and tracking behavior in the data inventory and future App Store privacy answers. Apply current official data-sending or consent controls when required, disable unnecessary automatic collection where supported, and never claim that an ATT prompt alone constitutes a working tracking feature.

### Privacy and support handoff

Do not add a privacy-policy, support, Terms, or EULA screen as part of this implementation skill unless the user explicitly requests it. Record missing policy/support URLs and any future in-app entry point as `pending` in the release handoff manifest for the separate App Store preparation skill. Never claim App Store readiness while these values remain pending.

Keep permission purpose strings, pre-permission explanations, runtime behavior, AppMetrica configuration, backend payloads, `PrivacyInfo.xcprivacy`, and the data inventory mutually consistent. Treat mismatches as release blockers, not documentation cleanup.

### Extension boundaries

- Share only small value snapshots with the widget through `UserDefaults(suiteName:)` or a small App Group file.
- Do not observe shared defaults. Reload widget timelines explicitly after a state write.
- Keep Notification Service Extension networking optional and time-bounded; always call the content handler.
- Keep Notification Content Extension self-contained and route actions through notification categories or deep links.
- Verify that every target has the minimum frameworks and resources it needs.

### Backend boundary

Tie the backend to the accepted concept while minimizing its surface. Prefer exactly:

- `GET /health`
- `POST /sync`

Let `/sync` accept and return one compact idea-specific snapshot. Include an installation identifier or lightweight bearer value and the APNs device token in the same payload when practical, so separate authentication, device-registration, and CRUD endpoints are unnecessary. Split operations only when a single endpoint cannot provide an honest working client-server or push flow.

Use a configurable host and port, deterministic JSON, clear status codes, and in-memory state or one safe local JSON file ignored by version control. Keep production deployment out of scope unless explicitly requested.

### Static-state audit

Audit app-owned source, not generated or third-party code, for:

- `static var`
- `shared` singleton declarations or calls such as `URLSession.shared`
- global mutable declarations
- `NotificationCenter` observer registration
- KVO and long-lived Combine subscriptions
- timers, delegates, or closures without cancellation or cleanup

Document required Apple or SDK global entry points rather than disguising them. The goal is explicit ownership and short lifetimes, not a misleading claim of zero static memory.

### Build and behavior gates

Do not finish after code generation. Require:

- Main target and all extensions compile.
- The Release configuration compiles without app-owned compiler warnings. Do not suppress a warning merely to pass this gate.
- No `.storyboard` or `.xib` files or build settings remain.
- App launches on an available simulator.
- Backend health and one real round trip pass.
- The local backend stub passes `/health` and the accepted domain round trip. A production HTTPS host may remain `pending` in this implementation workflow; keep local HTTP debug-only, record the placeholder as a release blocker, and do not claim that the resulting build is ready for App Store submission.
- App works with empty AppMetrica key.
- With a valid AppMetrica key, the custom `launch` event is reported exactly once per process after successful SDK activation; it is not duplicated on every scene foreground transition.
- AppMetrica modules and collection controls match the approved data inventory and ATT behavior; dependency versions are resolved and pinned by the project source of truth.
- Approved permission strings exist under the correct keys.
- Base and localized permission strings describe the built feature exactly.
- Denied or unavailable permission states remain navigable.
- Every permission flow is exercised from its visible trigger through authorized or simulator-available behavior and denial recovery; a system prompt alone is not a passing result.
- Widget and notification targets have distinct bundle identifiers and correct deployment targets.
- App and widget entitlements contain the required App Group, Push capability is configured where applicable, and signed entitlements are checked when signing is available.
- Every app-owned target that directly uses required-reason APIs contains the appropriate Privacy Manifest, and resolved third-party manifests cover the SDKs actually shipped.
- Dynamic Type, VoiceOver names/order/grouping, touch targets, contrast, and non-color status cues match the approved accessibility specification.
- Every target is restricted to `iphoneos` and `iphonesimulator`, device family `1`, with Mac Catalyst, Designed for iPhone/iPad on Mac, and Apple Vision compatibility disabled.
- The processed main-app Info.plist supports only `UIInterfaceOrientationPortrait`; no landscape, upside-down, or iPad-specific orientation entries remain.
- The built application contains the approved `AppIcon`; asset compilation emits no missing-app-icon warning.
- When Apple signing is available, archive the Release configuration for Generic iOS Device, validate the archive, and inspect the code-signed app and extension entitlements. Otherwise report archive validation as a signing-dependent limitation.

## State and memory rules

Minimize process-wide and long-lived state:

- Do not create app-owned singletons, global mutable variables, `static var` storage, service locators, or mutable global caches.
- Do not use `NotificationCenter` observers, KVO, Combine publishers, or permanent event buses for app state.
- Do not use `URLSession.shared`; inject a configured `URLSession` instance.
- Allow immutable `static let` constants only when they hold no runtime state.
- Allow framework-required global entry points such as `UNUserNotificationCenter.current()`, `ATTrackingManager`, or AppMetrica's SDK API only inside small injected adapters.
- Prefer direct delegates, target-action, scoped closures, and `async` functions. Capture owners weakly where a callback can outlive them.
- Give Core Bluetooth, location, audio, camera, and other delegate-based objects an explicit owner and lifecycle. Stop scanning, recording, or location updates when the feature ends.

Do not claim that the app uses no static memory at all; enforce the practical goal of no app-owned mutable global state and no unnecessary long-lived objects.

## AppMetrica

Integrate the current official AppMetrica iOS SDK through Swift Package Manager after checking its current official documentation and iOS 15 compatibility.

- Isolate SDK calls in an instance-owned `AppMetricaAnalyticsClient` injected into the UI.
- Read `APPMETRICA_API_KEY` from build configuration or Info.plist via `AppConfiguration`; never hard-code it in Swift.
- Support changing the key through `Config.xcconfig`, a scheme environment override, or another user-approved startup configuration without source changes.
- If the key is empty, disable analytics gracefully and keep the app usable.
- After successful SDK activation, report a custom event named exactly `launch` once per process for the application startup. Treat it as a product event separate from AppMetrica's automatic session and app-open statistics. Do not report it when configuration is missing or activation fails, and log reporting failures without crashing.
- Keep data sending and ATT-dependent behavior consistent with the user's authorization and the current AppMetrica SDK's official controls.
- Link advertising-support functionality only when the approved ATT feature uses it. Document how authorized and denied states change actual behavior.
- Report only useful product events: the required `launch` event, screen open, feature action, permission outcome, and API result.
- Never report contact data, precise location, photos, recordings, advertising identifiers, secrets, or other sensitive payloads as event parameters.
- Record AppMetrica's SDK-level data collection separately from custom event parameters; “we do not send sensitive event parameters” does not mean “the SDK collects no data.”
- Document when a key change requires terminating and relaunching the app because the SDK is activated once per process.

## Extensions

Implement all extensions as real targets, not placeholders:

- Lock Screen widget: show one glanceable, useful state from the core feature. Share a compact Codable snapshot through an App Group without observers.
- Notification Service Extension: enrich or transform a remote notification and safely fall back to the original content before the system deadline.
- Notification Content Extension: render a compact programmatic custom notification UI with at least one relevant action.

Keep extension binaries small and do not link the whole application feature graph into them.

## Minimal backend and client

Create a small backend next to the app in a clearly separate `backend/` directory. Prefer the simplest already-installed runtime and no third-party packages: Python standard library or Node's built-in HTTP modules are acceptable.

- Prefer only `/health` and one idea-specific `/sync` endpoint. Let `/sync` also carry lightweight authentication or installation identity and APNs device-token registration when practical.
- Add separate endpoints only when the accepted feature cannot work honestly without them.
- Use memory or one local JSON file for persistence unless the accepted feature requires more.
- Provide a single documented start command and a repeatable sample request.
- Configure the iOS base URL outside Swift source.
- Use an injected `URLSession`, Codable request/response types, timeouts, cancellation, and visible error handling.
- Keep local HTTP allowances debug-only. State that a release/App Store build needs reachable HTTPS.
- Allow the production backend URL to remain an explicit placeholder during this implementation workflow. Mark deployment and real HTTPS verification as `pending`; do not block local completion or disguise the placeholder as a working service.
- When APNs signing credentials are unavailable, provide an honest sender stub or request generator and identify remote-push delivery as unverified; do not simulate success silently.

## Verification

Verify in proportion to the environment:

1. Generate the project and resolve package dependencies.
2. Build the main app and every extension for an available simulator with `xcodebuild`.
3. Start the backend, call `/health`, and exercise at least one real client-server round trip.
4. Launch the app in a simulator when available, wait for the meaningful application hierarchy rather than accepting a launch screen, and inspect its principal screens.
5. Search the created project for storyboards/XIBs, app-owned singleton patterns, mutable static storage, observers, and `URLSession.shared`; fix violations or explain unavoidable framework calls.
6. Exercise every permission feature through its real visible trigger. Capture a screenshot and accessibility hierarchy before and after important interactions, verify the expected UI state changed, and cover denied recovery. Retry a failed simulator interaction once, then classify it as functional, visual, environment/transient, or an unexpected exit; inspect the process and logs after exits. Do not create an XCTest or UI-test target for this workflow.
7. Verify that source, localized, and processed Info.plist permission text matches the approved feature-specific copy.
8. Verify AppMetrica configuration with an empty key, its selected modules and consent/tracking behavior, and document how to supply a real key. With a valid test key, verify that the custom `launch` event is attempted exactly once after successful activation and not on each scene foreground transition.
9. Inspect `PrivacyInfo.xcprivacy`, resolved SDK privacy manifests, backend payloads, and AppMetrica behavior against the approved data inventory.
10. Verify App Group and Push entitlements in source and, when signing is available, in the signed products and provisioning profiles.
11. Record privacy-policy URL, support URL, their future in-app entry point, and production backend deployment as `pending` when they are deferred to App Store preparation. Do not treat them as local implementation failures or claim submission readiness.
12. State which behaviors still require a physical device, Apple signing, APNs credentials, or real hardware.
13. Inspect final build settings and verify `TARGETED_DEVICE_FAMILY = 1` for every target and no Mac, Mac Catalyst, visionOS, or Apple Vision platform compatibility. Xcode may still list iPad simulators for iPhone-app compatibility mode; do not mistake that list for native iPad support.
14. Inspect the source and built main-app Info.plist and verify that `UISupportedInterfaceOrientations` contains exactly `UIInterfaceOrientationPortrait`. Exercise the principal screens without rotating the simulator; do not add rotation code to satisfy this check.
15. Build the app with the final asset catalog, verify the `AppIcon` is compiled into the application, and inspect its appearance at full size and a small Home Screen-like preview.
16. Build Release without app-owned warnings. When signing is available, archive for Generic iOS Device, validate the archive, and inspect the final signed entitlements; otherwise report the exact signing-dependent checks that remain.

Do not broaden the MVP while fixing build or launch failures.

## Release handoff manifest

After successful verification, create or update `Release/release-manifest.json` with:

- Product name, bundle identifiers, version, and build number.
- App Group, capabilities, and entitlement verification status.
- Permission keys, exact localized copy, trigger paths, denial behavior, and reviewer instructions.
- Data inventory, tracking behavior, AppMetrica modules, and privacy status.
- Backend, privacy-policy, and support URLs.
- Extension identifiers and roles.
- Deterministic application states intended for future App Store screenshots.
- Device-only, signing, APNs, and hardware checks that remain unresolved.

Use deterministic JSON and mark unavailable values as `pending`. Never include AppMetrica API keys, APNs keys, `.p8` files, access tokens, or other secrets.

## Handoff

Lead with what works. Provide clickable paths to the project, configuration file, backend entry point, approved visual mockup, app-icon master, design specification, each app-owned Privacy Manifest, permission localizations, and `Release/release-manifest.json`. Include exact build and backend-run commands, configuration values the user must provide, verified targets, Release/archive status, privacy and capability checks, deferred App Store policy/support/backend items, and remaining device-only or signing limitations.
