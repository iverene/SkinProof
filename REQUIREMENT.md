# SkinProof — Software Requirements Specification

**Document:** `REQUIREMENT.md`  
**Project:** SkinProof  
**Document version:** 1.0  
**Status:** Initial implementation specification  
**Product category:** Beauty technology / Skin AI  
**Target platform:** Responsive web application  
**Primary audience:** People who want to track visible skin changes and make more informed skincare decisions  
**Hackathon integration:** YouCam Skin Analysis API

---

## 1. Project Overview

### 1.1 Product Summary

SkinProof is a responsive web application that helps users understand changes in their visible skin characteristics over time. Users sign in with Google, complete a skin analysis using the YouCam Skin Analysis API, review their results, record skincare products and reactions, and compare scans taken on different dates.

Rather than functioning as a one-time skin scanner, SkinProof connects three kinds of information:

1. **AI skin metrics** returned by YouCam.
2. **Routine and reaction records** entered by the user.
3. **Historical comparisons** that help users understand how their recorded skin metrics have changed.

The product presents understandable summaries and practical, cautious next steps. It does not diagnose skin conditions, guarantee product outcomes, or claim that a skincare product caused a change in an AI metric.

### 1.2 Problem Statement

Consumers may buy skincare products without a consistent way to assess visible changes over time. A single selfie or skin score does not explain how results have changed, which routines were followed, or whether the user experienced irritation.

SkinProof aims to make skin tracking more organized by combining repeatable AI analyses with a personal skincare journal and a clear progress timeline.

### 1.3 Product Vision

Help users move from **“What does my skin look like today?”** to **“What has changed, what have I recorded, and what is a sensible next step?”**

### 1.4 Core Value Proposition

- Track skin analysis results over time instead of viewing a single scan in isolation.
- Keep skin metrics and routine notes together.
- Compare selected scans in a clear, understandable interface.
- Make routine reviews easier without presenting AI results as medical diagnoses.
- Give users control over their account and personal records.

### 1.5 Hackathon Differentiator

The primary differentiator is the **Skin Change Timeline**: a historical view that combines dated YouCam skin metrics with user-entered routine and reaction records. The app highlights changes in recorded measurements and relevant journal entries while making clear that correlation does not establish causation.

A functional integration with the YouCam API is required. A mock-only skin analyzer does not satisfy the project goal.

---

## 2. Goals and Success Criteria

### 2.1 Product Goals

1. Provide a simple and secure Google sign-in experience.
2. Complete a real skin analysis through the YouCam API.
3. Persist scan metrics in Cloud Firestore.
4. Let users view and compare their previous scans.
5. Let users record skincare products, routines, and reactions.
6. Present clear, accessible, mobile-friendly results.
7. Protect users' personal data and avoid unsupported medical claims.

### 2.2 MVP Success Criteria

The MVP is considered functionally complete when a tester can:

- Sign in using Google.
- View and update basic profile preferences.
- Submit an image for a real YouCam skin analysis.
- See a loading state and receive a useful success or error message.
- View results derived from the actual API response.
- Save and reopen completed scan records.
- View a chronological scan history.
- Select two scans and compare available metrics.
- Create, view, edit, and delete their own routine records.
- Sign out and return later without losing saved records.
- Be prevented from reading or changing another user's private records.
- Use the application on desktop and mobile screen sizes.

### 2.3 Suggested Demo Metrics

These are evaluation targets, not claims of existing performance:

- A first-time user can understand the purpose of the product within 15 seconds.
- A typical demo user can reach the first scan results without assistance.
- Scan history and comparison are demonstrable using real API results.
- The core demo can be completed in approximately two minutes.
- No API secret is exposed in browser code or client-visible network configuration.

---

## 3. Target Users and Use Cases

### 3.1 Primary Users

People who want to monitor visible skin characteristics and organize their skincare routine. The application is designed for general consumer education and personal tracking, not clinical use.

### 3.2 Main Use Cases

| ID | Use case | Expected outcome |
|---|---|---|
| UC-01 | Create an account / sign in | User signs in with Google |
| UC-02 | Manage profile | User views or updates supported preferences |
| UC-03 | Analyze skin | User receives results from YouCam |
| UC-04 | Review a scan | User views the metrics and explanations for a scan |
| UC-05 | View scan history | User sees saved scans in chronological order |
| UC-06 | Compare scans | User compares metrics from two saved scans |
| UC-07 | Manage routine records | User records skincare products and routine details |
| UC-08 | Record reactions | User logs a self-reported skin reaction |
| UC-09 | Review timeline | User views scan changes alongside journal records |
| UC-10 | Manage account and privacy | User signs out and can initiate data deletion |

---

## 4. Scope

### 4.1 Required MVP Features

- Google authentication through Firebase Authentication.
- User profile creation and retrieval.
- Responsive dashboard.
- Image upload and validation.
- Real YouCam Skin Analysis API integration.
- API task-status handling when required by the selected endpoint.
- Normalized scan metrics saved in Cloud Firestore.
- Scan detail page.
- Chronological scan history.
- Comparison of two scans.
- Skincare routine and product records.
- Self-reported reaction notes.
- Skin Change Timeline.
- Loading, empty, success, and error states.
- Firebase security rules and backend authentication.
- Privacy notice and consent before image submission.
- README setup instructions and `.env.example`.

