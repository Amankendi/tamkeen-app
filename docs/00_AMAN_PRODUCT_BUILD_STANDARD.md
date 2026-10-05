# AMAN Product Build Standard

**Version:** 1.0  
**Status:** Canonical global source  
**Owner:** AMAN / ALKENDI  
**Effective date:** 2026-10-06  
**Applies to:** All software, platform, website, AI-agent, automation, internal-tool, client-delivery, and experimental product repositories managed under AMAN / ALKENDI.

## Canonical reference

This standard was established from the ChatGPT conversation with AMAN on **2026-10-06**, beginning from the reference pattern shown in the “Anthropic designer (ex. Apple)” product/build specification and then adapted into an AMAN-wide engineering and product operating standard.

This file is the repository-level source derived from that conversation.

**Precedence rule:**

1. Safety, legal, security, and data-protection requirements.
2. This `AMAN Product Build Standard`.
3. The repository’s `PROJECT_MASTER_SPEC`.
4. The current `PRODUCT_BASELINE`.
5. The current `CURRENT_SPRINT`.
6. Individual implementation notes.

A project-specific specification may extend this standard, but must not silently weaken security, testing, data integrity, authorization, accessibility, privacy, or quality requirements.

---

## 1. Product philosophy

Build products that are useful before they are impressive.

Every feature must answer at least one of these questions:

- What user problem does this solve?
- What business outcome does this improve?
- What operational burden does this remove?
- What decision does this make faster or better?
- What risk does this reduce?
- What measurable value does this create?

Do not add functionality merely because it is technically possible.

Avoid:

- fake dashboards
- fake data
- fake integrations
- fake AI capability
- placeholder actions presented as working
- visually impressive but operationally empty flows
- generic “AI-powered” claims without a real workflow
- duplicated features
- conflicting business rules
- hidden manual work presented as automation

If a capability is incomplete, label it clearly.

Use one of:

- WORKING
- PARTIAL
- EXPERIMENTAL
- BLOCKED
- COMING SOON
- NOT IMPLEMENTED

Never present unfinished work as complete.

---

## 2. Source-of-truth structure

Every significant project should converge toward this documentation structure:

```
docs/
  00_AMAN_PRODUCT_BUILD_STANDARD.md
  01_PROJECT_MASTER_SPEC.md
  02_PRODUCT_BASELINE.md
  03_BRAND_DESIGN_SYSTEM.md
  04_TECH_ARCHITECTURE.md
  05_DATABASE_AND_PERMISSIONS.md
  06_DECISIONS_LOG.md
  07_ROADMAP.md
  08_CURRENT_SPRINT.md
```

Not every file must exist on day one, but the information must not be scattered indefinitely.

### GLOBAL STANDARD

Defines how AMAN products are built.

### PROJECT MASTER SPEC

Defines what this specific product is.

### PRODUCT BASELINE

Describes what exists now and what is verified to work.

### CURRENT SPRINT

Defines what is being built now.

Agents and developers must not repeatedly reinterpret the entire product when only the current sprint changed.

---

## 3. Audit before rebuild

Before changing an existing product:

1. Inspect the current stack.
2. Inspect routes and screens.
3. Inspect components.
4. Inspect data model.
5. Inspect authentication and roles.
6. Inspect APIs and integrations.
7. Inspect environment variables by NAME only.
8. Inspect deployment configuration.
9. Identify duplicates.
10. Identify conflicting rules.
11. Identify broken functionality.
12. Identify working functionality that should be preserved.
13. Identify security or privacy risks.
14. Identify technical debt that blocks the requested goal.

Do not rebuild working systems simply because rebuilding is easier for the agent.

Default strategy:

```
PRESERVE good work
REMOVE duplication
RESOLVE contradictions
SIMPLIFY architecture
COMPLETE missing systems
TEST the result
```

---

## 4. Execution behavior for AI coding agents

When the specification makes a decision obvious, implement it.

Ask for clarification only when:

1. there are materially different business outcomes,
2. credentials or external authorization are required,
3. a destructive action needs approval,
4. legal/compliance meaning is unclear,
5. the project owner must choose between mutually exclusive product directions.

