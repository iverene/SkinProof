# SkinProof Design System and Product Experience

**Document:** `DESIGN.md`  
**Product:** SkinProof  
**Status:** Implementation-ready design specification  
**Audience:** Product, design, frontend, backend, and demo teams  
**Companion document:** [`REQUIREMENT.md`](./REQUIREMENT.md)

---

## 1. Design intent

SkinProof is a personal skin-tracking product, not a diagnostic portal, beauty storefront, or generic AI dashboard. The experience should make a user feel that their information is being handled carefully and that their progress can be understood without needing specialist vocabulary.

The visual direction combines:

- **Soft editorial wellness:** warm ivory, restrained sage, generous whitespace, tactile imagery, and refined typography.
- **Clinical-grade clarity:** explicit dates, source labels, metric definitions, predictable layouts, and honest uncertainty.
- **Personal progress:** a calm, memorable Skin Change Timeline that connects measured scans with the user's own routine and reaction notes.

Every screen should communicate at least one of these promises:

1. **Trust:** “I can see where this information came from.”
2. **Personalization:** “This is about my skin and my records.”
3. **Progress:** “I can understand what changed over time.”

### 1.1 Design north star

> **Better insights. Healthier skin. A more confident you.**

This is an encouragement, not a health outcome guarantee. Product copy must remain informational and must not imply diagnosis, treatment, or product causation.

### 1.2 Primary product moment

The signature moment is the transition from a scan result to the **Skin Change Timeline**:

1. The user sees the latest measured result.
2. The user selects or is invited to compare it with an earlier scan.
3. The user sees compatible metric changes with dates and plain-language explanations.
4. Routine and reaction events appear as context, clearly marked as self-reported.
5. The user understands what is recorded and what remains unknown.

The design should make this journey feel rewarding without overstating what the data proves.

---

## 2. Experience principles

### 2.1 Calm, not clinical

Use quiet surfaces, low visual noise, and restrained color. The application can be precise without looking like a hospital portal. Avoid alarm-colored dashboards, dense tables, and medical symbols unless a meaningful warning requires them.

### 2.2 Evidence before interpretation

Show the actual provider metric, its date, and its source before offering a summary. Use “recorded metric” and “reported observation” language. Never invent an overall score or trend from data the API does not provide.

### 2.3 One obvious next action

Every screen has one primary action:

| Screen | Primary action |
|---|---|
| Sign-in | Continue with Google |
| Dashboard | Analyze my skin |
| Skin Scan | Submit for analysis |
| Processing | Wait for analysis / retry if failed |
| Results | View change over time |
| History | Compare two scans |
| Timeline | Add a routine note |
| Routine Journal | Add product or reaction |
| Profile | Save changes |

Secondary actions should be visually quieter and should not compete with the primary action.

### 2.4 Personal, not performative

Use the user's name, scan dates, selected goals, and recorded events where available. Do not create fake personalization through arbitrary recommendations or an AI chat bubble on every screen.

### 2.5 Progress without judgment

Charts should describe change, not grade the user. A metric difference is not automatically an improvement or deterioration. Reserve red and amber for actual warnings, failed states, or user-reported discomfort—not ordinary metric values.

### 2.6 Transparent boundaries

The UI must distinguish:

- **AI analysis:** returned by YouCam and normalized by SkinProof.
- **User-reported:** entered by the user in the routine journal.
- **Product guidance:** general, evidence-bounded next steps.
- **Unavailable:** not returned, incompatible, or not enough data.

These distinctions are part of the product's trust model and should never be hidden in a tooltip only.

---

## 3. Visual identity

### 3.1 Color tokens

The palette is intentionally small. Most of the interface should use warm neutrals; color is reserved for hierarchy, action, and meaningful status.

