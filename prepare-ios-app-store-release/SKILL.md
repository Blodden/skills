---
name: prepare-ios-app-store-release
description: Prepare an already approved and implemented portrait-only iPhone application for App Store submission. Use after build-ios-app-concept when Codex needs to create the mandatory privacy policy and support materials, add an approved in-app privacy-policy link, prepare required App Store metadata, capture one to three authentic screenshots from the existing one or two screens, draft App Privacy and mandatory compliance answers, collect minimum App Review information, and produce a local Release archive. Ask for legal and contact facts on every run without creating a reusable profile. Exclude optional marketing assets, custom EULA, TestFlight, expanded reviewer materials, repeated implementation audits, and every publish, upload, validation, submission, or release action unless the user separately and explicitly authorizes that exact external action.
---

# Prepare iOS App Store Release

## Goal

Turn a completed iPhone application and its approved release handoff into the smallest mandatory App Store submission package. Prepare legal text, the required product-page content, authentic screenshots, App Store questionnaire answers, minimum review information, and a local Release archive. Make only the narrowly approved application-code change needed to expose the privacy-policy URL.

Keep this skill limited to App Store preparation. Do not reopen the product idea or design, broaden the MVP, add marketing extras, or redo implementation verification already owned by `build-ios-app-concept`.

## Required handoff

Start only when these inputs exist:

- A completed Xcode project or workspace and its path.
- An idea-approved and design-approved `AppSpec.md`.
- Approved application design and icon.
- `Release/release-manifest.json` produced by the implementation skill.
- A buildable main iPhone target and its required extensions.
- The intended primary App Store locale.

Treat `AppSpec.md` as the accepted product and privacy contract and `Release/release-manifest.json` as the implementation handoff. If either is missing or they visibly contradict each other, stop and request an updated handoff from the appropriate earlier skill. Do not reconstruct a release contract by performing a new application audit.

## Hard boundaries

Do not repeat checks for:

- AppMetrica API key configuration or the custom `launch` event.
- Replacement of the local backend with production HTTPS.
- ATT, IDFA, or the actual tracking implementation.
- Debug URLs, test keys, embedded secrets, or unsupported Apple platforms.
- Application icon correctness.
- Application download or installation size.

Use the accepted values already recorded in the handoff when App Store forms need them. If a mandatory form answer is absent, ask the user or route the missing fact back to the implementation handoff; do not inspect the whole codebase to rediscover it.

Do not create:

- Marketing URL or promotional text.
- App previews or videos.
- Optional reviewer notes, walkthroughs, attachments, or hardware demonstrations.
- Additional localizations not requested as the primary submission locale.
- Terms of Use or a custom EULA. Use Apple's standard EULA.
- TestFlight groups or testing workflows.
- Unit tests, UI tests, test targets, fixtures, or test-only code.
- A reusable developer, legal, or App Store profile.

Never use a third-party legal-document generator unless the user makes a separate request to do so.

## Current Apple requirements

Before preparing a release, browse current official Apple Developer and App Store Connect documentation. Verify only the submission requirements relevant to this workflow, including:

- Required Xcode and iOS SDK version for new uploads.
- Current iPhone screenshot dimensions and file rules.
- Required App Store metadata and App Review fields.
- Current App Privacy, age-rating, content-rights, DSA, and export-compliance questions.

Use official Apple sources as the primary authority and cite them when reporting a requirement. Do not hardcode a volatile requirement as timeless. If current official documentation is unavailable, mark the affected value `pending` rather than guessing.

## Per-run user facts

Inspect the handoff first, then ask once for only the mandatory facts that cannot be derived from it. Depending on the submission, these can include:

- Developer or legal entity name to display in the legal documents.
- Public support and privacy contact email.
- Public address or telephone details when required for the selected storefronts or legal status.
- Private App Review contact first name, last name, email, and telephone number.
- Copyright holder and year.
- Intended App Store countries or regions.
- DSA trader declaration and required factual details.
- Content-rights declaration.
- Export-compliance answers that require the developer's legal determination.
- Confirmed data-retention and deletion commitments when the handoff leaves them pending.
- Existing Privacy Policy and Support URLs, if already published.