Do not repeatedly ask confirmation for routine engineering decisions.

Do not invent facts, credentials, APIs, vendors, customers, case studies, pricing, legal status, production readiness, or test results.

At the end of each implementation cycle, report:

```
DONE
TESTED
FAILED
BLOCKED
NEXT
```

Include evidence when available.

---

## 5. Architecture principles

Prefer simple, modular, maintainable systems.

Requirements:

- clear separation between UI, business logic, data access, and integrations
- typed interfaces where the stack supports them
- reusable components without premature abstraction
- explicit state ownership
- consistent naming
- reproducible builds
- migrations for schema changes
- configuration through environment variables
- provider abstractions where external vendors may change
- no critical business logic hidden only in UI components

Avoid:

- one giant component
- duplicated business rules
- circular dependencies
- hardcoded production values
- hidden global state
- vendor lock-in without business justification
- direct browser access to privileged services

---

## 6. Data model

Use a relational database when the product contains meaningful relationships, permissions, transactions, projects, users, orders, clients, or operational state.

Supabase/PostgreSQL is the preferred default when it fits the product, but it is not mandatory if the existing architecture has a better justified choice.

Database rules:

- use primary keys
- use foreign keys
- use constraints
- use unique constraints where business identity requires them
- use timestamps
- avoid duplicate business truth
- define deletion behavior intentionally
- use migrations
- keep production schema changes reproducible
- record important state transitions

Never rely on frontend filtering as a data-security mechanism.

---

## 7. Authentication, authorization, and permissions

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

Both are required.

Use role-based and, when needed, resource-based permissions.

Examples:

- SUPER_ADMIN
- ADMIN
- MANAGER
- EMPLOYEE
- CONSULTANT
- CLIENT
- PARTNER
- PROVIDER

Roles must be project-specific where necessary.

For Supabase, use RLS where appropriate.

Critical rule:

**User A must not access User B’s private data simply by changing a URL, identifier, request, or frontend state.**

Authorization must be enforced server-side and/or database-side.

---

## 8. Security and secrets

Security is part of the product definition.

Never store real secrets in:

- source code
- Git history
- screenshots
- prompts
- README examples
- browser-exposed environment variables
- frontend bundles

Repository documentation may contain variable names only, for example:

```
OPENAI_API_KEY
SUPABASE_URL
SUPABASE_PUBLISHABLE_KEY
RESEND_API_KEY
STRIPE_SECRET_KEY
```

Real values belong in protected environment/secret storage.

If a secret is accidentally exposed, rotate it. Removing it from the latest file is not enough.

Additional requirements where applicable:

- server-side validation
- input validation
- output encoding
- secure sessions
- rate limiting
- upload validation
- file access rules
- CSRF protection where relevant
- audit logs for sensitive actions
- least-privilege service credentials
- safe production error messages
- dependency review
- HTTPS in production

---

## 9. Privacy and GDPR

Products serving users in Europe must be designed with GDPR in mind.

Where applicable provide:

- privacy policy
- legal notice
- cookie policy
- consent management
- data export path
- account deletion path
- data-retention rules
- lawful handling of personal data

Do not enable non-essential tracking before consent when consent is legally required.

Store consent version and timestamp where appropriate.

Collect only data that has a defined purpose.

---

## 10. AI systems and agents

An AI feature must have a defined job.

For each agent define:

- identity
- objective
- inputs
- allowed data
- capabilities
- limitations
- tools/integrations
- outputs
- approval requirements
- logging
- cost controls
- version
- status

Recommended statuses:

- prototype
- testing
- active
- paused
- retired

Do not present agents as unlimited autonomous workers.

High-impact actions should require explicit authorization when appropriate, including:

- sending external communications
- publishing
- deleting important records
- financial transactions
- contractual actions
- changing permissions
- modifying production infrastructure
- releasing confidential data

Human approval points must be designed into the workflow, not added as an afterthought.

---

## 11. AI cost and token governance

Every paid AI call should be attributable when technically feasible.

Record:

- user
- project
- agent
- provider
- model
- timestamp
- input usage
- output usage
- estimated cost
- success/failure

Use budgets by appropriate scope:

- request
- user
- project
- agent
- day
- month

Use model routing.

Examples:

- extraction, formatting, classification -> economical model
- routine generation -> standard model
- difficult strategy, architecture, analysis -> stronger reasoning model

Do not send entire histories when only a small context window is needed.

Use:

- retrieval
- context selection
- summarization
- prompt versioning
- caching where safe
- deterministic preprocessing when AI is unnecessary

Target:

**maximum useful intelligence per euro and per token.**

---

## 12. Integrations

External systems must be treated as replaceable modules when practical.

Typical integration classes:

- email
- calendar
- CRM
- payments
- shipping
- storage
- AI providers
- analytics
- messaging
- automation
- maps/geolocation

Do not spread one vendor SDK across the entire codebase if a clean adapter can isolate it.

Never claim an integration works until a real or appropriately sandboxed end-to-end path has been tested.

---

## 13. Email architecture

Do not make a founder’s personal inbox the permanent public system identity of a business product.

Prefer role-based addresses such as:

- info@
- hello@
- support@
- projects@
- billing@
- notifications@

Use SPF, DKIM and DMARC when operating a custom domain.

Separate:

- public identity
- automated system mail
- transactional mail
- internal administration

Verify DNS before claiming configuration is complete.

---

## 14. UX and design quality

Design should communicate product logic, not hide it.

Requirements:

- clear hierarchy
- consistent spacing
- consistent typography
- coherent component library
- predictable navigation
- visible system status
- useful empty states
- useful error states
- useful loading states
- meaningful calls to action

Avoid:

- random visual effects
- excessive gradients
- unnecessary glassmorphism
- meaningless motion
- stock imagery that weakens credibility
- duplicate cards with different styling
- tiny text used to appear “premium”
- mobile layouts that are merely compressed desktop pages

Every interactive control should define relevant states:

- default
- hover
- focus
- active
- disabled
- loading
- error

---

## 15. Motion

Motion should explain:

- hierarchy
- transition
- state
- relationship
- progress
- feedback

Do not animate everything.

Respect `prefers-reduced-motion`.

Performance and usability outrank decoration.

---

## 16. Responsive behavior

Test intentionally at mobile, tablet, desktop, and wide desktop sizes.

Minimum practical targets should include approximately:

- 375px
- 768px
- 1024px
- 1440px+

Avoid horizontal overflow.

Navigation, modals, forms, tables, media, dashboards, and agent interfaces must be usable on small screens when the product claims mobile support.

---

## 17. Internationalization

If a project is multilingual, internationalization must be architectural.

Support RTL/LTR properly.

Do not hardcode language strings throughout components.

Where business content differs by market, allow independent content per language instead of assuming machine translation is authoritative.

For Arabic products, test Arabic layout independently.

---

## 18. Accessibility

Target WCAG 2.2 AA where reasonably achievable.

Requirements include:

- semantic HTML
- keyboard navigation
- visible focus states
- labels
- sufficient contrast
- alt text
- accessible form errors
- screen-reader status where needed

Do not communicate critical information using only:

- color
- hover
- animation

---

## 19. Performance

Performance is a product feature.

Optimize:

- images
- fonts
- JavaScript
- network calls
- third-party libraries
- caching
- rendering strategy

Lazy-load non-critical assets.

Avoid loading heavy decorative media before core content.

For public web products, aim for healthy Core Web Vitals where realistically possible.

---

## 20. SEO and discoverability

For public web products implement where relevant:

- unique metadata
- canonical URLs
- Open Graph
- social cards
- robots.txt
- sitemap
- structured data
- hreflang for multilingual pages

Do not copy one generic title and description across every route.

---

## 21. Analytics

Measure outcomes, not only visits.

Track events tied to the product’s goal, such as:

- signup
- lead submission
- purchase
- booking
- checkout
- project request
- agent use
- service interest
- form abandonment
- conversion by source

Analytics must respect privacy requirements.

---

## 22. Error handling

Never expose users to:

- blank screens
- raw stack traces
- database error dumps
- secret values
- provider internals
- silent failures

Async flows should account for:

- loading
- success
- empty
- error
- retry

Preserve user-entered form data after recoverable submission errors.

