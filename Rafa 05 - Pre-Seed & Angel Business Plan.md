

# **Rafa Pre-Seed Business Plan**

## *Angel, founder-network, and friends-and-family financing for a gated cinematic production factory*

| Control | Value |
| :---- | :---- |
| Document ID | RAFA-BP-ANGEL-001 |
| Investment audience | Founder-aligned angels, operator investors, creative-technology executives, and early strategic supporters |
| Planning horizon | Thirty-six-month company plan with an eighteen-month implementation baseline |
| Illustrative capital request | $1.75 million illustrative pre-seed financing |
| Status | Investor-diligence working plan; assumptions are explicitly labeled and subject to validation |
| Architecture position | Cinematic/Virtual Production pipeline; Unreal Engine is an automated structural rendering service, not a game product |

| Source basis: This plan is derived from the Rafa Company & Technical Master Requirements, Engineering Execution Manual, Seven-Year Business Playbook, Five-Year Operations Manual, Twelve-Month Onboarding Playbook, and the supplied cinematic/virtual-production architecture notes. External market-size figures are not presented as verified facts; the financial sections use bottom-up planning assumptions that must be replaced with diligence data. |
| :---- |

### **Companion documents**

* Rafa 00 \- Company & Technical Master Requirements  
* Rafa 01 \- Engineering Execution Manual  
* Rafa 02 \- Business Playbook (Seven-Year Operating System)  
* Rafa 03 \- Operations Manual (Five-Year Infrastructure & Administration)  
* Rafa 04 \- Recruiting, Hiring & 12-Month Onboarding Playbook

# **Static contents**

1. Executive summary and investment proposition  
2. Company, problem, and category thesis  
3. Cinematic/Virtual Production architecture  
4. Product and service model  
5. Bottom-up market model and customer discovery  
6. Go-to-market and pilot strategy  
7. Competition and defensibility  
8. Eighteen-month implementation and hiring plan  
9. Operating model, governance, and founder controls  
10. Illustrative three-year financial plan  
11. Financing structure, use of funds, and tranche gates  
12. Milestones, investor reporting, risks, and return paths  
13. First 100 days and diligence appendix

# **Executive summary and investment proposition**

| Investment proposition: Finance the proof that a small senior team can transform Unreal Engine, orchestration, governed production data, and provider-neutral AI into a repeatable cinematic production factory. The pre-seed round funds architecture, a private atomic render service, Rafa state and rights controls, human approvals, a small paid-pilot slate, and the evidence needed for institutional financing. |
| :---- |

Rafa is being built as a software-defined cinematic and virtual-production company. It converts approved creative intent into versioned production manifests, deterministic structural renders, controlled generative polish, post-production packages, and auditable delivery. The company earns early revenue through paid discovery, atomic proofs, and pilots while building reusable software and production assets.

The pre-seed investor is underwriting a sequence of de-risking events rather than a finished enterprise platform. Capital is released against promotion gates: architecture authorization, twenty repeatable atomic renders, authoritative project-to-artifact lineage, fault-tolerant orchestration, version-specific human approvals, validated provider polish, and a paid end-to-end pilot.

| Item | Pre-seed position |
| :---- | :---- |
| Illustrative raise | $1.75M, subject to legal and financial diligence. |
| Instrument | Post-money SAFE or convertible security for speed; priced equity is an alternative if governance or tax needs justify it. |
| Target runway | Approximately 18 months under the base operating plan, with hiring and infrastructure released by gate. |
| Primary outcome | End-to-end pilot capability and evidence that customers will pay for repeatable, governed virtual production. |
| Initial revenue model | Paid discovery, atomic proof, pilot packages, and tightly bounded production work. |
| Follow-on thesis | Institutional seed after G6/G7 evidence: reliable pilot delivery, customer references, unit-economics baseline, and a credible platform roadmap. |
| Investor fit | Patient operators who understand software, studios, production technology, infrastructure, or enterprise workflow. |

## **Assumption and diligence policy**

