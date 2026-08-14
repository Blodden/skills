---
name: build-ios-app-concept
description: Design, render a portrait iPhone UI mockup, obtain approval for, create, implement, and verify a previously selected portrait-only iPhone iOS application concept with its accepted name and permission copy. Use when Codex should turn an approved app idea into the smallest practical programmatic UIKit iOS 15 project centered on real use of every required permission, with minimal supporting code, simple MVC-style architecture, configurable AppMetrica, low static state, an iPhone Lock Screen widget, notification service and content extensions, the smallest working client-server interaction, portrait-only interface support, and no iPad, Mac, Mac Catalyst, Designed for iPhone/iPad on Mac, visionOS, or Apple Vision support. Also use when the user asks whether Codex can create the Xcode project and supporting backend from scratch.
---

# Build iOS App Concept

## Goal

Turn an accepted product concept into the smallest working portrait-only iPhone iOS application that exercises every required permission through a real user action. Keep product scope centered on those permission-backed functions. Write the supporting navigation, state, models, configuration, analytics, extensions, networking, error handling, and verification code needed to make them work, but keep it deliberately small and avoid unrelated features. Keep the application understandable, buildable, and easy to reconfigure. Stop at defined approval gates instead of making product decisions silently.

## Permission implementation

Before designing or implementing permission-backed features, use the exact purpose text approved during ideation; do not silently rewrite it. Functional rationale and system purpose copy are separate: make the feature rationale specific, but keep the approved displayed string unchanged. If the exact copy is missing, request it before producing the design.

Ensure that all 10 authorization categories are actually reachable and exercised. Let one coherent product function cover several permissions where natural; do not add ten isolated demo features solely to request them.

### Required feature mapping

| Capability | Framework and request | Required configuration | Minimum real feature |
|---|---|---|---|
| Bluetooth | CoreBluetooth; retain an instance-owned `CBCentralManager`, whose use triggers authorization | `NSBluetoothAlwaysUsageDescription` | Scan for and display nearby compatible peripherals; allow connect/disconnect when possible |
| Camera | AVFoundation; `AVCaptureDevice.requestAccess(for: .video)` | `NSCameraUsageDescription` | Capture a photo, video, or scan used by the accepted concept |
| Contacts | Contacts; request access through an instance-owned `CNContactStore` | `NSContactsUsageDescription` | Select a contact for a concept-relevant share, invite, or recipient action |
| Face ID | LocalAuthentication; evaluate an `LAContext` policy on demand | `NSFaceIDUsageDescription` | Protect or confirm a meaningful local action, with passcode/unavailable handling |
| Location | CoreLocation; instance-owned `CLLocationManager.requestWhenInUseAuthorization()` | `NSLocationWhenInUseUsageDescription` | Show or use a current concept-relevant nearby/current-location result |
| Microphone | AVFAudio/AVFoundation; use an iOS-15-compatible record-permission API | `NSMicrophoneUsageDescription` | Record, preview, stop, and discard or use a short audio clip |
| Photo-library read | PhotoKit; request `.readWrite` authorization only when direct library access is required | `NSPhotoLibraryUsageDescription` | Select or browse an existing image/video for the accepted concept |
| Photo-library add | PhotoKit; request `.addOnly` authorization | `NSPhotoLibraryAddUsageDescription` | Save a generated or captured image/video and show success/failure |
| Advertising tracking | AppTrackingTransparency; request through `ATTrackingManager` after an explanatory user action | `NSUserTrackingUsageDescription` | Gate a clearly described advertising or attribution feature; keep the app useful when denied |
| Push notifications | UserNotifications; `UNUserNotificationCenter.current().requestAuthorization` with accepted options | No usage-description key; iOS owns the system alert copy | Show a pre-permission explanation, notification preferences, and a concept-relevant notification flow |

### Permission UX rules

- Request a permission only after the user selects the feature that needs it. Do not fire all prompts at launch.
- Display the accepted general purpose text on the system prompt where iOS supports it.
- Handle `notDetermined`, authorized/limited, denied, restricted, unavailable, and unknown states as applicable.
- After denial, explain the disabled feature and offer a deliberate Settings action. Do not loop or pressure the user.
- Keep the core app usable when optional permissions are denied.
- Never upload contacts, precise location, media, recordings, or identifiers merely to demonstrate a permission.
- Use system pickers when they improve privacy, but note that PHPicker may not trigger full photo-library authorization. Because this product explicitly demonstrates the approved read permission, keep a separate direct-read feature when that authorization must be exercised.