| Token | Hex | Role | Usage |
|---|---|---|---|
| `sage-500` | `#759581` | Primary action | Buttons, active progress, selected states |
| `forest-700` | `#29483D` | Brand/deep emphasis | Logo, headings, navigation, strong text |
| `ivory-50` | `#FAF9F6` | Application background | Main page background |
| `white` | `#FFFFFF` | Raised surfaces | Cards, dialogs, inputs, navigation surfaces |
| `rose-300` | `#D6A29A` | Secondary highlight | Small accents, selected reaction context, editorial detail |
| `slate-500` | `#68746D` | Secondary text | Labels, descriptions, metadata |
| `charcoal-900` | `#1F2A26` | Body text | Primary readable text |
| `line-200` | `#E4E8E3` | Borders | Card and input borders |
| `sage-100` | `#E9F0EB` | Soft action background | Selected cards, informational callouts |
| `warning-500` | `#B7792D` | Caution | Provider or validation warnings |
| `danger-600` | `#B4534B` | Error | Failed states, destructive actions |
| `success-600` | `#3F7D5B` | Confirmed success | Saved state, successful completion |

#### Color rules

- Use `forest-700` or `charcoal-900` for text, never sage alone for body copy.
- Use `sage-500` for primary actions, not for every decorative element.
- Use muted backgrounds (`sage-100`, ivory, white) to create grouping before adding more color.
- Do not use red/green as the only way to communicate metric meaning.
- Pair every status color with an icon, label, or text explanation.
- Check contrast for text, borders, focus rings, and disabled states.

### 3.2 Typography

Use one editorial display style and one highly readable interface style. If the chosen font is not available, preserve the hierarchy with system fallbacks.

Recommended stack:

```css
--font-display: "Fraunces", "Iowan Old Style", Georgia, serif;
--font-body: "Inter", "Helvetica Neue", Arial, sans-serif;
```

The display face is for brand moments and large page headings only. Body copy, metric labels, buttons, forms, and data must use the body face for legibility.

| Token | Size / line height | Weight | Usage |
|---|---:|---:|---|
| `display-xl` | 48 / 1.05 | 500 | Landing hero or major editorial moment |
| `display-lg` | 36 / 1.1 | 500 | Auth and empty-state headline |
| `heading-xl` | 28 / 1.2 | 650 | Page title |
| `heading-lg` | 22 / 1.25 | 650 | Section title |
| `heading-md` | 18 / 1.3 | 650 | Card title |
| `body-lg` | 17 / 1.55 | 400 | Introductory copy |
| `body-md` | 15 / 1.5 | 400 | Default UI copy |
| `body-sm` | 13 / 1.45 | 400 | Supporting text and metadata |
| `label` | 12 / 1.3 | 650 | Uppercase or tracked labels |
| `metric-xl` | 40 / 1 | 650 | Prominent metric value |

Do not use all-caps for sentences. Letter spacing should be subtle and reserved for small labels.

### 3.3 Spacing and shape

Use an 8-point spacing system:

```text
space-1  = 4px   (icon-to-label adjustments)
space-2  = 8px   (tight control gaps)
space-3  = 12px  (compact content gaps)
space-4  = 16px  (default component padding)
space-5  = 24px  (card and section gaps)
space-6  = 32px  (large component padding)
space-7  = 40px  (page rhythm)
space-8  = 48px  (major section spacing)
space-10 = 64px  (hero and desktop separation)
```

Recommended shape tokens:

```text
radius-sm = 8px     inputs, compact controls
radius-md = 14px    cards, buttons
radius-lg = 20px    feature cards, image panels
radius-xl = 28px    editorial hero surfaces
radius-full        pills, avatars, circular progress
```

Prefer a 1px `line-200` border and a very soft shadow over heavy elevation:

```css
box-shadow: 0 10px 30px rgba(41, 72, 61, 0.06);
```

Use shadows only to establish a surface hierarchy. Do not apply a shadow to every card.

### 3.4 Imagery and illustration

- Use close, natural, well-lit skin imagery with neutral backgrounds.
- Avoid retouched “perfect skin” imagery that implies an expected result.
- Keep imagery in rounded containers with a quiet border.
- Use the leaf motif sparingly as a brand accent, not as repeated decoration.
- Do not use external image URLs in production without an approved asset and license.
- When a demo fixture is used, label it as demo data.

---

## 4. Information architecture

### 4.1 Primary navigation

Desktop navigation is a fixed left sidebar. Mobile navigation is a compact bottom bar.

```text
Dashboard
Skin Scan
History
Routine Journal
Profile
```

Use `lucide-react` icons paired with visible labels on desktop. The active item uses a sage-tinted background and forest text. Icons must not be the sole label.

### 4.2 Route model