* Financial projections are management planning scenarios, not forecasts or promises. They must be rebuilt from validated pilot throughput, pricing, labor, provider, infrastructure, rights, and customer-acquisition data.  
* No external total-addressable-market figure is treated as verified in this document. Market potential is modeled bottom-up using target customer counts, annual contract value, production volume, and expansion assumptions.  
* Technical performance claims from source notes become hypotheses until benchmarked against the approved Unreal Engine version, GPU nodes, storage fabric, provider APIs, and delivery profile.  
* Legal, securities, employment, tax, privacy, biometric, copyright, performer, voice, and training-data matters require qualified counsel in the applicable jurisdiction.  
* The company releases capital by evidence gates. A missed gate triggers corrective action, scope reduction, pause, financing revision, or stop—not automatic continuation.

# **Company, problem, and category thesis**

## **Company mission**

Build a trusted software-defined production company that converts approved ideas into consistent, traceable, rights-aware cinematic media while retaining the creative judgment, technical knowledge, and verified execution data required to improve every subsequent project.

## **Customer problem**

* AI video can create impressive frames, but production teams struggle with identity drift, camera inconsistency, geometry errors, temporal instability, rights uncertainty, and repeated manual rework.  
* Virtual-production and Unreal workflows often depend on skilled operators manually opening projects, adjusting scenes, launching renders, moving files, and reconstructing context after failures.  
* Creative approvals, asset rights, model parameters, provider costs, and delivery decisions are frequently split across chat, spreadsheets, vendor dashboards, local disks, and individual memory.  
* Customers need controlled revisions, recurring characters and environments, professional delivery specifications, and accountable approvals—not only a compelling generation demo.  
* Existing software categories separate creative tooling, render infrastructure, workflow automation, asset management, and AI generation; Rafa connects them under one production contract.

## **Category definition**

Rafa defines the category as a cinematic production operating system plus an expert production service. The company first proves the workflow by operating it. Over time, repeated components become platform capabilities, standard services, customer integrations, and proprietary production intelligence. The service layer creates revenue and training data; the software layer creates leverage, consistency, switching cost, and margin expansion.

# **Cinematic/Virtual Production architecture**

| Architecture doctrine: Rafa is a software-defined cinematic and virtual-production company. Unreal Engine is treated as a deterministic scene-construction and offline-rendering service inside a governed production factory. The company is not building a game, a gameplay loop, or a consumer game client. |
| :---- |

## **Why the category remains under-automated**

The opportunity exists because the required skill set spans disciplines that normally live in separate organizations. Cinematographers, character artists, and animators understand visual quality but may not design durable orchestration, database schemas, APIs, CI/CD, security boundaries, or failure recovery. Software and infrastructure engineers understand those controls but may not understand camera language, Sequencer, Control Rig, MetaHuman, Chaos, timing, and editorial acceptance. Rafa is designed around the deliberate merger of those disciplines.

* Art-versus-infrastructure gap: creative practitioners and distributed-systems engineers rarely share one production operating model.  
* Game-engine mindset trap: most teams treat Unreal as an interactive editor or game runtime instead of a scriptable cinematic rendering service.  
* Upfront systems cost: reliable automation requires governed data, private render control, storage throughput, GPU capacity, reproducible templates, monitoring, and recovery before scale.  
* Artistic-control misconception: automation does not remove control; it encodes approved creative intent into versioned parameters, assets, constraints, and review gates.  
* Provider volatility: generative providers change models, policies, pricing, quotas, and formats, so the company must own the normalized contract and workflow state.

## **Reference production topology**

| Approved brief / script / production request        |        v\[n8n interaction and human approval plane\]        |        v\[Rafa PostgreSQL: versions, rights, state, cost, lineage\]        |        v\[Kestra durable execution and recovery\]        |        v\[Authenticated edge render gateway\]        |        v\[Unreal Sequencer \+ Control Rig \+ Chaos \+ MRQ\]        |        v\[NAS/object storage: structural passes and evidence\]        |        v\[Provider-neutral polish adapter\]        |        v\[Audio, lip-sync, editorial, QC, release, archive\] |
| :---- |

