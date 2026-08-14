---
name: generate-ios-app-ideas
description: Generate and refine user-provided or new general-audience portrait-only iPhone iOS application concepts that avoid age restrictions and high-risk App Store review topics; must actually use Bluetooth, camera, contacts, Face ID, location, microphone, separate photo-library read and add access, advertising tracking, and push notifications; keep the user-facing MVP centered on those permissions with minimal extra scope; preferably have understandable Russian or Latin names built around one of the patterns «YP», «Y P», or «Я П»; warn that compact «ЯП» and «YaP»/«Ya P» variants can attract unnecessary attention to the naming; include an iPhone Lock Screen widget and push notification extensions; and require only a minimal backend. Use when the user brings an app idea to develop or asks for portrait-only iPhone app ideas, product concepts, names, permission copy, extension scenarios, or MVP backend outlines with these constraints, or explicitly invokes $generate-ios-app-ideas.
---

# Generate iOS App Ideas

## Goal

Generate coherent, buildable portrait-only iPhone iOS product ideas in which every required capability has a natural role. Center the MVP on the smallest coherent set of permission-backed user actions. Allow the supporting navigation, state, configuration, analytics, extensions, backend, and error handling required to make those actions work, but do not invent unrelated product features. Keep this skill limited to ideation and product specification. Do not create projects, source files, designs, or backend code unless the user separately requests implementation.

## Inputs

Treat any idea supplied by the user as the primary creative direction. Preserve its core problem, audience, theme, and accepted features while adapting it to satisfy the constraints. Concentrate on developing that idea before contributing original alternatives.

When the user supplies an idea:

- Refine and complete that idea instead of replacing it.
- Proactively suggest concrete improvements to the user's idea when they make it clearer, more coherent, easier to implement, or safer for App Store review.
- When a nearby variation appears materially stronger, present it as an optional close alternative after refining the user's idea. Keep it recognizably based on the user's direction and explain the advantage briefly.
- Resolve missing details with reasonable assumptions.
- Point out a conflict only when a required capability cannot be integrated credibly.
- When the supplied idea is age-gated or based on a high-risk review topic, briefly flag the conflict and adapt its useful core into a general-audience concept instead of following the risky element literally.
- Do not replace the user's direction with unrelated concepts. Generate broader alternatives only when the user explicitly requests them.
- After refining the user's idea and any useful close alternatives, always offer broader alternative generation as an optional next step, even when the current idea is feasible.
- When the user's idea spans multiple Apple platforms or depends on landscape layout, preserve and refine only the iPhone portion and adapt its flows to portrait orientation. Do not add companion experiences for other devices.

Use any theme, audience, number of ideas, tone, or naming preference supplied by the user. When no idea is supplied:

- Generate 5 distinct ideas.
- Generate 5 name variants for each idea.
- Prefer consumer applications with a small, credible MVP.
- Answer in the user's language.

Do not block on missing creative preferences. Make reasonable assumptions and state only assumptions that materially affect the result.

## Hard constraints

Make every idea satisfy all of the following:

1. Actually use every permission or authorization listed below through a reachable, intentional user action. Do not merely declare a purpose string.
2. Keep the user-facing MVP centered on the smallest coherent set of features needed to exercise all required permissions. A single feature may cover several permissions.
3. Do not add unrelated secondary product features. Supporting UI and technical code needed for navigation, state, configuration, analytics, extensions, backend interaction, permission recovery, and error handling are allowed and should be minimal.
4. Use the shared, deliberately general permission copy without adding product-specific details.
5. Include a Lock Screen widget, Notification Service Extension, and Notification Content Extension.
6. Include the smallest backend that can demonstrate one real client-server interaction, synchronization, and push-token handling. Prefer merging responsibilities when that reduces code.
7. Keep the concept suitable for a general audience without age gates or content likely to create a material App Store review obstacle.
8. Make the product exclusively an iOS app for iPhone. Do not propose iPad or iPadOS support, macOS, Mac Catalyst, Designed for iPhone/iPad on Mac distribution, visionOS or Apple Vision, watchOS, tvOS, or cross-platform and companion apps. Treat the widget and notification extensions as parts of the iPhone app.
9. Make the iPhone application portrait-only. Keep every core flow usable without device rotation; do not propose landscape-only screens, interfaces, or features. Treat Lock Screen widget and notification layouts as portrait-oriented surfaces.

Treat the naming pattern as a strong preference, not a hard constraint. Prefer names containing or clearly expanding to «YP», «Y P», or «Я П», but keep a stronger natural name when forcing the pattern would make it unclear or awkward.

Reject an idea internally and replace it when a permission, extension, or backend exists only to check a box. Avoid duplicating the same product with superficial theme changes.

Avoid concepts centered on adult or sexual content, dating, gambling or betting, alcohol, tobacco, drugs, graphic violence, weapons, hate or extremism, self-harm, anonymous or unmoderated user-generated content, medical diagnosis or treatment claims, real-money speculation, crypto investment, surveillance, stalking, or illegal activity. Avoid requiring identity or age verification merely to access the core product. Treat these rules as review-risk reduction, not a guarantee of App Store approval.

## Naming rules

Make names concrete enough that a user can infer the theme.

Aim for most name options to visibly contain or expand to one of these exact uppercase patterns: `YP`, `Y P`, or `Я П`. Allow some options outside these patterns when they are clearer, more memorable, or supplied and preferred by the user.

Do not propose compact `ЯП` or Latin `YaP`/`Ya P`/`YA P` constructions. If the user arrives with a name using one of them, preserve it only as the user's working option, explicitly note that this form can draw unnecessary attention to the naming, recommend avoiding it, and offer nearby variants using `YP`, `Y P`, `Я П`, or a natural name without a pattern. Treat this as naming advice, not a reason to reject the underlying idea.