### 4.2 Stretch Features

These features should only be attempted after all MVP requirements work:

- LLM-assisted summaries grounded only in the user's actual metrics and notes.
- Optional reminders to log routines or schedule another scan.
- Product catalog and ingredient reference data.
- Routine consistency summaries.
- Downloadable personal progress report.
- PWA installation and limited offline access to previously loaded text records.
- Account export and self-service deletion workflow.
- Optional image retention with explicit consent and a documented deletion policy.
- Additional YouCam beauty APIs, if relevant and time permits.

### 4.3 Out of Scope for the MVP

- Medical diagnosis or treatment.
- Claims that the application can cure acne or other skin conditions.
- Prescribing medications or changing prescribed treatment.
- Claims that a product caused a metric to improve or worsen.
- Automated scraping of retail catalogs.
- E-commerce checkout or payment processing.
- Social sharing of face images or skin records.
- Training a custom computer vision model.
- Storing raw face images indefinitely.
- A multi-tenant clinician or dermatologist portal.

---

## 5. Technical Stack and Architecture

### 5.1 Required Technology

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | Vite + React + JavaScript | User interface and interactions |
| Styling | Tailwind CSS v4 | Responsive layout and visual system |
| Icons | `lucide-react` | Consistent interface icons |
| Authentication | Firebase Authentication | Google sign-in and identity |
| Database | Cloud Firestore | Profiles, scan metrics, routine records |
| Backend | Node.js + Express | Protected endpoints and business logic |
| Skin AI | YouCam Skin Analysis API | Skin metric analysis |
| Deployment | Vercel + Node.js-compatible backend host | Application hosting |
| Version control | Git + GitHub | Source control and collaboration |

Use JavaScript rather than TypeScript unless the team explicitly changes this requirement. Keep the frontend and backend in separate directories. Do not introduce Next.js or shadcn/ui by default.

### 5.2 High-Level Architecture

1. React handles the user interface and Firebase client SDK.
2. Firebase Authentication signs users in with Google.
3. The frontend obtains a Firebase ID token for protected backend requests.
4. Express verifies the ID token before processing protected requests.
5. Express calls YouCam using a server-side API credential.
6. The backend validates and normalizes the YouCam response.
7. The backend writes completed scan records to Firestore using the Firebase Admin SDK.
8. The frontend reads only records authorized for the signed-in user, either through restrictive Firestore rules or through protected backend endpoints.
9. Routine records are written through a consistent, validated access path.

### 5.3 Architectural Rules

- Never expose the YouCam API key in frontend code.
- Never place private server credentials in variables prefixed with `VITE_`.
- Never trust a user ID supplied by the browser as proof of authorization.
- Derive the authenticated user UID from the verified Firebase ID token on protected backend routes.
- Do not store the same source of truth in multiple places without a documented reason.
- Do not trust raw AI output blindly; validate and normalize fields before displaying or saving them.
- Use server timestamps for authoritative creation and update times where appropriate.
- Keep provider-specific response handling inside a dedicated YouCam service module.

---

## 6. Functional Requirements

Priority definitions:

- **P0 — Required:** Must be implemented for the MVP.
- **P1 — Important:** Implement after the core P0 path works.
- **P2 — Stretch:** Optional enhancement.

### 6.1 Authentication and Account

#### FR-AUTH-01 — Google Sign-In (P0)

The application shall allow users to sign in using Google through Firebase Authentication.

**Acceptance criteria**
- A clearly visible “Continue with Google” control is provided.
- A successful sign-in creates an authenticated session.
- A failed or cancelled sign-in displays an appropriate message.
- The application does not create duplicate profiles for repeated sign-ins by the same Firebase UID.

#### FR-AUTH-02 — Session Persistence (P0)

The application shall restore the Firebase authentication state when the user returns, according to the configured Firebase persistence behavior.

**Acceptance criteria**
- A returning authenticated user can access authorized pages.
- A signed-out user cannot access protected pages or private records.
- The interface shows an appropriate loading state while authentication is being resolved.

#### FR-AUTH-03 — Sign-Out (P0)

The application shall provide a sign-out action.

**Acceptance criteria**
- Signing out clears the active client authentication session.
- The user is redirected to a public page.
- Private application pages are not rendered as accessible content after sign-out.

#### FR-AUTH-04 — Profile Initialization (P0)

On first successful sign-in, the application shall create a profile document if one does not already exist.

#### FR-AUTH-05 — Profile Management (P1)

Users shall be able to view and update supported profile preferences, such as display name, skin goals, and preferred units or display settings if implemented.

Do not collect unnecessary personal information.

#### FR-AUTH-06 — Account Deletion (P1)

The application should provide a documented account-deletion process. Deletion must remove or arrange deletion of the user's profile, scans, routine records, and any retained images according to the documented retention policy. Firebase Authentication account deletion alone does not automatically remove Firestore documents.

### 6.2 Skin Analysis

#### FR-SCAN-01 — Image Selection (P0)

Users shall be able to select a supported image file for analysis.

**Acceptance criteria**
- The UI identifies accepted file formats and size limits.
- Unsupported formats and oversized files are rejected before submission.
- The user can preview the selected image before submission.
- The user can cancel or replace the selected image.
- Image submission requires an explicit action.

#### FR-SCAN-02 — Image Validation (P0)