## **Cinematic implementation patterns**

| Pattern | Required design | Business consequence |
| :---- | :---- | :---- |
| Sequencer as source of truth | Each shot is represented by a governed Level Sequence or sequence configuration generated from the approved manifest. | Shot state becomes versionable, reproducible, auditable, and portable across render nodes. |
| Spawnables where appropriate | Shot-scoped actors and props are spawned by Sequencer rather than left permanently resident in the level. | Reduces state leakage and memory accumulation across long unattended queues; requires benchmark validation per asset type. |
| Offline deterministic evaluation | Chaos, animation, cloth, hair, particles, and camera are evaluated under frame-controlled rendering rather than real-time gameplay assumptions. | Prioritizes repeatability and quality over interactive frame rate. |
| Control Rig for procedural performance | Breathing, muscle tension, joint offsets, facial controls, and procedural transforms live in Control Rig or validated native systems, not heavyweight per-frame Blueprint logic. | Preserves creative programmability while reducing CPU overhead and rewrite risk. |
| Subsystem ingestion model | A custom Editor/Engine Subsystem receives validated manifests and sets global production state; scene actors do not poll external systems. | Centralizes validation, reduces hidden state, and keeps actors focused on rendering behavior. |
| C++ for heavy array math | Large vector, weather, transform, facial, and simulation arrays are processed in native code or optimized engine facilities and exposed through narrow interfaces. | Improves throughput, predictability, testability, and maintainability. |
| No production Event Tick abuse | Expensive I/O, database access, weather queries, and large loops do not execute every frame in general Blueprints. | Avoids CPU bottlenecks that waste GPU capacity and make queue economics unpredictable. |
| MRQ warm-up and pre-roll | Engine and render warm-up frames settle physics, cloth, hair, particles, exposure, and caches before recorded frame one. | Prevents first-frame defects and reduces manual rework. |
| Private edge control | Kestra calls an authenticated edge gateway; Unreal Remote Control and workstation administration are never public endpoints. | Enables enterprise security, auditability, and insurability. |

## **Investor relevance**

For a pre-seed investor, the architecture is a de-risking plan. The objective is not to build every possible feature. It is to prove the smallest private system that can repeatedly render one approved shot, recover from failure, preserve evidence, and expand without discarding the core contracts. Each architectural choice limits a class of future rewrite.

# **Business model and offer ladder**

| Offer | Scope | Commercial model | Primary proof |
| :---- | :---- | :---- | :---- |
| Discovery and production architecture | Paid feasibility, rights/data review, workflow design, pilot plan, benchmark definition. | Fixed fee. | Qualified use case with clear acceptance, economics, and governance. |
| Atomic proof | One governed shot or short cinematic fixture proving camera, character, environment, structural render, polish, and delivery. | Fixed fee with bounded revisions. | Repeatability and benchmark package. |
| Pilot package | Small sequence, campaign unit, or internal studio workflow with approvals, variants, QC, and evidence. | Milestone-based fixed fee or capped time-and-materials. | Customer value, cycle time, cost, quality, and adoption. |
| Repeat production service | Reserved production capacity, reusable assets, scheduled output, service levels, and ongoing optimization. | Retainer/capacity reservation plus usage. | Recurring revenue and asset reuse. |
| Platform-assisted enterprise workflow | Customer integration, governed portal/API, policy, rights, reporting, provider abstraction, and support. | Implementation \+ annual subscription/support \+ usage. | High switching cost and expansion revenue. |
| Owned or co-produced IP | Selective company investment in licensed or owned content using the platform. | Project finance, licensing, distribution, revenue share. | Long-duration asset value; ring-fenced risk capital. |

## **Initial customer selection**

