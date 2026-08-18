---
name: design-ios-app-concept
description: Design and render an accepted portrait-only iPhone application concept before implementation. Use when an idea, name, permissions, feature-specific permission copy, data behavior, extensions, and minimal backend have already been accepted and the user wants screen structure, a seamless startup/loading experience, permission UX, visual mockups, accessibility behavior, widget and notification layouts, or an app icon. Produce an approved design handoff without creating an Xcode project, application source code, dependencies, or backend code.
---

# Design iOS App Concept

## Goal

Turn an idea-approved `AppSpec.md` into the smallest feasible portrait-only iPhone interface that makes every required permission-backed function reachable through a clear, intentional user action. Render the application screens and app icon, show them to the user, and stop until the user explicitly approves the design.

Do not create an Xcode project, targets, dependencies, application source code, or backend. Keep product scope centered on the accepted permission-backed functions and add only the navigation, states, explanations, and error handling needed to make them understandable.

## Inputs and source of truth

Read `AppSpec.md` when available. Preserve its accepted product, naming, permission, privacy, tracking, extension, backend, platform, orientation, and review-risk decisions. If it is unavailable, reconstruct the complete specification from the accepted conversation before designing and write it when a workspace becomes available.

Require `idea-approved` status or an equivalent explicit approval in the conversation. If the idea is still being selected, stop and propose `generate-ios-app-ideas` instead of choosing silently.

Do not reopen accepted decisions unless they create a concrete feasibility, privacy, accessibility, or App Store review conflict. Explain the conflict and obtain approval before changing `AppSpec.md`.

Treat every new or changed user-facing screen or state requested after design approval as a design delta. Render and present at least the affected screen or state, obtain explicit approval, and update `AppSpec.md` before implementation. A request to implement does not approve an appearance the user has not seen. Never mark a design delta approved after its code has already been written.

## Design rules

- Use the fewest screens that keep all permission-backed flows understandable. Prefer one primary screen with compact sections, sheets, alerts, and Apple-provided controllers.
- Add a screen only when an accepted interaction becomes materially clearer. Do not invent onboarding, profile, feed, history, settings, or other unrelated product features.
- Map every user-facing feature to at least one required authorization. Supporting navigation, state, configuration, analytics, extensions, backend interaction, and error handling are allowed and must stay minimal.
- Request a permission only after the user selects the visible feature that needs it. Never design a batch of prompts at launch.
- Preserve exact approved feature-specific permission copy. For push authorization, design the app-owned explanation while recognizing that iOS owns the system alert text.
- Cover `notDetermined`, authorized or limited, denied, restricted, unavailable, loading, empty, offline, success, and error states where applicable.
- Represent `cloudSyncEnabled` with only compact checking, available, remotely disabled, and offline/fallback status states in an existing screen. Do not add an in-app switch, settings screen, or separate feature-flag UI.
- Design one user-perceived startup screen, not two visibly different screens. Explicitly present and obtain approval for its background, artwork source such as the app icon or a separate mark, sizing, placement, corner treatment, presence or absence of text and an indicator, and transition to the main interface. Make the static iOS launch representation and its programmatic continuation use the same approved background, artwork, sizing, and placement so the handoff has no perceptible flash. Keep the static representation free of dynamic content and nonlocalized text.
- Start required local initialization and the single `cloudSyncEnabled` request as soon as the programmatic startup continuation appears. Default its app-controlled display interval to at least 0.7 seconds and at most 2 seconds; record both values in `AppSpec.md` and obtain approval for any change. If the request exceeds the maximum, design a transition using the last cached value or `true` on first install, with compact offline/fallback status on the main screen.
- Show the startup screen once per cold process launch, not whenever the app returns from the background. Do not place permissions, ATT prompts, advertisements, retry controls, or unrelated onboarding on it.
- Keep the core application useful when optional access, including ATT, is denied.
- Make microphone recording, saving, saved, playback, replace, and delete states clear through both status and controls.
- Show explicit standard UIKit success and failure feedback after PhotoKit add operations.
- Keep every layout feasible with programmatic UIKit, Auto Layout, iOS 15, iPhone-only deployment, and portrait-only orientation. Use SwiftUI only for the WidgetKit target where appropriate.
- Use native controls and navigation where practical. Support Dynamic Type, a logical VoiceOver order, meaningful labels and values, 44×44 point touch targets, sufficient light/dark contrast, and non-color status cues.
- Keep the Lock Screen widget glanceable and the notification content compact. Do not design them as substitute application screens.