### Minimum screen strategy

Use the fewest screens that remain understandable. Prefer one main view controller with compact sections, sheets, alerts, and Apple-provided pickers or controllers. Add a separate screen only when the permission-backed interaction cannot stay clear on the main screen. Permission status, denied recovery, loading, and error states are supporting UI and may share the same views.

Combine related capabilities into coherent flows. Do not create ten disconnected demo buttons, a separate settings screen, onboarding, profile, feed, or history screen unless the accepted permission-centered MVP genuinely needs them.

## Inputs and project ownership

Start from the accepted idea, name, permission copy, extension roles, and backend outline. Preserve decisions already approved by the user.

Implement only the portrait-oriented iPhone portion of the accepted idea. Adapt any landscape-dependent flow to portrait rather than retaining rotation support. Do not create or retain iPad, macOS, Mac Catalyst, Designed for iPhone/iPad on Mac, visionOS, Apple Vision, watchOS, tvOS, or other companion-platform targets and product surfaces.

Treat the exact approved purpose strings as required input. Do not derive, improve, localize, or make them more feature-specific. If one or more strings are unavailable in the current context, stop before design and ask the user to provide them or explicitly authorize reuse of a named existing source.

Ask only for missing blocking inputs:

- Destination parent directory or an existing project path.
- Product/module name and bundle identifier or bundle identifier prefix.
- AppMetrica API key when available; otherwise use a configurable empty placeholder.
- Apple Developer Team only when device signing or capabilities require it.

Do not require the user to create an empty Xcode project. Create the project directory and targets when given a destination. For a new project, prefer the repository's existing generator; otherwise use an installed generator such as XcodeGen or Tuist. If none exists, create a minimal `.xcodeproj` with available local tooling. Do not install tools or dependencies outside normal project resolution without approval.

Never overwrite a non-empty directory, an existing project, signing settings, or user-owned files without first inspecting them and receiving approval when the operation is ambiguous.

## Workflow

### Phase 1: Confirm the specification

Summarize the accepted concept in a compact specification:

- Product name and primary user scenario.
- The smallest permission-centered MVP features and their permission mapping.
- All 10 authorization categories, counting photo-library read and add access separately, with the exact approved purpose string quoted for each category.
- Lock Screen widget, Notification Service Extension, and Notification Content Extension roles.
- Minimal client-server behavior.
- General-audience and App Store review-safety constraints inherited from the idea.
- Explicit iPhone-only, portrait-only platform and orientation scope.

Resolve contradictions before designing. Do not reopen already accepted choices without a concrete technical conflict.

### Phase 2: Prepare and approve the design

Create the smallest coherent screen set that makes every permission-backed feature reachable through an intentional user action. Features may share a screen when that keeps the product clearer; do not create one screen per permission mechanically.

Do not introduce new product features during design. Supporting states, navigation, configuration, analytics, extension plumbing, backend interaction, and error handling are allowed when needed and must stay minimal.

Provide:

- Navigation map and screen inventory.
- Low-fidelity wireframe for each screen or distinct state.
- A rendered portrait iPhone mockup for every distinct user-facing app screen. At minimum, always render the main screen. When several screens are simple, one readable portrait-oriented composite artboard is acceptable. Do not render or approve landscape variants.
- Main, empty, loading, error, permission-not-determined, denied, and authorized states where relevant.
- The exact action that triggers each system permission request.
- The already approved purpose text reproduced verbatim; never replace general copy with a concept-specific rewrite.
- Widget layouts and notification-extension layouts.
- A compact visual system: colors, typography, spacing, buttons, cards, icons, and light/dark behavior.
- A mapping from every approved feature to a screen and interaction.

Generate the rendered mockup during the design phase with the available image-generation capability and display it inline in the conversation immediately after the textual specification. Do not substitute an ASCII wireframe for the rendered mockup. Treat the image as a review artifact rather than proof that the application has already been implemented. Keep every rendered element feasible in UIKit with programmatic Auto Layout and consistent with the stated screen inventory, interactions, and visual system.