Ask for these facts again on each independent run. Do not create or read a reusable `AppStoreProfile`, send them to a generator, or copy them outside the current application's release artifacts. Include public or review-contact values in an output artifact only where the agreed submission package requires them. Never persist account passwords, API keys, signing keys, session cookies, or other secrets.

Do not invent a legal identity, address, contact, trader status, content ownership, retention promise, or export classification. Offer a concise draft choice only when the user can validate it as factual. State that generated legal text is a preparation draft rather than legal advice.

## Workflow

### Phase 1: Establish the mandatory submission

Read `AppSpec.md`, `Release/release-manifest.json`, the approved screen inventory, and the project configuration needed to locate the scheme and version. Determine whether this is a first release or an update, the primary locale, product name, bundle identifier, version, build number, primary category, selected storefronts, and release mode.

Create `Release/AppStore/submission.json` as the local source of truth for the App Store package. Record accepted, pending, and externally blocked values explicitly. Do not include secrets.

### Phase 2: Draft the mandatory legal pages

Create:

- `Release/AppStore/legal/privacy-policy.md`.
- `Release/AppStore/legal/support.md`.

Build the privacy-policy draft from the approved data inventory and the user's factual answers. Cover only actual behavior:

- Developer or data-controller identity and contact.
- Data accessed locally by permission-backed functions.
- Data transmitted to the backend and third-party SDKs.
- AppMetrica, notification token, installation identifier, analytics, advertising, and tracking behavior recorded in the handoff.
- Purpose of processing and whether each data category is linked to the user or used for tracking.
- Retention, deletion, consent withdrawal, and permission revocation.
- How the user can make a privacy request.

Make the support page minimal: application name, support contact, how to report a problem, and the Privacy Policy link or a `pending` marker until it is published.

Show both drafts to the user and require explicit approval. Do not publish them as part of ordinary preparation. Publishing requires a separate explicit request.

### Phase 3: Add the in-app Privacy Policy link

A working Privacy Policy URL is required before completing submission readiness. If the page has not been published, finish the local drafts, mark the URL `pending`, and do not fabricate one.

After the user supplies the real URL:

1. Propose one minimal placement on an existing approved screen, normally a small text button at the bottom of the primary screen.
2. Obtain explicit approval for that placement.
3. Modify only the code needed to display `Политика конфиденциальности` or its approved localization and open the real URL through `SFSafariViewController` or the system browser.
4. Do not add a settings, information, legal, or onboarding screen solely for the link.
5. Build and launch the application and verify that the link is visible, accessible, and opens the correct HTTPS page.

Preserve the approved visual system and minimum application scope. Record the release-only UI adjustment in `submission.json` and `Release/release-manifest.json`.

### Phase 4: Prepare mandatory App Store metadata

Create `Release/AppStore/metadata/<locale>.md` containing:

- App name.
- Subtitle.
- Plain-text description.
- Keywords within Apple's current byte limit.
- Primary category.
- Copyright.
- Privacy Policy URL.
- Support URL.
- Version number and release mode.
- `What's New` only when it is mandatory for an update.

Keep every claim demonstrably true in the shipped application. Do not mention future functionality, generic superlatives, other companies' products, prices that may change, or permissions without the corresponding visible behavior.

Do not add optional product-page fields or a secondary category.

### Phase 5: Capture one to three authentic screenshots

Use only the application's existing one or two approved screens. Do not create a screen, feature, or navigation path for App Store imagery.

1. Select one to three meaningful existing states that explain the application with minimum repetition.
2. Populate deterministic demonstration data without real contacts, locations, photos, recordings, identifiers, notification contents, or other personal information.
3. Launch the real Release-equivalent application in a supported iPhone simulator.
4. Capture the real rendered UI at an Apple-accepted current portrait size.
5. Save opaque PNG or JPEG files under `Release/AppStore/screenshots/<locale>/` in intended display order.
6. Do not redraw, fabricate, materially alter, frame, caption, or composite the application UI.
7. Show the final files to the user and require explicit approval.

If one screenshot is sufficient, stop at one. Never manufacture variety to reach a target count.

### Phase 6: Prepare App Privacy answers

Translate the accepted data inventory into structured App Store Connect answers and store them in `submission.json`. For every collected data type record the purpose, whether it is linked to the user, whether it is used for tracking, and whether collection is optional.

