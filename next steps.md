# TARA — Multilingual Journey, My Plans, Navigation Drawer, and Promotional Access

## Objective

Work in the existing MGD TARA repository and current branch.

Implement the following improvements while preserving the existing chatbot, SIU integration, authentication, application workflows, MongoDB persistence, and UI design system.

This is an extension of the existing application, not a redesign or rewrite.

Before making changes, inspect the current branch, working tree, recent commits, relevant frontend components, backend APIs, database schemas, plan-entitlement logic, and existing tests.

Preserve all existing user changes. Do not reset, discard, or overwrite unrelated work.

---

# 1. Remove the Budget-Type Step from the Admission Journey

We previously introduced a question asking users whether their budget was a yearly tuition budget or a total budget.

We no longer need this question.

Requirements:

1. Remove the entire budget-type question from the conversational admission journey.
2. After collecting the user's budget and currency, proceed directly to the existing subject-selection step.
3. Remove the associated suggested-answer buttons and unnecessary intermediate journey state.
4. Update journey sequencing, validation, and frontend progress indicators accordingly.
5. Preserve the user's actual budget and currency.
6. Do not unnecessarily delete previously saved budget-type data from existing user records. Handle legacy records safely.
7. Ensure users with existing conversations or saved preferences can continue without getting stuck on the removed step.

Do not modify other admission questions or change the university filtering logic unnecessarily.

---

# 2. Implement the Same Admission Journey in Multiple Languages

## Current Problem

TARA's admission journey works properly in English, but when users communicate in Urdu, Hindi, Roman English, Arabic, or other languages, the journey's interactive options and workflows do not work consistently.

For example, the chatbot may understand a translated message but fail to display the correct buttons, suggested answers, or next journey step.

I want the same complete admission journey to work across languages.

## Required Languages

Initially implement and verify:

1. English
2. Roman English
3. Urdu
4. Hindi
5. Arabic

Keep the architecture extensible for additional languages.

## Expected Behaviour

The same workflow must be available in every supported language.

For example, the English journey might ask:

"What level of study are you interested in?"

With options:

- Bachelor's
- Master's
- PhD

When the user selects Urdu, Hindi, Roman English, or Arabic, TARA should display the equivalent translated question and the same three selectable options in that language.

Apply the same behavior throughout the complete journey, including:

- Study level.
- Intake.
- Budget and currency.
- Subject selection.
- University discovery.
- University shortlisting.
- Programme selection.
- Document handling.
- Application review.
- Application creation.
- Application submission confirmation.
- Follow-up questions and suggested answers.
- Success, cancellation, and error messages.

## Important Implementation Rules

1. Do not create separate admission workflows for each language.
2. Maintain one shared journey engine and one consistent set of internal journey states.
3. Translate questions, buttons, suggested options, and user-facing responses according to the selected language.
4. Map translated button selections to the same internal action identifiers used by English.
5. Accept both translated selections and natural-language answers.
6. Understand Roman Urdu and Roman Hindi written using Latin characters.
7. Handle mixed-language messages, such as Urdu or Hindi combined with English university names and programme titles.
8. Preserve exact university and programme names from the SIU catalogue rather than translating official names.
9. Preserve numbers, currency values, dates, and selected preferences when switching languages.
10. Support Arabic right-to-left layout where appropriate without breaking the existing interface.
11. Do not allow translation to bypass authentication, authorization, application validation, or explicit submission confirmation.
12. If a language is not confidently recognized, ask the user to select a language rather than silently starting an incorrect workflow.

## Language Selection and Persistence

Inspect the existing language-selection functionality first.

- Reuse the existing language selector if available.
- Preserve the user's selected language across messages, screens, and sessions.
- Allow users to change languages without losing their admission progress.
- Ensure a language change does not reset the current journey or create duplicate conversations.

## Testing

Test the same complete journey in English, Roman English, Urdu, Hindi, and Arabic.

Verify that all translated buttons trigger the correct internal actions and that the application workflow behaves identically across languages.

Do not treat translation of chatbot text alone as completion. The interactive journey must work.

---

# 3. Implement the My Plans Page Using the Supplied Design

**Design reference:**