The frontend and backend shall validate uploaded images.

**Minimum safeguards**
- Allow only supported image MIME types and formats.
- Enforce a configurable maximum upload size.
- Do not trust the filename or client-provided MIME type alone.
- Reject malformed or empty uploads.
- Use safe, temporary file handling and clean up temporary data.
- Never log raw image content or sensitive request payloads.

The final accepted formats, image dimensions, and size limit must follow the selected YouCam endpoint's current documentation.

#### FR-SCAN-03 — Consent Before Analysis (P0)

Before submitting an image to YouCam, the application shall explain that the image is processed by an external service for skin analysis and request the user's consent.

The application shall explain whether the image is retained by SkinProof, transmitted to YouCam, or deleted after processing. Do not promise deletion by YouCam unless the provider's current terms and API behavior support that promise.

#### FR-SCAN-04 — Submit YouCam Analysis (P0)

The backend shall submit a valid image to the selected YouCam Skin Analysis endpoint.

**Acceptance criteria**
- The request uses the documented endpoint, authentication method, required parameters, and supported image format.
- The YouCam credential is read from server-side environment variables.
- The frontend never receives the provider's secret credential.
- The backend handles rejected requests, timeouts, rate limits, and provider errors.
- Provider task IDs are treated as operational identifiers, not as user authorization.

#### FR-SCAN-05 — Handle Asynchronous Results (P0)

If the selected endpoint uses asynchronous processing, the backend shall track task status and retrieve results using the documented API flow.

**Acceptance criteria**
- The UI communicates that analysis is processing.
- The implementation avoids uncontrolled rapid polling.
- Polling or task retrieval stops when the task completes, fails, or reaches a configured timeout.
- A failed task does not create a misleading completed scan.
- A retry does not accidentally create duplicate scan records.

#### FR-SCAN-06 — Display Analysis Results (P0)

The application shall show relevant skin metrics returned by YouCam.

**Acceptance criteria**
- Values originate from the actual API response or a clearly labelled demo fixture.
- Each metric has a human-readable label and a concise explanation.
- The UI follows the provider's score direction and interpretation.
- Missing metrics are omitted or clearly marked unavailable; they are not fabricated.
- The UI distinguishes AI-generated analysis from user-reported information.
- Results include a notice that the feature is informational and not a medical diagnosis.

#### FR-SCAN-07 — Save Scan Metrics (P0)

After a successful analysis, the application shall save the normalized result to the authenticated user's scan subcollection in Firestore.

**Acceptance criteria**
- Each saved scan has a unique document ID.
- Each scan is associated with the authenticated user's UID.
- The scan includes a server-authoritative creation timestamp.
- The record contains the normalized metrics and minimal operational metadata needed for history and troubleshooting.
- Failed scans are not represented as successful completed scans.
- Sensitive raw provider payloads are not stored unless necessary, documented, and minimized.

#### FR-SCAN-08 — Scan Detail (P0)

Users shall be able to open an individual saved scan and review its metrics and date.

#### FR-SCAN-09 — Scan History (P0)

Users shall be able to view their saved scans in reverse chronological order.

**Acceptance criteria**
- An empty state explains how to create the first scan.
- Loading and error states are supported.
- The list is ordered by a reliable timestamp.
- Pagination or bounded queries are used when necessary.

#### FR-SCAN-10 — Scan Comparison (P0)

Users shall be able to select two saved scans and compare metrics that exist in both results.

**Acceptance criteria**
- The comparison shows each scan's date.
- Only metrics available in both scans are compared.
- Changes are calculated using the correct metric semantics.
- The UI does not imply that a numerical difference necessarily represents a clinical improvement.
- If a metric's scale or interpretation is unknown, the application does not assign a positive or negative meaning to its change.

#### FR-SCAN-11 — Consistent Capture Guidance (P1)

Before repeat scans, the application should suggest consistent lighting, camera distance, facial positioning, and removal of visual obstructions where practical.

These suggestions are intended to reduce variation, not guarantee comparable results.

### 6.3 Skin Change Timeline

#### FR-TIME-01 — Combined Timeline (P0)

The application shall display scan events and user-entered routine or reaction entries in chronological order.

#### FR-TIME-02 — Explain Changes (P0)

The application shall summarize observed differences between selected scans using the actual saved metrics.

**Acceptance criteria**
- The summary identifies the compared dates.
- The summary distinguishes observed metric changes from user-reported routine events.
- It does not claim a product caused an observed change.
- It explains when the available data is insufficient for a meaningful comparison.

#### FR-TIME-03 — Trend Visualization (P1)

The application should show a simple chart or trend view for selected metrics when enough compatible historical values exist.

- Avoid charts that imply a trend from only one data point.
- Label the metric and time axis.
- Make the data available in an accessible text summary.
- Handle missing dates and missing metric values accurately.

### 6.4 Routine and Reaction Journal

#### FR-ROUTINE-01 — Create Routine Record (P0)

Users shall be able to record a skincare product or routine entry.

Suggested fields:
- Product name.
- Product category, such as cleanser, moisturizer, sunscreen, or treatment.
- Optional brand.
- Frequency or time of use.
- Start date, if applicable.
- Optional notes.

#### FR-ROUTINE-02 — View Routine History (P0)

Users shall be able to view their saved routine entries.

#### FR-ROUTINE-03 — Edit Routine Record (P0)