## Workflow

### Phase 1: Validate the accepted specification

Summarize without redefining:

- product name, `com.idev.<product-slug>` bundle identifier family, and primary scenario;
- permission-centered MVP and all 10 authorization mappings;
- exact permission and pre-permission copy;
- denial behavior and data handling;
- Yandex Cloud backend, `cloudSyncEnabled` fallback and disabled behavior, AppMetrica event inventory and per-application SDK key status, ATT, widget, and notification-extension roles;
- platform, orientation, general-audience, and review-risk constraints.

Resolve contradictions with the user before rendering. Do not continue from an internally inconsistent specification.

### Phase 2: Prepare the design

Provide:

1. Navigation map and minimum screen inventory.
2. Mapping from every accepted feature and permission trigger to a screen and control.
3. Low-fidelity wireframes for every distinct screen or important state.
4. The single startup screen plus main, empty, loading, error, permission-not-determined, denied, limited, authorized, and compact `cloudSyncEnabled` disabled/offline states where relevant. Specify the startup background, artwork source, sizing, placement, corner treatment, text and indicator choice, transition, approved 0.7-second minimum, 2-second maximum, and timeout fallback.
5. The exact action that triggers each system permission request and the approved copy reproduced verbatim.
6. Widget and notification-content layouts plus relevant notification actions.
7. A compact visual system covering colors, typography, spacing, buttons, cards, icons, and approved light/dark behavior.
8. An accessibility specification covering Dynamic Type, VoiceOver names and order, grouping, touch targets, contrast, and non-color cues.
9. One original square app-icon direction consistent with the approved name and visual system. Keep it recognizable without small text or pre-rounded corners. Do not use an SF Symbol, Apple logo, Apple product glyph, or confusingly similar mark as the app icon or logo.

Run a compact HIG, accessibility, permission-flow, and implementation-feasibility preflight before asking for approval. Fix conflicts between the written design and accepted specification first.

### Phase 3: Render and present

Use the available image-generation capability to create a rendered portrait iPhone mockup for every distinct user-facing screen. At minimum, always render the primary screen. A readable portrait-oriented composite artboard is acceptable when several screens are simple. Do not render landscape, iPad, Mac, or Apple Vision variants.

Render one square app-icon master and show it at full size and a small Home Screen-like preview. Treat mockups as review artifacts, not proof of implementation. Keep every rendered element consistent with the written screen inventory and feasible in programmatic UIKit.

Store approved candidates in the application workspace under `Design/` using clear filenames such as `mockup.png` and `app-icon.png`, and display them inline in the conversation. Do not add design assets inside the skill folder.

### Phase 4: Approval gate

Ask the user to approve the screen structure, startup appearance, timing and fallback behavior, feature placement, permission triggers and denied states, visual system, widget and notification layouts, and app icon. **Stop and wait. Do not start implementation.**

After explicit approval, update `AppSpec.md` with:

- `design-status: approved`;
- approved screen inventory and navigation;
- approved startup appearance, minimum and maximum duration, initialization work, feature-flag timeout fallback, and cold-launch-only behavior;
- permission-to-screen and state mapping;
- visual system and accessibility behavior;
- widget and notification layouts;
- paths to approved mockups and app-icon source;
- approval-dependent decisions and remaining pending items.

Do not mark the design approved based on silence or partial feedback. After recording approval, propose `build-ios-app-concept`.

## Handoff

Lead with what the user approved. Provide clickable paths to `AppSpec.md`, rendered mockups, and the app-icon master. List remaining pending decisions and state explicitly that no Xcode project or application code was created by this skill.