| Strong fit | Weak fit / caution |
| :---- | :---- |
| Recurring characters, environments, campaigns, episodic work, franchise consistency, approval-heavy production, structured iteration, asset reuse, professional delivery specifications. | Disposable one-off clips where drift is acceptable, unclear rights, undefined decision authority, unlimited unpriced revisions, or buyers comparing solely with lowest-cost commodity generation. |
| Brands, studios, agencies, publishers, training/simulation producers, and enterprises that value traceability, repeatability, security, and controlled iteration. | Speculative content factories that cannot supply lawful source material, approval ownership, or commercial payment discipline. |

## **Revenue streams**

* Discovery, feasibility, architecture, and rights/data-readiness engagements.  
* Atomic proofs and pilot production packages.  
* Repeat production retainers and reserved capacity.  
* Enterprise implementation, integration, and migration services.  
* Annual platform subscription, support, governance, and reporting fees.  
* Usage-based render, provider, storage, and workflow charges with transparent overage rules.  
* Strategic partner licensing, joint solution, or preferred-provider revenue.  
* Owned/co-produced IP licensing and distribution only after core operations meet investment gates.

# **Bottom-up market model**

| Market-sizing rule: This plan deliberately avoids unsupported top-down market-size claims. The addressable opportunity is modeled from target account classes, validated willingness to pay, annual contract value, production volume, and expansion behavior. Management must replace each planning assumption with documented customer discovery and signed commercial evidence. |
| :---- |

## **Illustrative first-market wedge**

| Customer/account class | Illustrative account universe | Illustrative annual value | Revenue potential logic |
| :---- | :---- | :---- | :---- |
| Design partners and innovative studios | 10-25 qualified prospects | $50K-$250K pilot or annualized work | Win 2-5 paid design partners, convert 1-3 to repeat production, and document value/cycle-time proof. |
| Brand/agency repeat campaigns | 25-75 target accounts | $100K-$500K annual value | Recurring characters and campaign variants create reuse and approval value. |
| Publishers and entertainment rights holders | 10-40 target accounts | $100K-$750K annual value | Franchise consistency, approvals, and owned assets support higher-value pilots. |
| Enterprise training/simulation teams | 20-60 target accounts | $75K-$400K annual value | Traceability, repeatability, and professional delivery can matter more than artistic novelty. |

The model is not a claim that every account will buy. It is a prioritization tool. The company should pursue segments where the buyer has recurring production needs, high cost of inconsistency, approval or rights complexity, valuable reusable assets, and a clear owner for adoption and budget.

# **Go-to-market and pilot strategy**

## **Pre-seed commercial sequence**

14. Recruit three to five design partners through founder, advisor, virtual-production, agency, studio, and enterprise networks.  
15. Sell a paid discovery and production-architecture engagement before accepting a complex pilot. The engagement defines rights, assets, desired output, approval authority, technical specifications, benchmarks, budget, and stop conditions.  
16. Deliver an atomic proof using one character, one environment, one camera system, one approved render profile, and a bounded polish and review loop.  
17. Convert strong proofs into a ten-to-fifty-shot pilot or repeat campaign unit with transparent milestone billing and revision limits.  
18. Measure estimate-to-actual labor, render time, provider cost, storage, review wait, first-pass acceptance, revisions, defects, and customer value.  
19. Standardize the winning use case and reject custom work that bypasses the platform or obscures economics.

## **Pilot qualification gate**

| Requirement | Pass condition |
| :---- | :---- |
| Customer problem | Consistency, traceability, reuse, or approval complexity is economically meaningful. |
| Rights | Source, identity/performer, voice, model/provider, distribution, and reuse rights have a viable clearance path. |
| Decision authority | Customer names the person who can approve scope, creative versions, and final delivery. |
| Delivery | Resolution, frame rate, duration, audio, caption, codec, and review method are defined. |
| Revisions | Included rounds, response deadlines, change-order rules, and cancellation are explicit. |
| Economics | Price covers direct cost, contingency, and learning value under an approved loss-leader cap if applicable. |
| Learning | Pilot will generate reusable technical, commercial, quality, or workflow evidence. |

# **Competitive landscape and differentiation**