Include the application, backend, AppMetrica, notification, installation, advertising, and tracking behavior already stated in the handoff. Do not claim `Data Not Collected` when the handoff records transmission by a third-party SDK. Do not re-audit ATT, IDFA, AppMetrica, or backend code in this phase.

If the handoff cannot support an exact answer, mark the question `pending` and request a factual decision. Do not select a convenient privacy answer merely to unblock submission.

### Phase 7: Prepare mandatory compliance answers

Prepare only the questions required for this specific submission:

- Age Rating questionnaire.
- Content Rights declaration.
- DSA trader status.
- Export Compliance and encryption declaration.
- Advertising or tracking declarations required by the recorded application behavior.
- Account-related declaration when the app creates accounts.
- Storefront-specific declarations required by the selected countries, application category, or content.

Derive descriptive answers from the application where possible, but require the user to decide legal status, ownership, availability, and export matters. Do not add irrelevant regional questionnaires or optional compliance material.

### Phase 8: Prepare minimum App Review information

Create `Release/AppStore/review-information.md` containing only:

- Review contact first and last name.
- Review contact email and telephone number.
- Whether a demo account is required.
- A `provide directly in App Store Connect` marker for demo credentials when login is mandatory; never write the password to disk.
- One short review note only when a required function cannot be found or exercised without it.

Do not create expanded permission walkthroughs, denial descriptions, video, attachments, or optional reviewer guidance.

### Phase 9: Build the local Release archive

After the user approves the legal text, metadata, screenshots, questionnaire answers, version, and build number:

1. Apply the approved version and build number.
2. Build every shipped target with the current App Store-compatible SDK.
3. Create a local Release archive for Generic iOS Device with the existing signing configuration.
4. Inspect locally that the main application and required extensions are present and that the archive identifies the intended bundle, version, and build.
5. Do not create or run tests.

Apple-hosted validation, upload, and processing are external actions. Do not perform them during local preparation without a separate explicit authorization for the exact version and build.

## Output structure

Keep the local output minimal:

```text
Release/AppStore/
├── submission.json
├── metadata/
│   └── <locale>.md
├── legal/
│   ├── privacy-policy.md
│   └── support.md
├── screenshots/
│   └── <locale>/
└── review-information.md
```

Do not add Terms, custom EULA, marketing material, app-preview files, TestFlight documentation, extra reports, reusable profiles, or auxiliary release documents.

## Approval gates

Stop for explicit approval at these points:

1. Privacy Policy, Support page, and proposed placement of the in-app Privacy Policy link.
2. App Store metadata and final one to three screenshots.
3. App Privacy, mandatory compliance answers, and minimum App Review information.
4. Exact version and build before archive creation or any external action.

Approval of one gate does not imply approval of a later gate.

## External-action boundary

Never perform any of the following without a separate explicit instruction naming the intended action:

- Publish Privacy Policy or Support pages.
- Create or modify an App Store Connect application record.
- Remotely validate or upload a build.
- Enter or publish metadata in App Store Connect.
- Add a version for review.
- Submit a version for review.
- Release an approved version.

Before an authorized external action, show the exact application, bundle identifier, version, build, storefront scope, and action. Do not treat a request to “prepare” as authorization to mutate App Store Connect or a hosting service.

## Completion criteria

Mark local preparation complete only when:

- All mandatory local files exist and contain approved, non-placeholder values except external URLs or actions explicitly marked pending.
- The user approved the legal drafts, metadata, screenshots, privacy answers, compliance answers, and review information.
- The in-app Privacy Policy link opens the real approved HTTPS URL, or this remains clearly reported as a submission blocker.
- The local Release archive was created successfully, or signing is clearly reported as the remaining blocker.
- No optional artifacts or product features were added.

Do not call the application ready to submit while Privacy Policy or Support URLs, mandatory factual answers, signing, archive creation, or separately required external validation remain pending.

## Handoff

Lead with the readiness result. Provide clickable paths to the project, `AppSpec.md`, release manifest, `submission.json`, metadata, legal drafts, screenshots, review information, and local archive. List approved values, mandatory pending items, signing or URL blockers, and the exact next external action available for separate user authorization.