The concrete router may vary, but the information architecture should preserve these destinations:

```text
/                       public landing / sign-in
/onboarding             first-run profile and consent orientation
/dashboard              authenticated home
/scan                   new scan flow
/scans/:scanId          scan detail
/history                chronological history
/compare                two-scan comparison
/timeline               Skin Change Timeline
/routine                routine journal
/profile                profile, privacy, and account controls
```

The dashboard may preview history and timeline content, but those previews must link to the full destination.

### 4.3 Page shell

Authenticated desktop pages use:

```text
┌──────────────┬─────────────────────────────────────────────┐
│ fixed nav    │ top context row                              │
│ 240–264px    ├─────────────────────────────────────────────┤
│              │ page title + primary action                  │
│              │ content grid / feature panels                │
└──────────────┴─────────────────────────────────────────────┘
```

- Sidebar width: 240–264px.
- Content max width: 1280px.
- Main content padding: 32px desktop, 24px tablet, 16px mobile.
- On mobile, replace the sidebar with a fixed bottom navigation bar and reserve safe-area padding.
- Avoid placing important content behind a fixed navigation element.

---

## 5. Component foundations

### 5.1 Buttons

**Primary:** sage fill, white text, medium radius, clear hover and focus state.  
**Secondary:** white or transparent surface with forest text and border.  
**Tertiary:** text-only forest action for low-priority navigation.  
**Destructive:** danger outline or fill only for deletion confirmation.

Button requirements:

- Minimum touch target: 44 × 44px.
- Include a visible disabled state.
- Include an inline spinner and “Analyzing…” or equivalent label during async work.
- Prevent duplicate submission while a request is pending.
- Never use a loading animation without a textual state.

### 5.2 Cards and panels

Cards group related information; they should not turn every data point into a separate floating object.

Card anatomy:

```text
eyebrow / source label
title
metric or content
supporting explanation
optional action
```

Use feature cards for the latest scan and timeline preview. Use flatter list rows for history and routine entries.

### 5.3 Inputs and forms

- Labels are always visible; placeholders are not labels.
- Use helper text for file requirements, consent, or metric limitations.
- Show validation text close to the invalid field.
- Preserve entered values when a submission fails.
- Use a confirmation step for destructive actions.
- For dates, show the formatted local date while retaining an accessible native date input where possible.

### 5.4 Status and source badges

Status badges are short, not decorative:

```text
AI analysis
Self-reported
Demo data
Unavailable
Saved
Processing
```

Use a small icon and text. Never present “AI analysis” as a medical authority.

### 5.5 Avatars and identity

Use the Google profile image when available; otherwise use a quiet initials avatar. Do not enlarge the avatar beyond its informational role. The user's name is more useful than a decorative portrait.

### 5.6 Tooltips and definitions

Use an info button or expandable “What does this mean?” disclosure for metric definitions. Important meaning must also exist in visible or screen-reader-accessible text; tooltips cannot be the only source of explanation.

---

## 6. Screen specifications

## 6.1 Landing and sign-in

### Purpose

Set an emotional promise, establish credibility, and move the user into the product with minimal friction.

### Layout

Desktop:

- Two-column composition.
- Left: brand mark, editorial headline, concise value proposition, three trust points.
- Right: elevated sign-in card with leaf illustration or approved product image.

Mobile:

- Brand and headline first.
- Sign-in card follows, with no unnecessary split-screen crop.
- Keep the Google action visible without scrolling if practical.

### Content

Recommended headline:

> Know your skin. Track your changes.

Supporting copy:

> SkinProof brings your scan results, routine notes, and progress into one calm, personal view.

Trust points:

- Clear, dated analysis results.
- Personal routine and reaction journal.
- Progress you can review over time.

Primary action: **Continue with Google**.

Required supporting links or disclosures:

- Informational-use notice.
- Privacy link or disclosure.
- External processing notice before the scan flow, not hidden after upload.

### States

- Default.
- Google sign-in in progress.
- Sign-in cancelled.
- Provider/authentication error.
- Network unavailable.

Do not show the authenticated dashboard briefly before auth resolution completes.

## 6.2 Onboarding and profile setup

### Purpose

Orient a new user without creating a long questionnaire.

### Content