Users shall be able to edit their own routine entries.

#### FR-ROUTINE-04 — Delete Routine Record (P0)

Users shall be able to delete their own routine entries after an appropriate confirmation step.

#### FR-ROUTINE-05 — Log Reactions (P0)

Users shall be able to record a self-reported reaction or observation, including:
- Date and time, where appropriate.
- Related product, if known.
- User-selected observation, such as dryness, redness, stinging, or no noticeable reaction.
- Optional notes.

These entries are user-reported observations and must not be represented as AI-confirmed findings or medical diagnoses.

#### FR-ROUTINE-06 — Routine Consistency Summary (P2)

The application may summarize user-recorded consistency when enough entries exist. It must not fabricate missed or completed applications.

### 6.5 Guidance and Recommendations

#### FR-GUIDE-01 — Evidence-Bounded Next Steps (P0)

The application shall provide cautious, general next-step guidance based on supported information, such as:
- Reviewing the user's recent routine notes.
- Maintaining a simple routine where tolerated.
- Avoiding the addition of multiple new products at once when trying to understand irritation.
- Seeking advice from a qualified healthcare professional for persistent, severe, or concerning symptoms.

#### FR-GUIDE-02 — No Diagnosis (P0)

The application shall not diagnose a disease, prescribe treatment, guarantee results, or claim that a product caused a skin metric to change.

#### FR-GUIDE-03 — LLM Integration (P2)

If an LLM is added, it must:
- Receive only the minimum required user data.
- Use structured, validated inputs.
- Treat the model's output as untrusted content.
- Avoid generating diagnoses, prescriptions, or unsupported causal claims.
- Fall back to deterministic guidance if the model fails.
- Never fabricate metrics or claim that it has called YouCam when it has not.

### 6.6 Dashboard and Navigation

#### FR-UI-01 — Dashboard (P0)

The dashboard shall summarize the user's latest scan, a route to scan history, routine records, and the primary action to perform a new scan.

#### FR-UI-02 — Navigation (P0)

The application shall provide consistent navigation to:
- Dashboard.
- New skin scan.
- Scan history / comparisons.
- Routine journal.
- Profile / settings.

#### FR-UI-03 — Empty, Loading, and Error States (P0)

All data-driven pages shall provide clear loading, empty, success, and error states as applicable.

#### FR-UI-04 — Responsive Design (P0)

The application shall be usable on mobile and desktop screen sizes without essential content being clipped or requiring unnecessary horizontal scrolling.

#### FR-UI-05 — Accessible Controls (P0)

Controls shall have accessible names, visible focus indicators, sufficient contrast, and keyboard support. Form errors shall be communicated in text, not color alone.

---

## 7. Non-Functional Requirements

### 7.1 Security

- All production traffic shall use HTTPS.
- Secrets shall be stored in server-side environment variables or the deployment platform's secret manager.
- Protected backend routes shall verify Firebase ID tokens.
- Firestore rules shall deny unauthorized access by default.
- Users shall only read or change their own records, except for explicitly public data.
- Admin SDK access shall be limited to the backend runtime.
- Request bodies, file sizes, and accepted fields shall be validated.
- Error messages shown to users shall not expose credentials, internal stack traces, or sensitive provider responses.
- Dependencies shall be kept reasonably current and reviewed before deployment.

### 7.2 Privacy and Data Minimization

- Collect only the data needed for the stated product features.
- Explain the purpose of image processing before upload.
- Avoid storing original images unless the feature requires it and the user has consented.
- If images are temporarily stored, define an expiration and deletion mechanism.
- Avoid logging email addresses, raw images, full ID tokens, API keys, or detailed skin data.
- Provide a way to sign out and a documented route to account/data deletion.
- Document what is stored in Firebase and what is transmitted to YouCam.
- Review applicable privacy requirements and the current terms of each provider before public release.

### 7.3 Reliability

- Network failures shall not crash the application.
- API timeouts and failed analysis tasks shall result in a recoverable error state.
- A scan should be saved only after a valid successful result is available.
- Repeated submissions shall be handled to reduce accidental duplicate records.
- Firestore write failures shall be surfaced clearly and must not be presented as successful persistence.

### 7.4 Performance

- Load dashboard sections efficiently and avoid downloading all historical scans at once.
- Use bounded Firestore queries and pagination where appropriate.
- Resize or compress client images only if compatible with YouCam requirements and without introducing unacceptable analysis degradation.
- Avoid blocking the interface while a scan is processing.
- Use lightweight charts and animations.

No absolute performance guarantee is implied; test under realistic network conditions.

### 7.5 Maintainability

- Separate UI components, page-level components, Firebase initialization, data access, and YouCam integration.
- Keep provider API details in a dedicated backend service module.
- Use shared validation and normalization utilities.
- Prefer clear, small functions and consistent naming.
- Document environment variables and local setup.
- Keep demo fixtures separate from real API data.

### 7.6 Accessibility and Usability

- Use semantic HTML elements.
- Support keyboard navigation.
- Provide text labels for metrics and chart summaries.
- Ensure forms identify invalid fields and explain how to fix them.
- Respect reduced-motion preferences where animations are used.
- Keep important actions clear and consistent.

---

## 8. Firestore Data Model

The following is the proposed MVP structure. Field names may be refined during implementation, but access rules and ownership boundaries must remain.