Log technical details safely for diagnosis.

---

## 23. Environments

Use environment separation appropriate to project maturity:

```
LOCAL
DEVELOPMENT
PREVIEW / STAGING
PRODUCTION
```

Production secrets must not be reused casually in development.

A preview deployment should be available before major production releases where the hosting setup supports it.

Do not overwrite production unintentionally.

---

## 24. Repository hygiene

Keep the repository understandable.

Requirements:

- README explains setup and purpose
- no committed secrets
- no unexplained generated junk
- no duplicate abandoned source trees without documentation
- remove unused components when safe
- keep dependency list intentional
- use meaningful commit messages
- record architecture-changing decisions
- keep migrations under version control

Do not silently delete working product history solely to create a “clean” rewrite.

---

## 25. Testing

Testing must validate behavior, not screenshots alone.

Depending on the project, verify:

- production build
- routing
- authentication
- authorization
- database access rules
- forms
- API failure behavior
- payments in sandbox
- emails
- uploads
- multilingual layout
- mobile navigation
- critical user journeys
- admin restrictions
- cross-user isolation
- agent approval boundaries
- analytics events
- production environment configuration

For every failed test:

```
identify -> fix -> rerun -> record
```

Do not replace testing with the statement “should work”.

---

## 26. Quality gate

Before declaring a release ready:

- no dead buttons
- no fake forms
- no links to `#` presented as working navigation
- no unresolved critical console errors
- no exposed secrets
- no unauthorized cross-account data access
- no contradictory product rules
- no hidden placeholder text
- no unsupported production claim
- no critical mobile breakage
- no known P0/P1 bug left undocumented

If a known limitation remains, document it explicitly.

---

## 27. Definition of Done

A feature is done only when:

1. implementation exists,
2. primary path works,
3. failure path is handled,
4. authorization is correct,
5. responsive behavior is acceptable where relevant,
6. tests appropriate to risk have passed,
7. no secret was exposed,
8. documentation is updated where needed,
9. known limitations are stated,
10. the result can be reproduced by another developer or agent.

“Code written” is not the same as “done”.

---

## 28. Delivery status language

Repository and sprint reports should use these labels consistently:

### DONE
Implemented and integrated.

### TESTED
Verified through the stated test path.

### FAILED
Tested and currently not working.

### BLOCKED
Cannot proceed until a dependency, credential, decision, permission, or external condition changes.

### NEXT
Highest-priority next action.

Do not hide FAILED or BLOCKED items to make progress appear better.

---

## 29. Change control

Major product decisions should be recorded in `06_DECISIONS_LOG.md`.

Record:

- date
- decision
- reason
- alternatives considered
- impact
- owner

A later agent should not reverse an intentional decision simply because it prefers a different implementation.

---

## 30. Project-specific extension rule

Each project must define its own business truth separately from this standard.

Examples:

- product name
- users
- pricing
- services
- workflows
- roles
- business rules
- project-specific data model
- project-specific design identity
- project-specific integrations
- project-specific launch scope

Those belong in `01_PROJECT_MASTER_SPEC.md` and related project files.

Do not put one project’s special rules into this global standard.

---

## 31. Current-sprint discipline

Keep active work narrow.

`08_CURRENT_SPRINT.md` should define:

- sprint goal
- in-scope work
- out-of-scope work
- acceptance criteria
- dependencies
- test plan
- completion status

This prevents an AI agent from attempting to reinterpret or rebuild the whole product on every task.

---

## 32. Final operating rule

The purpose of this standard is not to make documentation larger.

The purpose is to make execution:

- clearer
- safer
- faster
- cheaper
- more testable
- easier to hand off
- less dependent on one model, tool, developer, or conversation

When documentation and implementation disagree, verify the real system, record the discrepancy, and repair the source of truth.

---

## Repository adoption record

When this file is added to a repository, that repository adopts **AMAN Product Build Standard v1.0** as its default product and engineering operating standard until superseded by a newer version.

**Canonical origin:** ChatGPT conversation with AMAN, 2026-10-06.  
**Reference theme:** “Anthropic designer (ex. Apple)” specification pattern -> AMAN-wide product build system.