- Welcome message using the Google display name when available.
- Optional skin goals as selectable chips.
- Short explanation of what a scan does and does not mean.
- Link to image processing and retention explanation.
- Continue to dashboard.

Goals are preferences, not diagnoses. Examples may include “texture,” “dryness,” or “routine consistency,” but the final list should reflect supported product behavior.

### Empty/default behavior

The user may skip optional goals. The dashboard must remain useful without them.

## 6.3 Dashboard

### Purpose

Answer immediately: **What is happening with my tracking, and what can I do next?**

### Desktop layout

```text
┌────────────────────────────────────────────────────────────┐
│ Good morning, Alex                         profile / alert │
│ Your latest scan · 12 May 2026                             │
├─────────────────────────────┬──────────────────────────────┤
│ latest scan feature card     │ Skin Change Timeline preview │
│ metric summary + scan CTA    │ recent events + view all    │
├─────────────────────────────┴──────────────────────────────┤
│ recent scans              │ routine shortcuts               │
└────────────────────────────────────────────────────────────┘
```

### Required content

- Greeting and latest scan date.
- Prominent **Analyze my skin** button.
- Latest result summary using actual available metrics.
- Small progress/timeline preview.
- Recent scans.
- Routine journal shortcut.
- Alert or profile entry.

### First-scan empty state

Headline:

> Your skin story starts here.

Copy:

> Complete your first scan to create a dated baseline. You can add routine notes anytime.

Primary action: **Analyze my skin**.

Do not show invented scores, improvement percentages, or placeholder medical language.

### Dashboard loading state

Use skeletons that preserve the card layout. Avoid a full-page spinner when navigation and shell content are available.

## 6.4 Skin Scan

### Purpose

Make image submission guided, explicit, and trustworthy.

### Flow

```text
1. Capture guidance
2. Choose or capture image
3. Preview and validate
4. Read consent
5. Submit
6. Processing
7. Results or recoverable error
```

### Capture guidance

Present a short checklist:

- Use even, natural lighting.
- Keep the camera at a consistent distance for repeat scans.
- Face the camera directly.
- Avoid sunglasses, heavy occlusion, or strong filters where practical.

These suggestions reduce variation; they do not guarantee comparable results.

### Upload panel

- Large image drop zone on desktop.
- Camera-friendly file input on mobile.
- Clear accepted format and maximum size text, driven by the selected YouCam endpoint.
- Preview with replace and remove controls.
- Accessible file error message.

### Consent

Consent must appear before the upload is sent. Explain:

- The image is transmitted to YouCam for skin analysis.
- What SkinProof stores after a successful analysis.
- Whether SkinProof retains the original image.
- That results are informational and not a medical diagnosis.

Use an explicit checkbox or equivalent deliberate confirmation. Do not hide consent in a disabled button label.

### Processing state

Use honest progress language:

> Your image was sent securely. We’re waiting for the analysis result.

Show an indeterminate progress indicator only for waiting. If the provider exposes stages, label only real stages. Do not use a fake face scan, measuring lines, or percentage that implies unsupported work.

Disable the submit action while pending. Provide a safe retry if the task fails or times out.

## 6.5 Analysis Results

### Purpose

Present the provider output as understandable, dated evidence and lead naturally to comparison.

### Layout

Top:

- Back/history link.
- “Skin Analysis Results” title.
- Scan date and source badge.

Primary panel:

- A calm summary of available metrics.
- No invented overall score.
- A short informational disclaimer.

Metric grid:

- 2–4 columns desktop, 1–2 columns mobile.
- Each card contains metric name, value, unit or scale if supplied, source, and plain-language explanation.
- Missing or unsupported values are omitted or labeled “Not available.”

Actions:

- **Compare with another scan** (primary when a second compatible scan exists).
- **View full report**.
- **Add a routine note**.

### Metric card structure

```text
AI ANALYSIS
Redness
32
Returned on 12 May 2026
Shows the recorded redness metric for this scan.
What does this mean?  >
```

Do not assign “good,” “bad,” “healthy,” or “unhealthy” unless the provider explicitly supplies a defensible interpretation and it is represented accurately.

### Result disclaimer

Use concise, persistent language:

> SkinProof records visible skin metrics for personal tracking. This result is informational and is not a medical diagnosis.

## 6.6 Scan history