| Alternative | Strength | Limit | Rafa differentiation |
| :---- | :---- | :---- | :---- |
| Traditional VFX/virtual-production studio | Creative craft, relationships, established production practices. | High manual coordination; knowledge may remain project-specific; limited software productization. | Encodes repeatable production contracts, lineage, approvals, cost, and recovery while preserving senior creative review. |
| AI video generation vendor | Fast model access and visually impressive outputs. | Provider-specific, variable control, weak integration with enterprise rights and production systems. | Provider-neutral orchestration anchored by deterministic structural rendering and governed state. |
| Game-development studio using Unreal | Deep engine skills and real-time systems experience. | May optimize for gameplay loops, client runtime, network replication, and editor-centric workflows. | Optimizes for cinematic Sequencer, offline physics, MRQ, evidence, headless render operations, and post-production. |
| Cloud render farm | Elastic compute and operational scale. | Renders supplied workloads but does not own creative intent, manifest, approvals, rights, or provider polish. | Combines business workflow, creative contracts, render control, QC, and delivery. |
| In-house enterprise creative team | Brand knowledge and direct stakeholder access. | Tool fragmentation, limited platform engineering, single-provider dependency, and staffing constraints. | Deployable production operating system, specialist team, and transfer of governed capability. |
| Generic workflow automation consultancy | Can connect SaaS tools quickly. | Often lacks Unreal, media, editorial, rights, and GPU-production expertise. | Deep cross-domain cinematic production architecture and reusable software assets. |

The pre-seed differentiation is execution discipline. A competitor can access the same engine or model, but reproducing Rafa requires cross-domain architecture, versioned production data, a private render-control layer, rights-aware assets, durable orchestration, editorial conformance, and a team that can operate the whole system. The initial moat is know-how and integration; the long-term moat is the accumulated library of validated workflows, assets, evaluations, economics, and customer integrations.

# **Eighteen-month implementation and hiring plan**

| Period | Technical and commercial focus | Core staffed capabilities | Promotion evidence |
| :---- | :---- | :---- | :---- |
| Months 0-2 | Architecture, contracts, security/storage foundation, atomic scene, customer discovery. | Founder/CEO, fractional architect, TPM/producer, senior backend/data, SRE, Unreal TD, QA; fractional legal/rights. | G0 and early G1 evidence; three qualified design partners. |
| Months 2-4 | Rafa schema, artifact registry, rights eligibility, edge gateway, repeatable UE structural package. | Add rendering-platform and storage/network depth. | G1: twenty consecutive repeatable renders; G2 lineage and restore. |
| Months 4-7 | Kestra durability, n8n approval gates, telemetry, fault recovery, first paid proof. | Add workflow automation, observability, product/production support. | G3/G4; paid atomic proof and full audit. |
| Months 6-9 | Provider-neutral polish, cost/quality controls, customer review. | Add AI integration and ML evaluation support. | G5; provider lifecycle and bounded cost. |
| Months 8-11 | Audio, lip-sync, editorial, QC, delivery and archive. | Add media systems/post and rights operations. | G6; end-to-end pilot delivery. |
| Months 10-14 | Small pilot slate, support, pricing, repeat offer, case-study permissions. | Add production coordination and specialized QA as needed. | G7; quality, cycle time, margin, and customer value. |
| Months 14-18 | Multi-project preparation, DR, security hardening, institutional financing package. | Selective engineering/security/client-success additions only after thresholds. | G8 scale readiness and seed-round diligence. |

## **Initial role scorecard**