### 8.1 Collections

```text
users/{uid}
  scans/{scanId}
  routines/{routineId}
  reactions/{reactionId}
```

### 8.2 User Profile Document

Path: `users/{uid}`

Suggested fields:

```js
{
  displayName: "Example User",
  email: "user@example.com",
  photoURL: null,
  skinGoals: ["acne", "texture"],
  createdAt: "Firestore Timestamp",
  updatedAt: "Firestore Timestamp",
  schemaVersion: 1
}
```

Requirements:
- The document ID must equal the authenticated Firebase UID.
- Do not use email as the primary identity key.
- Only allow supported profile fields to be changed by the client.
- Treat authentication-provider profile information as a source for initial defaults, not as an excuse to overwrite user preferences on every login.

### 8.3 Scan Document

Path: `users/{uid}/scans/{scanId}`

Suggested fields:

```js
{
  createdAt: "Firestore Timestamp",
  status: "completed",
  provider: "youcam",
  endpointVersion: "configured API version",
  apiTaskId: "provider task identifier, if needed",
  metrics: {
    acne: 42,
    redness: 28,
    oiliness: 61
  },
  metricSchemaVersion: 1
}
```

The numerical values above are fictional examples only. Implement only metrics actually returned by the chosen YouCam endpoint, and preserve the provider's metric definitions and score direction.

Requirements:
- Do not assume every scan returns every possible metric.
- Avoid storing raw images in scan documents.
- Do not expose provider credentials or sensitive raw responses.
- Use server-side timestamps for completed records.
- If the provider requires a task record before completion, represent pending and failed states safely and do not display them as completed analyses.
- Consider a retention policy for provider task IDs and diagnostic metadata.

### 8.4 Routine Document

Path: `users/{uid}/routines/{routineId}`

Suggested fields:

```js
{
  productName: "Gentle Cleanser",
  brand: null,
  category: "cleanser",
  frequency: "daily",
  startDate: null,
  notes: "",
  createdAt: "Firestore Timestamp",
  updatedAt: "Firestore Timestamp"
}
```

Requirements:
- Validate required fields and allowed category values.
- Keep notes length bounded.
- Do not store medical claims as verified facts.

### 8.5 Reaction Document

Path: `users/{uid}/reactions/{reactionId}`

Suggested fields:

```js
{
  observedAt: "Firestore Timestamp",
  routineId: null,
  observation: "dryness",
  notes: "",
  createdAt: "Firestore Timestamp",
  updatedAt: "Firestore Timestamp"
}
```

Requirements:
- Reactions are self-reported.
- The related routine ID is optional.
- If a referenced routine is deleted, the reaction record must remain understandable without that reference.
- Use a predefined list of observations plus an optional bounded note field.

### 8.6 Timestamp and Metric Rules

- Use Firestore `Timestamp` values rather than formatted date strings for stored dates.
- Format timestamps for display in the user's local timezone.
- Keep metric values in a consistent normalized format.
- Record the metric schema/version when API interpretation may change.
- Do not compare metrics from incompatible scales or endpoint versions without an explicit compatibility rule.
- Treat missing values as missing, not as zero.

---

## 9. API and Backend Requirements

Exact YouCam routes, parameters, and result shapes must be taken from the current official documentation for the selected API and account. Do not guess endpoint paths or request formats.

### 9.1 Backend Environment Variables

Suggested `.env.example`:

```dotenv
PORT=5000
NODE_ENV=development

# YouCam — use the exact credential name and values required by the account
YOUCAM_API_KEY=
YOUCAM_BASE_URL=

# Firebase Admin credentials / workload identity configuration
# Prefer Application Default Credentials or deployment-managed credentials.
GOOGLE_APPLICATION_CREDENTIALS=
```

Notes:
- The actual YouCam base URL and authentication header must match the official API documentation.
- Do not commit `.env`.
- Do not put server-only secrets in `frontend/.env` or `VITE_*` variables.
- For deployed environments, prefer managed secret storage or workload identity where supported.
- If a service account file is used locally, keep it outside source control.

### 9.2 Suggested API Routes

These are application routes to implement, not YouCam provider URLs.

| Method | Route | Purpose |
|---|---|---|
| `GET` | `/api/health` | Health check; must not reveal secrets |
| `POST` | `/api/scans` | Validate request and start skin analysis |
| `GET` | `/api/scans/:scanId/status` | Retrieve processing status, if needed |
| `GET` | `/api/scans/:scanId` | Retrieve an authorized scan |
| `DELETE` | `/api/scans/:scanId` | Delete a user's scan record if deletion is supported |
| `GET` | `/api/profile` | Retrieve authenticated profile |
| `PATCH` | `/api/profile` | Update allowed profile fields |
| `POST` | `/api/account/delete` | Initiate verified account/data deletion |

Routine CRUD may use protected Express routes or direct Firestore client access governed by restrictive Firestore rules. Pick one consistent pattern and document it.

### 9.3 Authentication of Backend Requests

- The frontend sends a Firebase ID token in the `Authorization: Bearer <token>` header.
- Express verifies the token using Firebase Admin SDK.
- The backend obtains the UID from the verified token.
- The backend ignores any client-supplied UID for authorization decisions.
- Protected routes reject missing, invalid, expired, or revoked tokens as appropriate.
- Return a suitable `401` for unauthenticated requests and `403` for authenticated but unauthorized actions.