### Purpose

Make repeat use and retrieval effortless.

### Layout

- Chronological list, newest first.
- Each row includes date, thumbnail only if retention is explicitly supported, selected metrics, and status/source.
- Desktop may use a two-column history/detail layout.
- Mobile uses stacked rows with a clear tap target.

### Empty state

> No saved scans yet.

Copy:

> Your completed scans will appear here so you can review and compare them over time.

Primary action: **Analyze my skin**.

### Filtering and pagination

Add filters only when they solve a real retrieval problem. Do not introduce a filter bar on a short history list. Use bounded queries and preserve the newest-first order.

## 6.7 Compare scans

### Purpose

Show what is different between two dated records without implying why it changed.

### Selection

- Two clearly labeled selectors: **Earlier scan** and **Later scan**.
- Prevent selecting the same scan twice.
- Explain why a metric may be unavailable if the scans are from incompatible schemas or endpoints.
- Default to the two most recent compatible scans when a sensible default exists, but let the user change it.

### Comparison layout

Desktop:

```text
Earlier scan                    Later scan
12 Apr 2026                     12 May 2026
metric value                    metric value
             ─────────────────
             recorded difference
```

Mobile:

- Stack earlier and later values vertically.
- Keep the metric name and difference visible without horizontal scrolling.

### Difference presentation

Use neutral labels:

- “Recorded change”
- “Higher in later scan”
- “Lower in later scan”
- “Not comparable”

Do not use celebratory green arrows or alarming red arrows by default. If a user-reported concern is shown, label it separately.

### Insufficient data state

If only one scan exists:

> Comparison becomes available after your next completed scan.

If metrics do not overlap:

> These scans do not share compatible metrics, so SkinProof cannot calculate a reliable comparison.

## 6.8 Skin Change Timeline

### Purpose

Be the signature product feature: a readable chronological story that combines measured events and user context.

### Timeline anatomy

Each event contains:

```text
date / time
event type badge
event title
measured value or user note
source
optional linked action
```

Event types:

- **Scan:** sage marker and “AI analysis” badge.
- **Routine started/updated:** forest marker and “Routine journal” badge.
- **Reaction logged:** muted rose marker and “Self-reported” badge.

Use a vertical line on desktop and a single-column stack on mobile. The event marker and source label must remain legible in both layouts.

### Timeline header

- Title: **Your skin change timeline**
- Short explanation: “Review recorded scans alongside the routine and reactions you logged.”
- Date range or filter control only if there is enough history.
- Primary action: **Analyze my skin** or **Add routine note**, depending on context.

### Scan event

Show:

- Scan date.
- Compatible metric summary.
- Link to scan details or comparison.
- “AI analysis” source badge.

### Routine/reaction event

Show:

- User-entered date.
- Product or observation.
- User's note, truncated with an expand action if needed.
- “Self-reported” source badge.

Never visually merge a self-reported note into an AI metric card in a way that makes it look measured.

### Timeline summary

When two or more compatible scans exist, a summary may say:

> Between 12 April and 12 May, the recorded redness metric changed from 28 to 32. You logged a new moisturizer on 20 April. This timeline shows timing, not cause.

This wording is intentionally descriptive and non-causal.

### No-history state

> Your timeline will build as you complete scans and add journal entries.

Offer both **Analyze my skin** and **Add routine note** as clear next actions, with scan as the primary action when no scan exists.

## 6.9 Routine Journal

### Purpose

Capture useful context without turning the app into a complicated treatment planner.

### Journal sections

1. Active products/routines.
2. Recent entries.
3. Reactions and observations.

### Add product form

Fields:

- Product name (required).
- Brand (optional).
- Category (required from a supported list).
- Frequency/time of use.
- Start date.
- Notes.

Keep the form short. Use progressive disclosure for optional notes.

### Reaction form

Fields:

- Observed date/time.
- Related product (optional).
- Observation: dryness, redness, stinging, or another supported option.
- Note (optional).

Label the form:

> Your observation

Supporting copy:

> This is a personal journal entry, not an AI-confirmed finding or diagnosis.

### Product list

Use compact rows with category, frequency, and start date. Provide edit and delete actions in a contextual menu or row action area. Deletion requires confirmation and must state what will happen to linked reaction references.