| Role | First accountable outcome | Hiring approach |
| :---- | :---- | :---- |
| Founder/CEO | Capital, design partners, strategy, governance, and hiring gates. | Founder. |
| Head of Engineering / Architect | Approved architecture and evidence-producing atomic render service. | Fractional-to-full-time based on candidate and capital. |
| Technical Program Manager / Producer | Integrated plan, gate packets, customer/pilot coordination, risk and evidence. | Early full-time or senior contract-to-hire. |
| Backend/Data Engineer | Rafa schema, state, rights, asset, job, audit, and cost foundation. | Senior full-time. |
| Platform/SRE Engineer | Private environments, CI/CD, secrets, observability, backup, recovery. | Senior full-time. |
| Unreal Technical Director | Sequencer-based atomic scene, Control Rig/physics patterns, MRQ package. | Senior full-time or dedicated long-term contractor. |
| Workflow Automation Engineer | Kestra/n8n flows with durable state, approvals, retries, and reconciliation. | Full-time after contracts/edge baseline. |
| QA/Media Systems | Fixture corpus, resilience tests, ffprobe/QC, release evidence. | Combined early role or specialist contract; split later. |
| Legal/Rights/Privacy | Company, contracts, consent, asset eligibility, provider and training-data controls. | Outside counsel/fractional specialist. |

# **Operating model, governance, and founder controls**

* Board or investor observer receives monthly cash, hiring, technical gate, pilot, risk, and use-of-proceeds reporting.  
* No broad production scaling before G7; no major infrastructure expansion before measured utilization and queue data.  
* Every hire requires a role scorecard, manager, twelve-month onboarding owner, equipment/software budget, and first-year outcomes.  
* Every pilot requires project charter, rights plan, acceptance specification, payment milestones, revision rules, cost forecast, and stop conditions.  
* Material architecture, security, provider, rights, and capital decisions are recorded and reviewed rather than decided through informal chat.  
* Founder maintains strategy and capital authority while technical and release owners have explicit domain decision rights.

# **Illustrative three-year financial plan**

| Financial basis: Illustrative management scenario in USD. It is not a forecast. The model assumes a service-led launch, gradual platform revenue, measured hiring, and no external market-size claim. Replace with actual pipeline, pricing, throughput, utilization, and accounting policy during diligence. |
| :---- |

| USD millions except counts | Year 1 | Year 2 | Year 3 |
| :---- | :---- | :---- | :---- |
| Paid discovery / proofs | 0.20 | 0.35 | 0.50 |
| Pilot and production services | 0.35 | 1.55 | 3.20 |
| Platform / support / usage | 0.00 | 0.20 | 1.10 |
| Total revenue | 0.55 | 2.10 | 4.80 |
| Gross margin | 28% | 43% | 55% |
| Operating expense | 1.65 | 2.45 | 3.10 |
| Illustrative EBITDA | (1.50) | (1.55) | (0.46) |
| Ending full-time equivalents | 7 | 12 | 18 |
| Active paying customers | 3-5 | 8-12 | 15-22 |
| Repeat / recurring share | 10% | 35% | 55% |

Base-case financing logic: the pre-seed round is expected to carry the company through architecture, atomic reliability, Rafa state, orchestrated approvals, one provider, post-production, and the first pilot evidence. A follow-on institutional round may be required before break-even. Management should maintain a twelve-month rolling cash forecast and begin financing preparation no later than nine months of cash remaining or earlier if the next gate requires long-lead hiring or infrastructure.

## **Illustrative pilot unit economics**

| Pilot element | Illustrative assumption | Validation required |
| :---- | :---- | :---- |
| Pilot price | $75K-$250K depending on sequence length, asset readiness, rights, and integration. | Signed proposals and win/loss evidence. |
| Direct labor | 35%-50% of price in early pilots. | Time tracking by production stage and role. |
| Provider/cloud/local compute | 5%-15% of price with variation contingency. | Per-job cost events and actual invoices. |
| Rights/external specialists | Project-specific; pass-through or separately priced. | Contracts and receipts. |
| Target contribution margin | 25%-40% early, expanding through reuse and software. | Estimate-to-actual project close. |
| Revision reserve | 10%-20% of labor/compute within explicit included rounds. | Change and revision logs. |
| Cash terms | Deposit plus milestone payments before heavy compute and delivery. | Contract and receivables behavior. |

# **Financing structure, use of funds, and tranche gates**