End with a clear approval question about both the textual design and rendered mockup. **Stop and wait. Do not create the Xcode project, backend, source files, or dependencies until the user explicitly approves the visual design.** If the user supplies a complete design and explicitly marks it approved, record that fact and continue.

### Phase 3: Create the project after approval

Create a multi-target Xcode project with:

- Main UIKit iPhone application target, deployment target iOS 15.0.
- Lock Screen WidgetKit extension, deployment target iOS 16.0 because Lock Screen widget families are unavailable on iOS 15.
- Notification Service Extension, deployment target iOS 15.0.
- Notification Content Extension, deployment target iOS 15.0.
- Shared App Group capability when the app and widget exchange data.

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

### Phase 4: Implement the smallest architecture

Default to UIKit and simple MVC. A large feature-oriented `UIViewController` is acceptable. Extract only resource-owning or deterministic helpers that materially reduce complexity, such as:

- `AppConfiguration`
- `APIClient`
- `AnalyticsClient`
- Permission-specific system adapters
- Small Codable models and pure formatters

Create one composition root in `SceneDelegate` or an `AppFactory` instance and inject dependencies through initializers. Avoid protocol-per-type, repositories, use-case layers, coordinators, reactive state stores, and dependency containers unless the accepted design truly needs them.

Minimize total source files, types, and lines without obscuring ownership or leaking resources. Reuse UIKit and Apple-provided controllers. Prefer direct code over reusable abstractions that have only one caller. Supporting code is allowed; product features unrelated to required permissions are not.

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
- No `.storyboard` or `.xib` files or build settings remain.
- App launches on an available simulator.
- Backend health and one real round trip pass.
- App works with empty AppMetrica key.
- Approved permission strings exist under the correct keys.
- Denied or unavailable permission states remain navigable.
- Widget and notification targets have distinct bundle identifiers and correct deployment targets.
- Every target is restricted to `iphoneos` and `iphonesimulator`, device family `1`, with Mac Catalyst, Designed for iPhone/iPad on Mac, and Apple Vision compatibility disabled.
- The processed main-app Info.plist supports only `UIInterfaceOrientationPortrait`; no landscape, upside-down, or iPad-specific orientation entries remain.

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
- Report only useful product events: app start, screen open, feature action, permission outcome, and API result.
- Never report contact data, precise location, photos, recordings, advertising identifiers, secrets, or other sensitive payloads as event parameters.
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
- When APNs signing credentials are unavailable, provide an honest sender stub or request generator and identify remote-push delivery as unverified; do not simulate success silently.

## Verification

Verify in proportion to the environment:

1. Generate the project and resolve package dependencies.
2. Build the main app and every extension for an available simulator with `xcodebuild`.
3. Start the backend, call `/health`, and exercise at least one real client-server round trip.
4. Launch the app in a simulator when available and inspect its principal screens.
5. Search the created project for storyboards/XIBs, app-owned singleton patterns, mutable static storage, observers, and `URLSession.shared`; fix violations or explain unavoidable framework calls.
6. Verify that every permission feature is reachable, handles denied state, and uses the approved purpose text.
7. Verify AppMetrica configuration with an empty key and document how to supply a real key.
8. State which behaviors still require a physical device, Apple signing, APNs credentials, or real hardware.
9. Inspect final build settings and verify `TARGETED_DEVICE_FAMILY = 1` for every target and no Mac, Mac Catalyst, visionOS, or Apple Vision platform compatibility. Xcode may still list iPad simulators for iPhone-app compatibility mode; do not mistake that list for native iPad support.
10. Inspect the source and built main-app Info.plist and verify that `UISupportedInterfaceOrientations` contains exactly `UIInterfaceOrientationPortrait`. Exercise the principal screens without rotating the simulator; do not add rotation code to satisfy this check.

Do not broaden the MVP while fixing build or launch failures.

## Handoff

Lead with what works. Provide clickable paths to the project, configuration file, backend entry point, approved visual mockup, and design specification. Include exact build and backend-run commands, configuration values the user must provide, verified targets, and remaining device-only limitations.