## 6.10 Profile and settings

### Content

- Google identity: display name, email, profile image.
- Skin goals.
- Privacy and image-processing explanation.
- Sign out.
- Account/data deletion route.

Do not collect or display unnecessary personal information. Keep account controls predictable and visually separate from skin insights.

---

## 7. Data visualization rules

### 7.1 General rules

- Use the provider's metric names and score direction.
- Keep units, ranges, and definitions attached to the metric.
- Never treat missing data as zero.
- Do not draw a trend line from one point.
- Do not compare incompatible metric schemas.
- Always provide an accessible text equivalent for a chart.
- Keep chart color count low: one primary series plus neutral comparison context.

### 7.2 Recommended visualizations

| Data situation | Visualization |
|---|---|
| Single scan | Metric card or compact radial/linear indicator only if the API defines the scale |
| Two scans | Side-by-side values with a neutral recorded difference |
| Three or more compatible scans | Lightweight line chart with dates and text summary |
| Scan + journal context | Timeline with distinct event types |
| Missing metric | “Not available” label, never an empty-looking zero |

### 7.3 Charts and motion

Charts may animate in gently when results load, but the animation must not delay comprehension. Respect `prefers-reduced-motion: reduce`. A chart should still communicate the data when animation is disabled.

Avoid:

- 3D charts.
- Gauges that imply a medical threshold.
- Multi-color “health score” dashboards.
- Decorative chart motion during an active analysis request.

---

## 8. Responsive behavior

### 8.1 Breakpoints

Use Tailwind's standard responsive approach unless implementation constraints require otherwise:

```text
mobile: < 640px
tablet: 640–1023px
desktop: ≥ 1024px
wide desktop: ≥ 1280px
```

Design for content behavior, not only device labels.

### 8.2 Desktop

- Fixed left sidebar.
- Two-column dashboard and comparison layouts.
- Timeline can use a wide content column with contextual summary at the side.
- Keep scan image and guidance side by side when both remain comfortable.

### 8.3 Tablet

- Sidebar may compress to icon-plus-label or become a drawer.
- Reduce dashboard columns before reducing text size.
- Preserve the primary scan action near the top of the content.

### 8.4 Mobile

- Fixed bottom navigation with Dashboard, Scan, History, Routine, and Profile.
- Stack all cards and comparison values vertically.
- Make scan action prominent and reachable.
- Use full-width buttons for primary actions.
- Keep forms single-column.
- Use horizontal scrolling only for non-essential chips; never for core data or primary actions.
- Reserve bottom safe-area space for navigation.

### 8.5 Responsive priority

If space is constrained, preserve content in this order:

1. Page title and primary action.
2. Date/source and actual result.
3. Explanation and disclaimer.
4. Secondary navigation and decorative imagery.

---

## 9. Interaction and motion

Motion should make state changes easier to understand, not make the product feel busy.

### 9.1 Motion principles

- Animate opacity and transform rather than layout-heavy properties.
- Use short transitions for controls: 150–200ms.
- Use 250–400ms for cards and route-level content.
- Use a calm ease-out curve for entrances.
- Avoid bounce, shimmer overload, and looping decorative motion.
- Never simulate analysis work that did not occur.

### 9.2 Recommended transitions

| Interaction | Motion |
|---|---|
| Button hover/focus | Color and elevation transition, 150ms |
| Card reveal | Fade + 8px upward movement, 250ms |
| Timeline entry | Staggered reveal only on initial load, 250–350ms |
| Chart load | Draw/fade in once, 400ms maximum |
| Mobile navigation | Short crossfade or slide, 200ms |
| Error/success feedback | Fade/slide into reserved message area |

### 9.3 Reduced motion

When reduced motion is requested:

- Remove stagger and chart drawing.
- Keep state changes immediate or use opacity only.
- Do not remove focus, loading, or success feedback.
- Ensure the processing state still communicates that the request is active.

---

## 10. Content and voice

### 10.1 Voice

SkinProof sounds:

- Clear, warm, and grounded.
- Encouraging without promising outcomes.
- Specific rather than overly polished.
- Human, not chatty or clinical.

SkinProof does not sound:

- Diagnostic.
- Alarmist.
- Sales-led.
- Overconfident about cause and effect.
- Like a generic AI assistant.