### 9.4 Error Handling

Use a consistent error response shape, for example:

```js
{
  error: {
    code: "SCAN_ANALYSIS_FAILED",
    message: "We couldn't analyze this image. Please try again."
  }
}
```

Requirements:
- Do not return API keys or internal stack traces.
- Log only the minimum operational information required to troubleshoot.
- Map provider failures to safe, understandable client messages.
- Distinguish invalid uploads, unauthorized requests, provider failures, timeouts, and persistence failures.

### 9.5 Idempotency and Duplicate Protection

- Prevent accidental multiple submissions caused by repeated clicks.
- Disable the submit action while a request is being submitted unless a deliberate retry is available.
- Where supported, use an idempotency key or internal request identifier.
- Ensure retry behavior does not create multiple completed scan documents for one intended scan.

---

## 10. Firebase Security Rules and Access Control

Firestore must not be left in open test mode for production or public demos.

### 10.1 Minimum Authorization Policy

- Unauthenticated users cannot read or write private user data.
- Authenticated users can access only documents under their own `users/{uid}` path.
- Users cannot change another user's profile, scan, routine, or reaction records.
- Clients cannot write protected scan metrics if scan results are intended to be trusted only from the backend.
- Server-side Admin SDK operations must perform their own authorization and validation because Admin SDK requests bypass Firestore Security Rules.
- Only supported profile fields may be updated by the client.
- Validate field types, field counts, allowed values, and size limits where practical.

### 10.2 Implementation Note

Write and test Firestore Security Rules before deployment. If the backend owns scan creation, make scan documents read-only to the client or route all scan reads through the backend. Do not rely on frontend route guards as a security boundary.

Use the Firebase Local Emulator Suite when feasible to test rules and authentication-related flows.

---

## 11. UI/UX Requirements

### 11.1 Design Direction

The interface should feel calm, trustworthy, and premium without looking clinical or making exaggerated beauty promises.

Recommended visual direction:
- Soft neutral background with restrained sage or muted green accents.
- Near-black text with readable contrast.
- Rounded cards and subtle borders.
- Consistent spacing and a clear typographic hierarchy.
- Lightweight transitions and animations.
- Mobile-first layouts with no unnecessary horizontal overflow.
- Use Helvetica/Neue Helvetica-like typography where available, with a suitable sans-serif fallback.
- Use `lucide-react` for icons.
- Use Tailwind CSS v4 conventions and CSS-first theme configuration.

### 11.2 Required Screens

#### Screen 1 — Landing / Sign-In
- Product name and concise value proposition.
- Short explanation of the skin tracking workflow.
- Google sign-in control.
- Privacy and informational-use notice.

#### Screen 2 — Onboarding / Profile Setup
- Welcome message.
- Optional skin goals.
- Image-processing consent information.
- Clear option to continue to the dashboard.

#### Screen 3 — Dashboard
- Greeting and latest scan summary.
- Primary “Analyze my skin” action.
- Recent scan history.
- Routine journal shortcut.
- Progress timeline preview.
- Helpful empty state for new users.

#### Screen 4 — New Scan
- Image selection or capture supported by the browser.
- Preview, replace, and cancel actions.
- File validation feedback.
- Consent notice before upload.
- Analysis button.
- Progress indicator and retry path.

#### Screen 5 — Scan Results
- Scan date.
- Relevant skin metric cards.
- Concise explanations.
- Appropriate metric visualization.
- Informational-use disclaimer.
- Save state and route to scan history.

#### Screen 6 — Scan History
- Chronological scan list.
- Date and selected summary metrics.
- Empty, loading, and error states.
- Navigation to a scan detail or comparison.

#### Screen 7 — Compare Scans
- Two scan selectors.
- Side-by-side or clearly grouped metric values.
- Change indicators only where valid.
- Date labels and limitations statement.
- No unsupported claims about cause or medical significance.

#### Screen 8 — Skin Change Timeline
- Scan and journal events in chronological order.
- Filters if implemented.
- Clear visual distinction between AI analysis and user-reported events.
- Text summary of notable recorded changes.

#### Screen 9 — Routine Journal
- Routine list and add-entry form.
- Edit and delete controls.
- Product name, category, frequency, and optional notes.
- Reaction logging.
- Validation and confirmation states.

#### Screen 10 — Profile / Settings
- Google profile details.
- Skin goal preferences.
- Privacy information.
- Sign-out control.
- Account deletion or documented deletion instructions.

### 11.3 UX States

Each asynchronous or data-dependent flow shall define:
- Initial state.
- Loading state.
- Success state.
- Empty state.
- Validation error state.
- Network or server error state.
- Retry or recovery path where appropriate.

---

## 12. Privacy, Consent, and Responsible Messaging

SkinProof handles personal profile information, facial images during analysis, and potentially sensitive self-reported skin observations. The implementation must minimize collection and communicate processing clearly.

### 12.1 Required Notices

Before the first scan, explain:
- What image or information is submitted.
- That YouCam processes the image to return skin analysis results.
- What information SkinProof saves to Firebase.
- Whether the original image is retained by SkinProof.
- How the user can delete their stored records.
- That the result is informational and not a medical diagnosis.

### 12.2 Retention

