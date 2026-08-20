# Johannes Neutze

**Engineering Manager — merchant platforms at Wolt (DoorDash). Munich.**

I lead teams that own merchant-facing order workflows and the platforms underneath them. Right now that is a greenfield CRM I founded and hired from zero, and before it the order workflows 150k+ merchants in 25+ countries depend on to trade. I came up through Android engineering, and I still pick the boring, reversible option most of the time — the rejected alternative is usually the interesting part of the story.

## High-scale platform reliability

I ran Zalando's Apps Lifecycle team: the CI/CD, release tooling, and developer-productivity systems every mobile team at the company built on, for apps with 20M+ monthly active users. The work that mattered was measurement before opinion — a developer survey to pick the targets, then Gradle Enterprise and XCMetrics to get real baselines. The first data showed iOS builds taking 18 minutes on CI against 4 on a laptop, which moved the argument from opinion to hardware.

Release engineering is where most of my judgment goes. A two-week release train on the client against continuous deployment on the backend; a 1% global canary ahead of full rollout; feature flags for per-market enablement; a documented rollback to last-known-good, which we exercised within 24 hours of a regression being declared a release blocker. The gate that decides whether a build ships is a weekly review of a reliability metric with a defined response below the line — inherit a team whose crash reporting has exhausted its own quota and the first fix is a signal someone will act on, not more data.

I also ran a virtual 24×7 incident-responder pool for a whole mobile organization — around ten engineers staffed from other teams rather than from my five reports, recruited on the development value of incident exposure rather than mandated — and stood as Incident Commander through Cyber Week, shipping behind risk-assessed release admission instead of a code freeze.

## Systems architecture

Kotlin and Scala services on Kafka, gRPC and REST, integrating with Salesforce and a merchant onboarding platform. The recurring call across four companies is an incremental, reversible migration over a rewrite, with the alternative named and costed:

- **The Tazz migration.** I was DRI for moving 5,174 merchants and 12,728 venues — commissions, contracts, banking and KYB — onto the platform after the acquisition. I routed it through the existing merchant onboarding service instead of writing a faster bulk importer. That cost throughput and kept every legal and compliance check running. It landed a week early with no production incidents raised.
- **The commission-sync connector.** Built in Kotlin on the Kafka platform the company already ran, rather than standing up new infrastructure. SOX-relevant commercial-terms propagation stays reviewable by any backend engineer on the team, which is the property I actually wanted.
- **A rollback at the last step.** I reverted a backend framework migration at its final stage, on production symptoms, against a licensing deadline and a lot of sunk cost. The team completed it later, on its own terms.
- **A sidecar instead of a rewrite.** Modernizing a legacy Java calendar app with hundreds of thousands of users, I moved features toward Kotlin alongside the old stack rather than freezing delivery for a rebuild.
- **Layered architecture, built once.** Presentation, domain use cases, data behind repositories, dependency injection scoped per feature — shared across two Android apps in a regulated digital-health product, so authentication, networking and persistence were built once rather than per screen.

## Engineering management and team topology

Two teams from zero to three — Selfapy's mobile team while still writing code, and Merchant CRM's engineering team through 43 hiring-manager interviews after a hiring process that had produced nothing in four months. Around 150 interviews and 10–15 hires across my time at Wolt. I have coached a senior engineer to Staff by closing the specific visibility gap a prior cycle had flagged, and a former report through his first months as a team lead.

The mechanisms I care about are the ones that outlast me:

- **Decision records as a habit.** An RFC and ADR practice plus a documented shortcuts register, so trade-offs stop being oral history and a deliberate short-term debt has a name, an owner and a fix.
- **Matrix delivery without authority.** Running a workstream as named DRI over borrowed senior and staff engineers who report elsewhere, in a programme where thirteen workstreams shared dependencies.
- **Capacity argued in the open.** I won 30% of the year for technical fundamentals and 15% for defects by writing the deferred product work into the roadmap next to it. The documented deferral is the mechanism — without it the split is just a number someone agreed to once.
- **Ownership designed into the process.** Rotational release management and feature owners writing their own release notes distribute responsibility structurally instead of routing it through me.

## Repositories

Nearly everything I build is employer-owned or private. What is public is academic — TU München, and still the problem domain I work in:

| Repository | What it is |
| --- | --- |
| [`master-latex-thesis`](https://github.com/neutze/master-latex-thesis) | Master's thesis: Android cracking tools and developer counter-measures. LaTeX sources. |
| [`paper-latex-cracking`](https://github.com/neutze/paper-latex-cracking) | Paper draft on circumventing major Android protection methods. |
| [`idp-latex-report`](https://github.com/neutze/idp-latex-report) | Interdisciplinary project: delivery-time forecasting and real-time order tracking. |
| [`praktikum-android-routem`](https://github.com/neutze/praktikum-android-routem) | Automotive services practical, Android/Java. |

## Elsewhere

- [LinkedIn](https://www.linkedin.com/in/johannes-neutze)
- [resume@neutze.de](mailto:resume@neutze.de)