Prefer one of these constructions:

- Compact Latin construction with `YP`: «YP Travel».
- Latin phrase whose prominent words expand to `Y P`: «Your Plan».
- Russian phrase expanding to `Я П`: «Я Путешествую», «Я Планирую», «Я люблю Падел».

For names using a preferred pattern, emphasize its letters with bold Markdown and briefly decode the construction, for example `**Y**our **P**lan` or `**Я** люблю **П**адел`. Keep the target letters uppercase in the displayed name. Present an exceptional name normally and briefly explain why it is stronger without the pattern. Do not force nonsensical grammar merely to obtain the pattern.

## Shared permission copy

Use these Russian texts verbatim unless the user explicitly requests another language or asks to revise the copy:

| Capability | Purpose text |
|---|---|
| Bluetooth | «Для подключения к устройствам поблизости» |
| Камера | «Для съёмки фото, видео и сканирования» |
| Контакты | «Чтобы находить знакомых и делиться с ними» |
| Face ID | «Для быстрого и безопасного входа» |
| Геолокация | «Для работы функций, использующих ваше местоположение» |
| Микрофон | «Для записи звука и использования голосовых функций» |
| Просмотр фотогалереи (`NSPhotoLibraryUsageDescription`) | «Чтобы выбирать фото и видео с устройства» |
| Сохранение в фотогалерею (`NSPhotoLibraryAddUsageDescription`) | «Чтобы сохранять фото и видео на устройстве» |
| Рекламный трекинг | «Чтобы показывать более подходящие предложения и оценивать рекламу» |
| Push-уведомления | «Чтобы сообщать о важных событиях и изменениях» |

Treat the push text as copy for an in-app pre-permission screen. Explain only once that iOS owns the text of the system notification authorization alert. Keep photo-library read access and add-only access separate in the permission copy and in every functional rationale; do not merge them into one permission.

Mention once, without derailing the answer, that final App Store copy may need more feature-specific justification. Do not rewrite the shared copy unless requested.

## Generate each idea

For each idea, provide:

1. **Concept**: Describe the product, target user, and primary recurring action in 2–3 sentences.
2. **Names**: Give the requested number of understandable name variants following the naming rules.
3. **MVP**: List the fewest coherent user-facing functions that together exercise all required permissions. Map every function to at least one permission, allow one function to cover several permissions, and exclude unrelated or speculative secondary features.
4. **Permission rationale**: Map every required capability to a real user action in one concise sentence. Map viewing or selecting existing media separately from saving new media. Reference the shared copy instead of repeating it.
5. **Extensions**:
   - Define one glanceable Lock Screen widget using WidgetKit. Use ActivityKit only when the concept has a genuinely live, time-bound state.
   - Define a Notification Service Extension that enriches, decrypts, filters, or attaches media to remote notifications.
   - Define a Notification Content Extension with a compact custom notification interface and at least one relevant action.
6. **Minimal backend**:
   - Name the smallest useful state or entity set.
   - Prefer only `GET /health` and one idea-specific `POST /sync` operation. Let `/sync` accept and return one compact snapshot and carry an installation identifier, lightweight authentication value, and APNs device token when practical.
   - Add or split endpoints only when the concept cannot demonstrate its required client-server or push flow with the merged operation. Keep the total to 1–3 endpoints.
   - Use memory or one JSON file. Avoid separate authentication, device-registration, CRUD, database, queue, admin, or recommendation layers unless they are unavoidable.
7. **Fit check**: State in one line why all permissions, extensions, and backend belong to the same product rather than being decorative.
8. **Review safety**: State in one line why the concept is appropriate for a general audience and does not depend on age-gated or high-risk content.

## Quality checks

Before answering, silently verify:

- Every idea covers all 10 required permission or authorization categories, counting photo-library read and add access separately.
- Every required permission is exercised by a reachable user action, not only listed in configuration.
- Every user-facing MVP feature supports at least one required permission; supporting technical behavior is allowed but minimized.
- Aim for most name options to contain or expand to `YP`, `Y P`, or `Я П`; allow exceptions when they are clearer or explicitly preferred by the user.
- Do not generate `ЯП`, `YaP`, `Ya P`, or `YA P` name constructions; when supplied by the user, warn about unnecessary attention to the naming and offer a nearby replacement.
- Names are understandable and related to the concept.
- Every idea contains all 3 required extension types.
- The backend is minimal, preferably `/health` plus one merged `/sync`, and exposes a concrete reason to exist.
- The concepts differ in audience and recurring behavior, not just visual theme.
- Every concept is general-audience, avoids age gates, and is not built around a high-risk App Store review topic.
- Every concept is explicitly iPhone-only and portrait-only, remains fully usable without rotation, and contains no iPad, Mac, Apple Vision, watch, TV, or cross-platform product surface.
- No implementation work has been performed.

## Response shape

When the user brings an idea, lead with the refined version of that idea. Proactively include worthwhile improvements and, when genuinely stronger, a small clearly marked set of close alternatives derived from the same direction; never let them displace the user's concept. End with one concise offer to generate broader alternative concepts in a follow-up, such as: «Если хочешь, могу отдельно предложить несколько более свободных альтернатив». Generate broader or unrelated alternatives only after the user accepts or explicitly asks for them. When generating ideas from scratch, lead with a short comparison list. Then expand each idea using the sections above. Put the shared permission-copy table once after the ideas, unless the user asks for copy first or requests only one idea.

When the user asks only for names, permission texts, extensions, or backend refinements, preserve the accepted parts of the existing idea and return only the requested section.

When the user selects an idea for implementation, summarize the chosen specification and propose using a separate implementation skill. Stop before writing code unless implementation is explicitly requested.