[My Plan — Journey to Opportunity](https://github.com/bilalaamir666/move/blob/f2733bf0e495d53b0d10f800f1692c973727a2c0/app/My%20Plan_%20Journey%20to%20Opportunity.png)

Inspect the actual image at the specified commit and use it as the visual source of truth.

The existing My Plans page is already mostly implemented. Preserve its existing structure and functionality.

## Promotional Code Section

Only improve the promotional information at the bottom of the page.

Replace the existing promotional explanation with exactly these two lines:

**Use this promo code to get access to five additional universities.**

**It also gives you discounts on Standard and Premium plans.**

Requirements:

- Display these two lines clearly and attractively.
- Do not add additional promotional paragraphs or unnecessary headings.
- Do not invent specific discount percentages or amounts.
- Keep the existing promo-code input and application functionality.
- I will provide the actual Standard and Premium discount values later, so leave the discount amounts unspecified for now.

Do not change the other My Plans sections unless necessary for this promotional update.

---

# 4. Redesign the Left Navigation Drawer

**Design reference:**

[TARA AI Guide — Navigation Drawer](https://github.com/bilalaamir666/move/blob/f2733bf0e495d53b0d10f800f1692c973727a2c0/app/TARA%20AI%20Guide%20Navigation%20Drawer.png)

Inspect the reference image and existing drawer implementation.

The drawer opens when the user clicks the three-line menu button on the top-left of the home screen.

Keep the existing visual design as the starting point, but apply the following changes.

## A. Remove the Profile Identity Row

Remove the entire top identity section containing:

- Profile picture.
- Muhammad Bilal Aamir.
- Student designation.
- SIU label.

Do not display this identity row in the drawer.

The user's profile information remains accessible through the existing Profile option in the footer.

## B. Keep the Active Plan Section

Keep the existing active-plan information and upgrade-plan functionality.

Users should be able to see their current plan and access the upgrade option.

Connect this section to the actual plan and entitlement information rather than hardcoded values.

## C. My Plan

Keep the My Plan navigation entry.

However, replace its current Home icon with an appropriate plan-related icon.

Use an icon such as a clipboard-list, notebook, or suitable planning icon.

Do not use a Home icon for My Plan.

## D. My Applications

Keep the My Applications entry and connect it to a dedicated applications overview screen.

A student may apply to multiple universities depending on their plan or promotional entitlement.

The My Applications page must therefore support multiple application records.

Display each application in an individual, clean card containing relevant information such as:

- University name.
- Programme name.
- Application ID, where available.
- Application status.
- Submission date, where available.
- Relevant action to view application details.

Support any number of applications permitted by the user's actual plan and entitlement.

For example, users may have three, five, or ten applications depending on their access.

Important:

- Retrieve actual applications belonging to the authenticated user.
- Do not show fabricated applications.
- Do not assume that a shortlist is an application.
- Display the actual application status returned by SIU.
- Keep applications separate from shortlisted universities.
- Enforce user ownership on the backend.
- Do not change SIU submission behavior or create duplicate applications.

If the existing My Applications page is missing, implement it using the current TARA design system and existing application APIs.

## E. Remove Promo Code from the Drawer

Remove the separate Promo Code navigation entry.

Promo-code information and redemption should remain accessible through My Plans.

## F. Remove Settings from the Drawer

Remove Settings from the drawer because Profile already contains the settings functionality.

Do not duplicate the same settings entry.

## G. Keep the Following Entries

Keep:

- About.
- Help & Support.
- Logout.

Ensure each entry retains its existing functionality.

## H. Footer

Keep the existing footer and Profile access.

The drawer should look clean, balanced, and uncluttered after removing the identity row, promo-code entry, and Settings.

---

# 5. Promotional Access Popup During University Discovery

## Objective

I want students to discover the available plans and promotional benefits naturally when they start exploring universities.

My proposed location is immediately after the user clicks **See Universities** and enters the University Admission screen.

Please inspect the existing navigation and user journey and implement a lightweight promotional popup at this point.

## Popup Content

Use concise, friendly wording.

Suggested content:

**Have a promo code?**

Unlock access to five additional universities by applying your promo code.

You can also get discounts on Standard and Premium plans.

## Popup Actions

Provide two clear actions:

1. **My Plans** — takes the student to the My Plans page, where they can view plans and enter a promo code.
2. **Maybe Later** — closes the popup and allows the student to continue exploring universities.

If appropriate, include a direct **Enter Promo Code** action that takes the student to the promotional-code section of My Plans.

Do not make the popup intrusive or prevent users from continuing their university journey.

## Popup Behavior

- Show the popup at the appropriate point after entering university discovery.
- Avoid showing it repeatedly every time the user returns to the same page.
- Respect dismissal and session state.
- Do not reset the university search, filters, selected subject, budget, shortlist, or chatbot conversation.
- Keep the popup responsive and visually consistent with TARA's existing design.

## Important: Verify the Actual Entitlement Rules

Currently, the default student access allows applying to one university.

The promotional and paid plans provide additional access, but the exact entitlement behavior must be inspected before implementing the popup.

In particular, distinguish between:

- Access to explore additional universities.
- Ability to shortlist additional universities.
- Ability to create additional applications.
- Ability to submit applications to multiple universities.

These are not necessarily the same permission.

Do not assume that a promo code providing access to five additional universities automatically provides five additional application submissions.

Inspect the existing plan limits, promo-code validation, backend enforcement, and application-creation checks.

The popup must communicate only benefits that are actually supported by the existing entitlement logic.

If the current implementation does not clearly define these limits, report the ambiguity rather than inventing a rule.

---

# 6. Navigation and Backward Functionality

The promotional flow must not interrupt the user's existing journey.

Example:

1. User starts university admission.
2. User provides their preferences.
3. User clicks See Universities.
4. The promotional popup appears.
5. User opens My Plans.
6. User views the plans or applies a promo code.
7. User returns to university discovery.
8. Their previous search, selected filters, university results, shortlist, and chatbot conversation remain intact.

Implement proper navigation and state preservation.

The same requirement applies when opening My Applications or returning from its details page.

Do not send users back to the beginning of the admission journey when navigating between these screens.

Use the existing routing and state-management architecture.

---

# 7. Backend, Database, and Security Requirements

Inspect existing implementation before changing schemas or endpoints.

- Reuse existing MongoDB collections and services.
- Reuse existing plan, promo-code, application, and user-preference APIs.
- Add only missing functionality.
- Enforce authenticated user ownership.
- Keep plan limits and promo-code validation on the backend.
- Never trust frontend-supplied plan limits or user IDs.
- Ensure users cannot exceed their actual entitlements by modifying frontend requests.
- Preserve existing AES-256-GCM encryption where applicable.
- Do not expose credentials, tokens, or personal information.
- Avoid destructive migrations and unnecessary database changes.

Do not weaken authentication, SIU channel restrictions, or explicit application-submission confirmation.

---

# 8. Testing and Verification

Run focused tests first and the relevant complete test suites after implementation.

### Multilingual Journey

- English.
- Roman English.
- Urdu.
- Hindi.
- Arabic.
- Translated buttons and suggested options.
- Language switching during an active journey.
- Conversation and preference preservation.
- Application review and submission confirmation.

### My Plans

- Correct design and layout.
- Promo-code text.
- Existing promo-code validation and redemption.
- No invented discount values.

### Navigation Drawer

- Identity row removed.
- Active plan and upgrade action retained.
- Correct My Plan icon.
- My Applications navigation.
- Promo Code and Settings removed.
- About, Help & Support, Logout, and Profile still functional.

### My Applications

- Multiple applications displayed.
- Correct application details and statuses.
- User ownership.
- Empty state.
- Loading and error handling.
- Application details navigation.

### Promotional Popup

- Opens at the correct point.
- Correct content and actions.
- Dismissal works.
- Does not repeatedly interrupt the user.
- My Plans navigation works.
- Returning preserves the university discovery state.

### Regression

- Existing admission journey.
- University catalogue and SIU integration.
- Shortlisting.
- Application creation.
- Document handling.
- Confirm & Submit.
- Conversation persistence.
- Frontend production build and TypeScript checks.

Do not submit real applications during testing.

Clearly distinguish mocked tests from real SIU or MongoDB integration tests.

---

# 9. Git and Safety Rules

- Work in the existing TARA repository and current branch.
- Inspect the working tree before editing.
- Preserve all existing uncommitted changes.
- Do not reset, clean, or discard user work.
- Do not modify unrelated screens or workflows.
- Do not stop or restart the SIU backend unnecessarily.
- Do not commit.
- Do not push.
- Do not expose secrets or credentials.

---

# 10. Final Report

Report:

1. How the budget-type step was removed.
2. Which languages now support the complete interactive admission journey.
3. How translated options map to the existing journey actions.
4. My Plans promotional-section changes.
5. Navigation drawer changes.
6. My Applications implementation and how multiple applications are displayed.
7. Promotional popup placement, wording, and behavior.
8. Actual plan and promo-code entitlement rules discovered.
9. How navigation preserves the user's journey state.
10. Exact test results, including passed, failed, and skipped counts.
11. Remaining limitations or unresolved product decisions.
12. Files changed and current Git status.

## Priority Order

1. Remove the budget-type question.
2. Make the existing admission journey work consistently in English, Roman English, Urdu, Hindi, and Arabic.
3. Implement the My Plans promotional-section changes.
4. Update the navigation drawer.
5. Implement My Applications for multiple university applications.
6. Add the university-discovery promotional popup.
7. Verify entitlements, navigation preservation, and regression behavior.

**Important:** Implement these changes incrementally. Preserve the existing TARA and SIU functionality, especially application creation and Confirm & Submit. Do not replace working features with mock implementations or introduce unrelated redesigns.






# Additional Requirements — Multiple University Applications, Reusable Student Data, and Subscription/Payment Architecture

This section extends the existing TARA implementation task. Treat it as mandatory and implement it alongside the previous requirements. Inspect the current application, plan, subscription, payment, and SIU integration code before making changes.

## 1. Support Multiple University Applications Without Repeating the Journey

### Core requirement

A student must not have to complete the entire admission journey again just because they want to shortlist or apply to another university.

Separate the system into:

- **Student Profile:** Reusable personal and academic information.
- **Study Preferences / My Plan:** Reusable default study preferences.
- **University Shortlist:** Multiple universities the student is considering.
- **University Applications:** Separate application records for each university and programme.

Do not treat the student's first application as their only possible application or lock the chatbot to that university after submission.

### Example workflow

Assume a student has already completed the admission journey with:

- Study level: Bachelor's
- Subject: Computer Science
- Budget: AED 20,000
- Preferred country: UAE
- Intake: September 2027
- Three shortlisted universities
- Documents uploaded to their TARA document library

The student applies to University A and later says:

"I want to apply to another university."

TARA should respond naturally:

"Of course! Your study preferences and saved documents are already available. Which university would you like to explore next?"

The student selects University B.

TARA should then:

1. Load the student's existing profile and My Plan.
2. Reuse the existing study level, subject, budget, currency, country, and other relevant preferences.
3. Retrieve University B's actual programmes and admission requirements from the authorized SIU catalogue.
4. Ask the student to select a programme.
5. Reuse existing information and documents where appropriate.
6. Ask only for information that is genuinely missing, outdated, or different for University B.
7. Prepare a separate application for University B.
8. Keep the existing University A application unchanged.

Do not restart the entire admission journey.

### 2. Reuse Preferences, but Allow Application-Specific Changes

Implement a clear distinction between default student preferences and application-specific information.

**Reusable preferences should include, where available:**

- Study level.
- Preferred subject or programme.
- Default budget and currency.
- Preferred destination countries.
- Preferred universities.
- Academic background.
- Existing personal and educational information.
- Other relevant saved preferences.

**Information that may differ between applications:**

- University.
- Selected programme.
- Country or study destination.
- Intake.
- University-specific admission requirements.
- Application-specific answers and documents.
- Any additional information required by SIU.

Use the saved preferences as defaults, not as mandatory values that cannot be changed.

For example, if the student selects a university in another country, TARA should ask whether they want to use that country for this application or update their general preference.

Similarly, the student may select a different intake for University B without changing the default intake for all future applications.

Do not overwrite general preferences simply because a student makes a different selection for one application. Update global preferences only when the student explicitly asks to change them or confirms the proposed update.

### 3. Reuse Documents Without Confusing Application Attachments

The existing TARA document library must remain the central location for uploaded documents.

When preparing a new university application:

1. Retrieve the user's existing document metadata from their authenticated document library.
2. Compare available documents with the requirements of the selected university and programme.
3. Identify documents that are suitable for reuse.
4. Ask the student to upload only missing or outdated documents.
5. Allow the student to reuse a document without uploading the same file to TARA again.
6. Associate the selected document with the new application.
7. Follow SIU's actual document-upload and confirmation requirements for that specific application.

Important distinctions:

- A document stored in TARA is not automatically uploaded to SIU.
- A document uploaded to SIU for University A must not automatically be treated as attached to University B.
- Reuse the original TARA document where appropriate, but obtain separate SIU upload confirmation or references whenever SIU requires them.
- Preserve document ownership, encryption, category, expiry information, and access restrictions.
- Do not ask the user to upload an existing valid document again unnecessarily.

If a document has expired, is incomplete, or does not satisfy the new university's requirements, explain why a replacement is needed.

### 4. Multiple Application Records and Application Management

Inspect the existing application schema and SIU adapter before changing anything.

TARA must support multiple distinct application records belonging to the same authenticated user, subject to the user's actual subscription and SIU restrictions.

Each application should maintain its own:

- TARA application identifier.
- Actual SIU application identifier, when created.
- University.
- Programme.
- Study level.
- Country.
- Intake.
- Application-specific information.
- Required and attached documents.
- SIU document references and upload status.
- Current application status.
- Creation and update timestamps.
- Submission status and confirmation history.

Do not overwrite an existing application when a student starts another one.

Prevent accidental duplicate application creation and submission using the existing idempotency and confirmation architecture.

The My Applications screen must display the user's actual application records, not merely shortlisted universities or proposed applications.

Preserve existing user ownership and SIU identity restrictions.

### 5. Application Entitlements and Subscription Limits

Inspect the existing subscription, plan, promo-code, and entitlement logic before implementing or changing any limits.

Do not assume that a limit on university discovery is the same as a limit on application creation or submission.

Keep these capabilities conceptually separate:

- Exploring universities.
- Viewing university details and programmes.
- Shortlisting universities.
- Creating application drafts.
- Creating SIU applications.
- Submitting applications.

The actual limits must come from the approved subscription and entitlement rules, not frontend hardcoding.

If a student has reached a limit, TARA should explain which capability is restricted and show the relevant upgrade or plan information.

Do not block the student from viewing their existing applications or accessing their saved information merely because their subscription changes.

Do not silently delete, disable, or modify existing applications after a plan downgrade.

If the existing rules do not clearly define a limit, document the ambiguity rather than inventing one.

## 6. Prepare the Backend for the Future SIU Payment API

We will integrate the actual SIU payment API later. Prepare TARA's architecture so that the integration can be added without rewriting the subscription and entitlement system.

First inspect the existing payment, promo-code, subscription, and plan models. Reuse them where appropriate.

### Required architecture

Separate the following concepts:

1. **Subscription Plans:** Free, Standard, Premium, and any other officially supported plans.
2. **Subscription Records:** The user's current plan, status, start date, expiry date, renewal information, and payment references.
3. **Entitlements:** What the user is allowed to access under their current plan.
4. **Promo Codes:** Code validation, eligibility, discount rules, and applicable plan benefits.
5. **Payment Transactions:** Payment provider reference, amount, currency, status, and timestamps.

Use a payment-provider adapter or service interface so the future SIU payment integration can be connected without changing the core subscription logic.

Do not invent SIU payment endpoints, payloads, webhook formats, or authentication methods.

### Promo-code support

Prepare the backend to support server-side promo-code validation.

The future SIU payment integration is expected to handle promo-code detection and payment-related validation.

TARA should be ready to receive the verified payment or promotion result and apply the corresponding approved entitlement.

Never trust a discount amount, promo validity, or payment-success flag supplied only by the frontend or chatbot.

Do not apply discounts twice or allow the same payment event to grant entitlements repeatedly.

### What happens after a Standard or Premium purchase?

Design and test the subscription lifecycle so that after a payment is successfully verified:

1. The backend receives a verified payment result from the approved payment integration.
2. It validates the transaction and prevents duplicate processing.
3. It updates the user's subscription record.
4. It activates the corresponding Standard or Premium entitlements.
5. The frontend refreshes the user's plan and access information.
6. The drawer, My Plans page, university discovery, and application-related controls reflect the newly activated entitlements.
7. TARA can immediately use the updated entitlement information in subsequent conversations.

The user should receive a confirmation only after the backend has verified the payment and activated the plan.

Do not activate a paid plan merely because the user clicked a purchase button or returned from a payment page.

### Subscription lifecycle cases

Prepare for these cases:

- Successful payment.
- Failed payment.
- Cancelled payment.
- Pending payment.
- Duplicate payment notification.
- Expired subscription.
- Renewal.
- Upgrade from Standard to Premium.
- Downgrade from Premium to Standard.
- Refund or payment reversal, if supported by the future payment contract.
- Invalid or expired promo code.

Use the actual future payment contract when available. Until then, test the subscription lifecycle through clearly identified development fixtures or a payment-provider mock.

Keep payment mocks disabled in production.

### 7. Entitlement Enforcement

The backend must be the source of truth for plan access.

- Do not rely on hidden or disabled frontend buttons as access control.
- Validate the user's active entitlement on every protected backend operation.
- Keep application ownership and SIU authorization separate from subscription entitlement.
- Do not allow a paid plan to bypass SIU authentication or application-submission confirmation.
- Ensure that changing a plan does not grant access to another user's data.
- Make entitlement checks reusable across the frontend, chatbot, and backend APIs.

The frontend should display the user's actual active plan and relevant limits based on backend responses.

## 8. Required Testing

### Multiple Applications

Test this complete scenario:

1. A student completes the admission journey and saves their preferences.
2. The student creates an application for University A.
3. The student later asks to apply to University B.
4. TARA reuses the existing preferences and profile.
5. TARA asks only for the university-specific or genuinely missing information.
6. The student reuses suitable documents from their TARA document library.
7. A separate application record is created for University B.
8. The original University A application remains unchanged.
9. Both applications appear correctly in My Applications, using actual SIU records where available.
10. The student can start exploring another university after submitting an earlier application.

Also test:

- Changing intake for one application without changing global preferences.
- Changing destination country for one application.
- Explicitly updating general study preferences.
- Missing or expired documents.
- Duplicate application creation.
- Unauthorized access to another user's applications.
- Subscription limits on application creation and submission.

### Payment and Subscription

Test:

- Standard purchase success.
- Premium purchase success.
- Payment failure and cancellation.
- Pending payment.
- Duplicate payment notifications.
- Promo-code validation and invalid codes.
- Entitlement activation after verified payment.
- Entitlement updates in the frontend and chatbot.
- Subscription expiry and downgrade.
- Existing applications remaining accessible after a downgrade.

Use the actual SIU payment API only when its documentation, credentials, and QA environment are available. Otherwise, clearly label these as mocked payment lifecycle tests. Do not claim live payment integration has been verified.

## 9. Final Report Additions

In addition to the previous task's final report, explicitly report:

1. Whether multiple university applications are supported.
2. How student preferences are reused across applications.
3. Which fields remain global and which can be changed per application.
4. How TARA document reuse differs from SIU application-specific document uploads.
5. How duplicate applications are prevented.
6. Which application limits are enforced by the backend.
7. What subscription and entitlement functionality was implemented.
8. Whether the payment-provider adapter is ready for the future SIU payment API.
9. Which payment and subscription tests used mocks.
10. What is still blocked pending the actual SIU payment API contract.

## Important Implementation Rules

- Preserve the existing working admission journey.
- Do not force students to repeat previously completed steps.
- Do not overwrite global preferences with application-specific choices.
- Do not fabricate applications, payment results, promo codes, discounts, or entitlements.
- Do not create duplicate application or payment systems.
- Preserve MongoDB persistence, AES-256-GCM encryption, authentication, user ownership, SIU channel restrictions, idempotency, and explicit submission confirmation.
- Do not submit real applications or initiate real payments during development testing.
- Do not commit or push changes.

Implement these requirements incrementally, run focused tests, and report any product or entitlement decisions that still require confirmation.


Keep an eye on Claude Code's memory and context usage throughout the implementation. Avoid exhausting the context or letting the interface become unresponsive or grey. Preserve progress and manage memory carefully as you work.