- Default to not retaining original images after the processing flow unless retention is required and explicitly enabled.
- Do not store images in Firestore documents.
- If an image is temporarily stored for provider upload, remove the temporary copy after processing or failure.
- If persistent image storage is added, document the purpose, retention period, consent flow, access policy, and deletion behavior.
- Do not make claims about deletion from YouCam systems without verifying the provider's current policy and technical behavior.

### 12.3 Responsible Guidance

- Do not label a user as having a disease based on AI metrics.
- Do not infer diagnoses from a user's routine history.
- Do not describe a metric change as proof that a product works.
- Use language such as “the recorded metric changed” rather than “your product fixed your acne.”
- Encourage professional care for persistent, severe, painful, or concerning symptoms.
- Do not tell users to stop prescribed treatment or substitute the app for professional medical advice.

---

## 13. Testing Requirements

### 13.1 Unit Tests

Test:
- Metric normalization and missing values.
- Comparison calculations.
- Input validation.
- Date formatting.
- Routine form validation.
- Safe mapping of provider errors to user-facing messages.

### 13.2 Integration Tests

Test:
- Firebase authentication state handling.
- Backend ID-token verification.
- YouCam service success and failure paths.
- Scan persistence and retrieval.
- Routine CRUD.
- Ownership enforcement.
- Duplicate submission handling.

Use mocked provider responses for automated tests, but keep at least one verified end-to-end path against the real YouCam API for the hackathon demo.

### 13.3 Manual Acceptance Tests

| ID | Test | Expected result |
|---|---|---|
| TC-01 | Sign in with Google | User is authenticated and profile is initialized |
| TC-02 | Cancel Google sign-in | User sees a safe cancellation/error state |
| TC-03 | Upload unsupported file | File is rejected with an explanation |
| TC-04 | Submit valid image | Real YouCam analysis starts |
| TC-05 | Wait for result | Processing state changes to results or a clear error |
| TC-06 | Open saved scan | Correct saved metrics and date appear |
| TC-07 | Compare two scans | Only compatible available metrics are compared |
| TC-08 | Add routine entry | Entry is saved and displayed |
| TC-09 | Edit and delete routine | Changes persist and deletion is confirmed |
| TC-10 | Record a reaction | Entry appears in the timeline |
| TC-11 | Sign out | Private content is no longer accessible |
| TC-12 | Attempt cross-user access | Access is denied |
| TC-13 | Simulate provider outage | App shows a recoverable error |
| TC-14 | Reload after saving | Persisted profile and history remain available |
| TC-15 | Use a narrow viewport | Core flows remain usable without clipping |
| TC-16 | Inspect frontend build | No YouCam secret is exposed |

---

## 14. Development Milestones

### Milestone 1 — Project Foundation (P0)
- Create separate frontend and backend projects.
- Configure Vite, React, JavaScript, and Tailwind CSS v4.
- Configure Firebase project and Google sign-in.
- Establish environment variable handling.
- Build the shared layout and route structure.

**Exit criteria:** A user can sign in and reach the dashboard.

### Milestone 2 — YouCam Integration (P0)
- Confirm the selected Skin Analysis API and its current request/response schema.
- Implement image validation and consent.
- Implement backend request and asynchronous status handling.
- Display real returned metrics.
- Add safe error states.

**Exit criteria:** A real image can complete a YouCam analysis and show the returned result.

### Milestone 3 — Firestore Persistence (P0)
- Create profile documents.
- Save normalized scan records.
- Build scan history and detail views.
- Add Firestore rules and backend ownership checks.

**Exit criteria:** Users can revisit their saved scans securely.

### Milestone 4 — Comparison and Timeline (P0)
- Implement two-scan selection.
- Normalize and compare compatible metrics.
- Display a chronological skin and journal timeline.
- Explain limitations of comparisons.

**Exit criteria:** The user can demonstrate how recorded results differ across two scans.

### Milestone 5 — Routine Journal (P0)
- Add, view, edit, and delete routine entries.
- Add reaction records.
- Display journal events alongside scans.

**Exit criteria:** Routine and reaction records persist and appear in the timeline.

### Milestone 6 — Quality and Submission (P0)
- Test core flows on desktop and mobile.
- Test unauthorized access and API failure cases.
- Finalize privacy messaging.
- Write setup instructions and `.env.example`.
- Capture required screenshots.
- Record a public 1–3-minute demo video.
- Ensure the repository contains all required source code and assets.

**Exit criteria:** A judge can follow the README, run the project, and complete the primary demo flow.

### Milestone 7 — Stretch Enhancements (P2)
- Add a carefully constrained LLM summary.
- Add reminders or routine consistency summaries.
- Add export or optional image-retention workflows only if time allows.

---

## 15. Repository Structure

Suggested monorepo layout:

```text
skinproof/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── components/
│   │   ├── features/
│   │   │   ├── auth/
│   │   │   ├── profile/
│   │   │   ├── scans/
│   │   │   ├── timeline/
│   │   │   └── routines/
│   │   ├── hooks/
│   │   ├── lib/
│   │   │   └── firebase.js
│   │   ├── pages/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── .env.example
│   └── package.json
├── backend/
│   ├── src/
│   │   ├── config/
│   │   ├── middleware/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── services/
│   │   │   └── youcamService.js
│   │   ├── validators/
│   │   ├── utils/
│   │   └── server.js
│   ├── .env.example
│   └── package.json
├── docs/
│   ├── ARCHITECTURE.md
│   ├── PRIVACY.md
│   └── API.md
├── .gitignore
├── REQUIREMENT.md
└── README.md
```