| Use of proceeds | Illustrative allocation | Purpose |
| :---- | :---- | :---- |
| Product and engineering | 49% / $857,500 | Core team, repositories, Rafa, edge gateway, Unreal automation, workflows, provider gateway, QC. |
| Infrastructure and production equipment | 14% / $245,000 | Approved workstations/render nodes, NAS/network/backup improvements, test hardware, software. |
| Pilot production and customer development | 11% / $192,500 | Producer capacity, rights-safe fixtures, specialists, demonstrations, travel, customer onboarding. |
| Legal, rights, privacy, security, insurance | 9% / $157,500 | Corporate/securities, contracts, IP, performer/voice/identity controls, security reviews. |
| Operations, finance, and administration | 8% / $140,000 | Accounting, payroll, software, facilities, recruiting, service operations. |
| Contingency and runway reserve | 9% / $157,500 | Gate delays, equipment failure, provider changes, bridge to next financing. |

## **Suggested internal capital release**

| Tranche | Release condition | Maximum emphasis |
| :---- | :---- | :---- |
| A \- Foundation | Close, approved budget, G0, key leaders contracted. | Architecture, security/storage, atomic scene, discovery. |
| B \- Atomic reliability | G1 twenty-run evidence and no critical security exposure. | Rafa, edge gateway, artifacts, second wave staffing. |
| C \- Orchestrated pilot | G3/G4 and signed paid pilot. | n8n/Kestra productionization, customer delivery. |
| D \- End-to-end and follow-on | G6 plus validated unit economics and financing plan. | Pilot slate, reliability, seed diligence. |

## **Illustrative security terms**

* Instrument, valuation cap, discount, interest, maturity, pro-rata, MFN, information rights, and conversion mechanics must be set by qualified securities counsel.  
* Founder should avoid investor vetoes over ordinary operations at pre-seed; reserve approval rights for material financing, sale, charter changes, senior securities, and related-party transactions.  
* Pro-rata rights should be sized for serious follow-on investors and administered consistently.  
* No strategic exclusivity, broad IP license, training-data access, or customer restriction should be embedded in a financial SAFE without separate commercial valuation and board review.  
* Use-of-proceeds and milestone reporting may be contractual or board policy, but operating flexibility must remain sufficient to respond to technical evidence.

# **Milestones, investor reporting, and return paths**

| Milestone | Target evidence | Investment implication |
| :---- | :---- | :---- |
| G1 atomic reliability | Twenty consecutive unattended renders from the same manifest with correct passes, names, checksums, and logs. | Validates the core edge/Unreal concept. |
| G2 governed data backbone | Project-to-artifact lineage, rights controls, roles, and restore test. | Reduces enterprise and scaling risk. |
| G4 human-directed workflow | Version-specific approvals and rejected-version blocking. | Demonstrates usable production governance. |
| G5 polish adapter | Submit/status/cancel/result/cost contract, technical validation, bounded budget. | Reduces vendor lock-in and proves AI integration. |
| G6 paid end-to-end pilot | Conformant master, approvals, rights, delivery receipt, actual economics. | Primary institutional financing trigger. |
| G7 repeatability | Multiple projects or a pilot slate meeting quality, schedule, support, and customer-value thresholds. | Supports seed/Series A or strategic partnership. |

* Potential return paths include institutional growth financing, strategic minority investment, acquisition by a studio/creative platform/cloud/media-technology company, or durable cash-generating studio/platform operations.  
* An early investor should not depend on a single exit narrative. Management must create optionality through software IP, recurring contracts, governed assets, data/evaluation systems, and disciplined capitalization.  
* No return case should rely on unlicensed training data, speculative model ownership, or unlimited production scale without operational proof.

# **Risk register and mitigation**