### 10.2 Preferred language

| Prefer | Avoid |
|---|---|
| “Recorded metric” | “Your skin health score” when no such score exists |
| “You logged” | “The app detected your reaction” |
| “Changed between these scans” | “Your product improved your skin” |
| “Not enough data to compare” | “No change” when data is missing |
| “Informational, not a diagnosis” | “Clinically accurate” unless substantiated |
| “Consider speaking with a qualified professional” | “You have…” |

### 10.3 Error copy

Errors should say what happened and what the user can do next.

```text
We couldn’t analyze this image.
Check your connection and try again. Your scan was not saved as completed.
```

```text
Your scan finished, but we couldn’t save the result.
Please try again. We have not marked this scan as saved.
```

Do not expose provider stack traces, API keys, task internals, or raw payloads.

### 10.4 Empty states

Every empty state includes:

1. What is empty.
2. Why the user might care.
3. One primary action.

Avoid generic “Nothing here” messages.

---

## 11. Accessibility and inclusive design

### 11.1 Baseline

- Use semantic headings in order.
- Use landmarks for sidebar, main content, and bottom navigation.
- Provide accessible names for every icon button.
- Maintain visible keyboard focus.
- Support keyboard navigation through forms, dialogs, tabs, selectors, and timeline events.
- Do not communicate meaning by color alone.
- Keep text contrast readable on ivory, sage, and white surfaces.
- Use live regions for scan progress, save confirmation, and errors.
- Announce validation errors and preserve focus appropriately.

### 11.2 Images and scan privacy

- Provide meaningful alt text for editorial imagery.
- Treat user-submitted face images as sensitive; do not expose them in decorative background layers.
- If a scan preview is shown, give it a useful accessible label and a remove/replace action.
- Do not place private images in public URLs without an explicit retention and authorization design.

### 11.3 Charts

Every chart needs:

- A visible title.
- Metric name and units/range where supported.
- Date labels.
- A text summary of the data points and recorded change.
- A non-color distinction for series or event types.

### 11.4 Touch and readability

- Minimum 44 × 44px touch targets.
- Do not rely on hover to reveal core information.
- Support browser zoom without clipping critical content.
- Keep line lengths comfortable on desktop and readable on mobile.

---

## 12. Trust, privacy, and responsible UI

The design is part of the privacy model.

### 12.1 Consent placement

Consent belongs immediately before first submission, when the user can understand the specific action. A general privacy link on sign-in is not a substitute.

### 12.2 Source labeling

Use source labels close to content:

- `AI analysis · YouCam`
- `Self-reported · Routine journal`
- `Demo data`

If the exact provider metric definition is not available, show a conservative explanation and link to a fuller report or help text.

### 12.3 Data retention cues

If original images are not retained, say so accurately. If temporary processing is used, describe it without promising behavior controlled by YouCam. The interface must never imply that a scan image is permanently stored if it is not.

### 12.4 Recommendations

Recommendations should be deterministic and evidence-bounded in the MVP:

- Review recent routine notes.
- Keep the routine simple when investigating irritation.
- Avoid adding multiple new products at once.
- Consider qualified professional advice for persistent or concerning symptoms.

Any future LLM-generated guidance must be visually and semantically subordinate to measured data, grounded in validated inputs, and clearly labeled as general guidance.

---

## 13. Demo and realistic data

The judging experience should feel complete while remaining honest.

### 13.1 Demo data rules

- Use realistic but clearly labeled fixture data for secondary screens when live history is unavailable.
- Never present fixture data as a real API result.
- Use dates that are easy to understand during a live demo.
- Keep a visible `Demo data` badge in the demo-only path.
- Prefer a small, coherent history over many invented records.

### 13.2 Recommended demo narrative

1. Sign in with Google.
2. Land on the calm dashboard with a clear **Analyze my skin** action.
3. Upload an image and show capture guidance and consent.
4. Show the honest processing state.
5. Display actual YouCam metrics with source labels and explanations.
6. Open history and compare two dated scans.
7. Add a routine or reaction entry.
8. Open the Skin Change Timeline and show measured versus self-reported events.
9. Explain privacy, non-diagnostic boundaries, and the product differentiator.

Do not spend the demo on decorative interactions that do not support this story.

---