This structure is a guide, not a requirement to create empty files or folders prematurely. Keep modules proportional to the implemented features.

---

## 16. Environment and Configuration Requirements

### 16.1 Frontend

Use Vite environment variables only for public Firebase web-app configuration and the public backend base URL.

Example `frontend/.env.example`:

```dotenv
VITE_FIREBASE_API_KEY=
VITE_FIREBASE_AUTH_DOMAIN=
VITE_FIREBASE_PROJECT_ID=
VITE_FIREBASE_STORAGE_BUCKET=
VITE_FIREBASE_MESSAGING_SENDER_ID=
VITE_FIREBASE_APP_ID=
VITE_API_BASE_URL=http://localhost:5000
```

Firebase web configuration is not a substitute for security rules. Protect data through Firebase Authentication, restrictive Firestore Rules, and backend authorization.

### 16.2 Backend

Example `backend/.env.example`:

```dotenv
PORT=5000
NODE_ENV=development
FRONTEND_ORIGIN=http://localhost:5173

YOUCAM_API_KEY=
YOUCAM_BASE_URL=
```

Configure Firebase Admin using a supported server-side credential strategy, such as Application Default Credentials or a deployment-managed service identity. If local service-account credentials are necessary, keep them outside the repository and document their setup.

### 16.3 Required Configuration Checks

- Google sign-in is enabled in Firebase Authentication.
- Local development and production domains are correctly configured.
- Firestore is created in an appropriate region.
- Firestore rules are deployed and tested.
- YouCam credentials and API units are available.
- Backend CORS allows only the intended frontend origins.
- `.env` and credential files are excluded from Git.
- Deployment secrets are configured separately for frontend and backend.

---

## 17. Hackathon Submission Requirements

The final submission shall include:

1. A public repository with an appropriate license, or a private repository shared with the event organizers as specified by the hackathon.
2. A README with project overview, features, stack, setup, environment variables, and run instructions.
3. A clear explanation of which YouCam API is used and how it contributes to the product.
4. Screenshots of the main workflow.
5. A publicly accessible 1–3-minute demo video on an accepted platform.
6. A functional deployed demo where feasible.
7. Clear identification of any mock data used in secondary screens.
8. A real end-to-end YouCam analysis demonstrated in the video.
9. A description of consumer value, target audience, and the Skin Change Timeline differentiator.
10. No third-party trademarks or copyrighted media in the demo video unless permitted.

### Suggested Demo Sequence

- Introduce the problem in one sentence.
- Sign in or show the authenticated experience.
- Submit a real image and demonstrate the YouCam analysis.
- Show the returned metrics.
- Open scan history and compare two scans.
- Add a routine or reaction entry and show it on the timeline.
- Explain the technical integration, privacy approach, and consumer value.

Do not spend most of the video on slides. Prioritize a working product flow.

---

## 18. Assumptions and Open Decisions

The following items must be verified or decided during implementation:

1. The exact YouCam Skin Analysis endpoint, request parameters, supported image formats, task lifecycle, returned metrics, and API unit cost.
2. Whether the chosen YouCam API requires an image URL, multipart upload, or another input method.
3. Whether scan results are stored only in Firestore or read through protected backend endpoints.
4. Whether the app retains any original images after analysis. The MVP default is not to retain them.
5. Which skin metrics the selected API actually returns and how each metric should be interpreted.
6. The Firestore region and retention policy.
7. Whether account deletion is implemented in-app for the MVP or initially documented as a supported request workflow.
8. Whether an LLM is included. It is optional and must not block core functionality.
9. Whether the deployed demo uses a live YouCam API credential and has sufficient API units.
10. Which Firebase and backend hosting plans, quotas, and billing settings apply.

Do not fabricate API capabilities or assume that a metric exists simply because it is common in skin analysis products. Use the current official YouCam documentation for implementation decisions.

---

## 19. Definition of Done

A feature is complete only when:

- Its main user flow works.
- Inputs are validated.
- Loading, empty, success, and error states are implemented where applicable.
- Data persists when persistence is required.
- Authorization and ownership checks are in place.
- The UI works on mobile and desktop.
- Sensitive data and secrets are handled safely.
- Acceptance tests pass.
- The README or relevant documentation is updated.

The MVP is complete when the core end-to-end experience works:

**Google sign-in → consent → real YouCam skin analysis → saved metrics → scan history → two-scan comparison → routine/reaction journal → Skin Change Timeline.**

This flow is the primary delivery target. Optional features must not delay it.

---

## 20. Official Documentation

Consult the current documentation before implementing provider-specific details:

- Firebase Google sign-in: https://firebase.google.com/docs/auth/web/google-signin
- Firebase Authentication: https://firebase.google.com/docs/auth
- Cloud Firestore: https://firebase.google.com/docs/firestore
- Firestore Security Rules: https://firebase.google.com/docs/firestore/security/get-started
- Firebase Admin SDK: https://firebase.google.com/docs/admin/setup
- Firebase Local Emulator Suite: https://firebase.google.com/docs/emulator-suite
- YouCam AI API portal: https://yce.perfectcorp.com/ai-api

---

**End of document**