| Risk | Impact | Mitigation | Owner |
| :---- | :---- | :---- | :---- |
| Technical integration complexity | Schedule and cost overrun. | One-shot-first gates, contract tests, private edge gateway, fault injection, narrow supported versions. | Head of Engineering |
| Unreal unattended reliability | Failed renders and capacity loss. | Process supervisor, preflight, pinned versions, warm-up profiles, artifact verification, worker recovery. | Rendering Platform Lead |
| Provider volatility | Quality, cost, quota, or policy disruption. | Normalized adapters, second-provider substitution test, circuit breakers, budget caps, local benchmark path. | AI Platform Lead |
| Rights and identity misuse | Legal, reputational, and contractual exposure. | Asset rights registry, performer/voice consent, eligibility policy, restricted access, release blocking. | Legal/Rights Lead |
| NAS or infrastructure failure | Production interruption or data loss. | Segmentation, snapshots, immutable/offsite backup, checksums, restore drills, capacity headroom. | Infrastructure Lead |
| Customer concentration | Revenue and negotiating risk. | Segment limits, pipeline diversification, standard offers, concentration thresholds. | CEO/Commercial Lead |
| Scaling before repeatability | Burn increase without durable economics. | Stage-gated funding, hiring waves, pilot metrics, stop conditions. | CEO/Board |
| Creative review bottleneck | Long cycle time despite automation. | Structured review rubrics, parallel review capacity, decision deadlines, sampling, reusable templates. | Studio Operations |
| Hiring cross-domain talent | Execution gaps at system boundaries. | Competency-based work samples, senior founding team, twelve-month onboarding and certification. | Head of People/Engineering |
| Unverified performance assumptions | Mispriced projects and overstated capacity. | Benchmark every engine/provider/storage claim and update cost registry before quoting scale. | FinOps/SRE |

# **First 100 days after financing**

| Period | Required actions | Evidence |
| :---- | :---- | :---- |
| Days 1-15 | Finalize corporate/securities close; appoint architecture, TPM/producer, legal/rights owners; approve G0; create budget, cash, risk, decision, and hiring registers. | Closing file, board/consents, G0 packet, 18-month integrated plan. |
| Days 16-30 | Create repositories, environments, identity/secrets baseline, synthetic fixtures, asset/right schema draft, atomic UE project, customer discovery calendar. | CI baseline, security inventory, atomic scene plan, 10+ discovery interviews scheduled. |
| Days 31-60 | Implement private edge path, NAS output, MRQ profile, camera/sequence fixture, artifact verifier; draft pilot offer and contract positions. | First unattended render package; pilot proposal template; benchmark log. |
| Days 61-80 | Run repeatability and failure tests; build Rafa project/shot/asset/job backbone; qualify design partners. | G1 progress, database migration tests, three qualified pilot candidates. |
| Days 81-100 | Complete G1 packet or corrective plan; select first paid atomic proof; authorize next hiring and integration tranche. | Gate decision, signed proof/pilot, updated cash and risk forecast. |

# **Diligence data room checklist**

| Workstream | Required evidence before close |
| :---- | :---- |
| Corporate and capitalization | Entity documents, cap table, prior securities, founder vesting, option plan, board/consent history, related-party disclosures. |
| Intellectual property | Founder/employee/contractor assignments, licenses, trademarks/domains, source asset rights, model/provider terms, proprietary code inventory. |
| Technology | Architecture, repositories, SBOM, security review, test results, atomic render evidence, uptime/cost data, roadmap, technical-debt register. |
| Data and privacy | Data map, classification, retention, performer/voice/identity consent, provider transfers, training eligibility policy, incident history. |
| Commercial | Customer discovery, contracts, pipeline evidence, pricing, pilot results, churn/renewal data, concentration, backlog, delivery acceptance. |
| Financial | Historical statements, budget, cash forecast, revenue recognition, cost attribution, provider/cloud commitments, taxes, capitalization of development costs policy. |
| People | Team bios, employment/contractor agreements, compensation, hiring plan, onboarding system, key-person dependencies, succession. |
| Operations | Vendor register, insurance, facilities, network/storage inventory, backup/restore, business continuity, service ownership. |
| Regulatory and legal | Material contracts, disputes, employment/tax compliance, export/sanctions analysis where relevant, content/advertising claims, securities counsel review. |
| Investment-specific | Pre-seed/angel investor rights, information rights, milestones, use-of-proceeds controls, conflict policy, transfer restrictions, governance, exit and downside protections. |