## 14. Implementation guidance

### 14.1 Suggested frontend organization

Align visual components with product features rather than creating one giant dashboard component:

```text
frontend/src/
├── components/
│   ├── layout/
│   ├── navigation/
│   ├── feedback/
│   ├── metrics/
│   └── timeline/
├── features/
│   ├── auth/
│   ├── profile/
│   ├── scans/
│   ├── history/
│   ├── timeline/
│   └── routines/
├── pages/
├── services/
├── hooks/
├── lib/
└── utils/
```

Keep provider response parsing and metric normalization outside presentational components. A metric card should receive a normalized, typed-by-convention object and should not know the YouCam response shape.

### 14.2 Shared component contracts

Useful shared components include:

- `AppShell`
- `Sidebar`
- `MobileBottomNav`
- `PageHeader`
- `PrimaryAction`
- `MetricCard`
- `MetricDefinition`
- `SourceBadge`
- `ScanStatus`
- `ScanComparison`
- `TimelineEvent`
- `EmptyState`
- `AsyncState`
- `ConsentPanel`
- `ConfirmDialog`

Components should expose accessible labels and state props rather than relying on CSS-only meaning.

### 14.3 Data boundaries

- Normalize provider metrics once, near the API boundary.
- Store only fields needed by the product and comparison logic.
- Keep scan status distinct from completed result data.
- Treat timestamps as timestamps until formatting at the display boundary.
- Do not let mock fixtures share a silent code path with live provider data.

### 14.4 Performance

- Load dashboard summaries with bounded queries.
- Do not download the entire scan history for the dashboard.
- Lazy-load heavy charts or report sections when useful.
- Resize images only if compatible with the selected YouCam endpoint.
- Keep the scan processing request off the main UI thread where possible.

---

## 15. Design acceptance checklist

### Visual system

- [ ] Warm ivory background, white surfaces, sage action color, forest emphasis, and slate secondary text are consistently applied.
- [ ] Typography has a clear display/body distinction.
- [ ] Spacing follows the 8-point system.
- [ ] Cards use borders and restrained shadows.
- [ ] No excessive gradients, decorative badges, or competing colors.

### Product flow

- [ ] A new user understands the value proposition quickly.
- [ ] The dashboard has one obvious primary action.
- [ ] Scan guidance and consent appear before submission.
- [ ] Processing state is honest and prevents duplicate submission.
- [ ] Results use actual available API metrics and do not invent an overall score.
- [ ] History and comparison show dates and compatible metrics.
- [ ] Timeline distinguishes AI analysis from self-reported entries.
- [ ] Routine journal is practical and short.

### Responsive behavior

- [ ] Desktop uses a fixed left sidebar.
- [ ] Mobile uses compact bottom navigation.
- [ ] Scan upload and comparison remain usable on narrow screens.
- [ ] No essential content requires horizontal scrolling.
- [ ] Fixed navigation does not cover page content.

### Accessibility

- [ ] Keyboard focus is visible.
- [ ] Icon buttons have accessible names.
- [ ] Status and errors are announced in text.
- [ ] Charts have text summaries.
- [ ] Color is not the only status indicator.
- [ ] Reduced-motion preferences are respected.

### Trust and responsibility

- [ ] Source labels are visible near metrics and journal content.
- [ ] Informational/non-diagnostic language is present.
- [ ] No product-causation claims are made.
- [ ] Privacy and image-retention behavior is accurately described.
- [ ] Demo fixtures are clearly labeled.

---

## 16. Design decisions to verify during implementation

These decisions should be confirmed against the selected YouCam endpoint and the implemented data model before final UI polish:

1. Which metrics are returned, their ranges, units, and score direction.
2. Whether the provider returns a defensible aggregate score; default design assumes it does not.
3. Whether scan thumbnails or original images can be retained; default design assumes original images are not retained.
4. The provider's exact processing lifecycle and truthful progress states.
5. Which metrics are compatible across endpoint versions and schema versions.
6. Whether the timeline filters need to support large histories in the MVP.
7. The final font licensing and asset sources.
8. The exact breakpoint and navigation behavior after testing on target devices.

When implementation constraints conflict with this document, preserve the underlying principles: show the source, explain the limitation, keep the next action obvious, and never claim more than the data supports.