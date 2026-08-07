# 

# **Rafa Company & Technical Master Requirements**

## ***Authoritative company requirements, architecture boundaries, organizational design, delivery gates, and traceability model***

| Control | Value |
| :---- | :---- |
| Document ID | RAFA-REQ-001 |
| Planning horizon | Seven-year company architecture with an eighteen-month implementation baseline |
| Status | Company operating baseline; subject to controlled change |
| Primary audience | Founder, executives, managers, employees, contractors, and implementation partners |
| Authority | Approved requirements and procedures supersede informal chat, memory, and ad hoc practice |

**How to use this document:** Convert each requirement, control, milestone, and task into a tracked work item. No item is complete until its named evidence exists and its acceptance test passes. Where a vendor or product version is named, confirm the exact syntax against the approved deployment before implementation.

## **Companion document set**

* Rafa 00 \- Company & Technical Master Requirements  
* Rafa 01 \- Engineering Execution Manual  
* Rafa 02 \- Business Playbook (Seven-Year Operating System)  
* Rafa 03 \- Operations Manual (Five-Year Infrastructure & Administration)  
* Rafa 04 \- Recruiting, Hiring & 12-Month Onboarding Playbook

# **Document purpose and governing rules**

This document is the contract between business intent and executable work. It defines what the company must become, what the Rafa platform must do, which system owns each responsibility, what evidence proves completion, and which organizational capabilities must exist before the company can scale. It is written so that executives can govern the portfolio, managers can staff and sequence work, and engineers can derive tickets without inventing missing requirements.

**Authority hierarchy:** Approved requirements in this document govern over meeting notes and chat. Approved architecture decisions govern over individual implementation preference. Source-controlled specifications govern over screenshots. Production telemetry governs over anecdote. A requirement may be changed only through the change-control process defined here.

## **Static contents**

* 1\. Company charter, outcomes, and constraints  
* 2\. Operating model and architecture boundaries  
* 3\. End-to-end production lifecycle  
* 4\. Functional and non-functional requirements  
* 5\. Data, security, compliance, and intellectual-property controls  
* 6\. Organization, hiring sequence, and decision rights  
* 7\. Program roadmap, stage gates, and funding releases  
* 8\. Requirement catalog and acceptance evidence  
* 9\. Risk register, change control, and quality governance  
* 10\. Templates and implementation handoff

## **Terminology**

| Term | Definition |
| :---- | :---- |
| Rafa | The authoritative PostgreSQL and metadata platform that records projects, assets, decisions, workflow state, lineage, and evidence. |
| Production manifest | An immutable, versioned contract describing an approved project, sequence, scene, or shot and every referenced asset. |
| Structural render | A deterministic Unreal Engine render that establishes geometry, camera, timing, motion, depth, normals, and other control passes. |
| Polish pass | A provider-neutral video-to-video or diffusion process that improves appearance while preserving the approved structure. |
| Control plane | n8n, Kestra, Rafa, identity, policy, and observability services that coordinate work but do not carry large media payloads. |
| Artifact plane | NAS, object storage, caches, proxies, renders, logs, manifests, and export packages. |
| Human approval gate | A durable decision that records approver, version, decision, comments, and timestamp before downstream compute is permitted. |
| Evidence | A durable artifact that proves a requirement or task is complete: test report, deployment record, query result, log, screenshot, or signed approval. |
| Promotion gate | A stage boundary that cannot be crossed until entry criteria, acceptance tests, operational readiness, and ownership are satisfied. |

# **1\. Company charter, outcomes, and constraints**

## **1.1 Mission**

Build a software-defined production company that converts approved creative intent into consistent, traceable, economically measurable media output. The company will use deterministic scene construction, controlled generative polish, durable orchestration, and a verified-data flywheel to reduce rework while improving throughput, repeatability, and ownership of production knowledge.

## **1.2 Required business outcomes**

* A project can move from approved concept to verified master media through an auditable workflow rather than personal memory.  
* Every shot, asset, model call, render, approval, expense, and output can be traced to a project, version, owner, and evidence package.  
* The company can hire specialists into explicit roles with documented interfaces instead of relying on one generalist to understand the entire system.  
* A failed task can be retried or resumed without recreating upstream creative decisions or rerunning successful work.  
* New providers, models, rendering nodes, and editorial tools can be introduced through adapters without rewriting the core business process.  
* The production system can operate with a deliberate mix of cloud and local compute and can migrate workloads based on measured cost, latency, capacity, and risk.  
* Verified successful execution traces can be curated into training and evaluation datasets without leaking unlicensed or sensitive material.

## **1.3 Explicit non-goals for the initial operating baseline**

* The first release is not a fully autonomous studio. Human approval remains mandatory at concept, identity, shot plan, final selection, and release gates.  
* The company will not expose Unreal Engine Remote Control, databases, NAS shares, or workflow administration directly to the public internet.  
* The system will not treat any single model vendor, API, or proprietary file format as permanent infrastructure.  
* The first release will not optimize for maximum concurrency before one-shot reliability, recovery, evidence capture, and cost attribution are proven.  
* The company will not use unverified generated data as ground truth for characters, environments, cameras, legal rights, billing, or employment records.

## **1.4 Operating constraints**

| Constraint | Policy response | Management implication |
| :---- | :---- | :---- |
| Large media files | Control services exchange IDs, manifests, checksums, and signed locations; bulk bytes move through storage services. | Network and storage engineering are first-class workstreams. |
| Model nondeterminism | Constrain outputs with schemas, valid asset lists, validation, retries, and human approval. | AI output is a proposal until validated and approved. |
| Local GPU brittleness | Use health checks, queue leases, idempotency keys, retries, artifact checks, and failure states. | Capacity planning includes maintenance and degraded modes. |
| Long external jobs | Use asynchronous submit/poll/callback patterns; never hold open a synchronous request for the entire render. | Workflow state must survive process restarts. |
| Creative iteration | Version manifests and assets; invalidate only affected downstream artifacts. | Change impact must be calculated before rerendering. |
| Small initial team | Prefer managed services and configuration-first integrations while preserving provider-neutral contracts. | Hire for system boundaries, not fashionable tool names. |
| Rights and privacy | Attach provenance, license, consent, usage restrictions, retention, and deletion rules to every governed asset. | Legal and data stewardship are embedded in production, not post-production. |

# **2\. Operating model and architecture boundaries**

## **2.1 Authoritative architecture**

**Normative logical architecture**

\[Creator / Client / Internal Brief\]  
              |  
              v  
     \[n8n Interaction Plane\]  
 intake, approvals, notifications, provider job lifecycle  
              |  
              v  
        \[Rafa PostgreSQL\]  
 source of truth, versions, states, rights, costs, lineage  
              |  
              v  
       \[Kestra Execution Plane\]  
 durable orchestration, scheduling, retries, fan-out, recovery  
              |  
              v  
 \[Authenticated Edge Worker Gateway\]  
 leases jobs, validates manifests, controls local applications  
              |  
              v  
 \[Unreal Engine Structural Render\]  
 camera, world, character, animation, depth, normal, motion passes  
              |  
              v  
   \[NAS / Object Storage Artifact Plane\]  
 immutable manifests, source assets, renders, proxies, masters, logs  
              |  
              v  
 \[Provider-neutral AI Polish Adapter\]  
 submit \-\> poll/callback \-\> validate \-\> store result  
              |  
              v  
 \[Audio / Lip-sync / Editorial / QC\]  
              |  
              v  
   \[Approved Master \+ Delivery Package\]

## **2.2 System ownership matrix**

| System | Owns | Must not own | Primary accountable role |
| :---- | :---- | :---- | :---- |
| Rafa/PostgreSQL | Canonical business and production records, version state, lineage, rights, costs, evidence pointers. | Large media bytes; UI-specific transient state; secret material. | Data Platform Lead |
| n8n | Interactive intake, approval experiences, notifications, long-running external API lifecycle, light transformations. | Heavy compute; authoritative state; giant binary transfers; hidden business rules. | Workflow Automation Lead |
| Kestra | Durable workflows, task dependency, retries, schedules, fan-out, queue coordination, recovery. | Creative approvals; direct user experience; canonical media metadata. | Platform/Orchestration Engineer |
| Edge worker | Authenticated job lease, local application control, path translation, process supervision, artifact verification. | Business approval; provider-specific creative logic. | Rendering Platform Lead |
| Unreal Engine | Deterministic scene assembly and structural render outputs. | Workflow state, billing, approval decisions, cross-project asset policy. | Unreal Technical Director |
| NAS/object storage | Durable artifacts, immutability tiers, checksums, lifecycle, replication. | Workflow decisions; secret management. | Infrastructure Lead |
| AI provider adapter | Provider translation, job lifecycle, cost capture, normalized results. | Approval, rights decisions, canonical shot state. | AI Media Integration Lead |
| Editorial/QC | Conformance, timeline assembly, audio, legal delivery package, human quality decisions. | Rewriting upstream source of truth outside controlled change. | Post-Production Lead |

## **2.3 Normative architecture decisions**

| ADR | Decision | Consequence |
| :---- | :---- | :---- |
| ADR-001 | Rafa is the source of truth | Every state transition is persisted in PostgreSQL with version, actor, reason, and evidence. Workflow tools may cache but cannot become authoritative. |
| ADR-002 | n8n owns interaction; Kestra owns execution | n8n handles humans and provider job lifecycle. Kestra handles durable dependency execution, compute, retries, and recovery. |
| ADR-003 | No direct public UE control | An authenticated edge worker receives validated jobs and controls UE locally. Remote Control binds only to a protected interface. |
| ADR-004 | Artifacts are immutable by version | Approved manifests and generated artifacts are append-only. New iterations create new versions and lineage links. |
| ADR-005 | Provider-neutral contracts | Core schemas use normalized capabilities and outputs. Vendor fields live in adapter-owned extension objects. |
| ADR-006 | Cloud-first incubation, measured local migration | Use managed/API inference while stabilizing workflows. Move workloads locally only when benchmark, support, and cost gates pass. |
| ADR-007 | Configuration-first, code where boundaries require it | Use built-in nodes and declarative workflow features where they are observable and supportable; write small tested adapters for security, validation, and application control. |
| ADR-008 | Evidence-based completion | A status value alone is not completion. Required evidence and validation results must be attached before promotion. |
| ADR-009 | Rights travel with assets | License, consent, attribution, restrictions, and retention are mandatory metadata and are enforced at delivery and dataset curation. |
| ADR-010 | Observability before scale | The company will not increase concurrency until queue, cost, latency, error, and quality telemetry are reliable. |

## **2.4 Environment model**

| Environment | Purpose | Data policy | Promotion condition |
| :---- | :---- | :---- | :---- |
| Local developer | Unit tests, schema work, adapters, workflow drafts, synthetic fixtures. | No production secrets or unrestricted source media. | Automated tests and code review. |
| Development integration | Shared services and end-to-end test fixtures. | Synthetic or approved low-risk samples; isolated accounts. | Integration suite and security scan. |
| Staging | Production-like topology, performance, recovery, release rehearsal. | Sanitized or explicitly approved assets; separate credentials. | Release candidate evidence package. |
| Production | Approved client/internal workloads. | Governed source assets and least-privilege access. | Change approval, rollback, monitoring, and on-call readiness. |
| Recovery | Restoration validation and disaster operation. | Encrypted replicas and minimal services. | Quarterly restore test and annual failover exercise. |

# **3\. End-to-end production lifecycle**

| Stage | Required work | Exit evidence | Accountable |
| :---- | :---- | :---- | :---- |
| L0 Opportunity intake | Business owner records opportunity, use case, rights assumptions, target deliverables, schedule, and budget envelope. | Opportunity brief; owner; decision date; risk class. | Commercial/Studio Lead |
| L1 Project authorization | Executive sponsor approves scope, funding, rights-review path, and accountable producer. | Project charter v1; cost center; producer; approval. | Executive Sponsor |
| L2 Creative development | Story, script, style, identity, locations, and production constraints are developed under version control. | Approved script and creative bible versions. | Creative Director |
| L3 Asset and rights readiness | Assets are ingested, validated, licensed, scanned, parameterized, and linked to restrictions. | Asset readiness report; rights evidence. | Asset/Legal Steward |
| L4 Shot planning | Script is decomposed into sequences and shots with camera, blocking, performance, audio, and QC requirements. | Approved production manifest and shot plan. | Producer / Director |
| L5 Structural render | Kestra dispatches validated manifests to the edge worker and UE5; artifacts and passes are stored and checked. | Structural render package; technical QC. | Rendering Platform Lead |
| L6 Polish and variation | Approved structural render is submitted through provider adapters; variants are costed, tracked, and normalized. | Variant package; provider metadata; cost record. | AI Media Lead |
| L7 Human selection and revisions | Director selects, rejects, or requests bounded changes. Change impact creates new versions without overwriting. | Selection decision; revision request; rationale. | Director |
| L8 Audio and editorial | TTS/recorded voice, lip-sync, sound, captions, timeline, and conformance are assembled. | Editorial master candidate; cue sheet; timeline. | Post-Production Lead |
| L9 Quality and release | Technical, creative, rights, accessibility, security, and delivery checks are completed. | Release checklist; signed approvals; checksums. | Release Manager |
| L10 Delivery and archive | Approved package is delivered, acknowledged, archived, and assigned retention. | Delivery receipt; archive manifest; retention class. | Operations |
| L11 Learning and dataset curation | Eligible traces are sanitized, scored, reviewed, and included in evaluation/training datasets. | Dataset decision; provenance; quality score. | ML/Data Steward |

## **3.1 State transition rules**

Each governed entity uses explicit states and allowed transitions. A transition is an event with actor, prior state, new state, reason, idempotency key, manifest version, and evidence links. Workflows may request a transition; Rafa validates whether it is permitted. Manual database edits to bypass transitions are prohibited in production.

**Optimistic state transition example**

\-- Example transition guard  
UPDATE pipeline\_jobs  
SET state \= 'STRUCTURAL\_RENDER\_RUNNING',  
    state\_version \= state\_version \+ 1,  
    started\_at \= now(),  
    lease\_owner \= :worker\_id,  
    lease\_expires\_at \= now() \+ interval '10 minutes'  
WHERE job\_id \= :job\_id  
  AND state \= 'STRUCTURAL\_RENDER\_QUEUED'  
  AND state\_version \= :expected\_version  
RETURNING job\_id, state\_version;

# **4\. Functional requirements**

## **4.1 Portfolio, opportunity, and project control**

Portfolio control establishes why work exists, who owns the result, how much can be spent, and when leadership will stop or redirect it. These requirements prevent technical enthusiasm from becoming an uncontrolled program.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-PORT-001 | The system shall record every opportunity with sponsor, owner, intended customer, use case, value hypothesis, rights assumptions, risk class, and decision status. | Portfolio Manager | Opportunity record with all mandatory fields; validation rejects incomplete authorization. |
| REQ-PORT-002 | A project shall not enter production planning until scope, budget envelope, target delivery, accountable producer, and rights-review route are approved. | Executive Sponsor | Project authorization event and signed charter. |
| REQ-PORT-003 | Project forecasts shall separate labor, local compute, cloud compute, provider APIs, storage, external services, contingency, and expected gross margin. | Finance Lead | Approved forecast and monthly variance report. |
| REQ-PORT-004 | Every project shall define kill, pause, and escalation criteria before heavy production begins. | Producer | Charter contains thresholds and decision owners. |
| REQ-PORT-005 | Portfolio reporting shall show stage, forecast completion, spend, burn rate, quality trend, risks, and capacity demand. | PMO Lead | Dashboard reconciles to source records. |

## **4.2 Intake, briefs, and approval workflow**

Interaction must be easy for creators and managers while remaining secure, version-aware, and auditable. n8n provides the interaction workflow, but approval facts are persisted in Rafa.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-INT-001 | n8n shall accept structured briefs from approved forms, APIs, or internal systems and assign an immutable intake ID. | Workflow Automation Lead | Contract test and persisted intake event. |
| REQ-INT-002 | The intake contract shall validate required fields, maximum sizes, allowed file types, identity, and rate limits before creating project work. | Application Security Lead | Negative tests demonstrate rejection and logging. |
| REQ-INT-003 | Human approval gates shall persist approver identity, artifact version, decision, rationale, requested changes, and resulting state transition. | Product Manager | Approval record can reproduce who approved what. |
| REQ-INT-004 | Approval links shall be single-purpose, time-bounded, authenticated, and non-transferable where supported. | Security Lead | Security test and audit-log evidence. |
| REQ-INT-005 | A regenerated artifact shall not silently replace the artifact previously submitted for approval. | Workflow Lead | Versioning test confirms both versions remain available. |

## **4.3 Creative development and production manifest**

Creative development remains iterative; execution does not. Once approved, the production manifest is the typed contract that downstream systems consume.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-CR-001 | Story, script, character bible, environment bible, style bible, and shot plan shall be independently versioned and linked to approvals. | Creative Systems Lead | Version graph and approval links. |
| REQ-CR-002 | The production manifest shall reference assets by immutable version ID, not by mutable display name or local drive path. | Data Platform Lead | Schema validation and path-independence test. |
| REQ-CR-003 | The manifest shall define project, sequence, scene, shot, timing, camera, environment, characters, performance, audio, render outputs, QC, rights, and lineage. | Technical Product Manager | JSON Schema and valid canonical fixture. |
| REQ-CR-004 | The system shall calculate downstream invalidation when an approved upstream component changes. | Platform Architect | Change-impact tests for script, camera, identity, and audio changes. |
| REQ-CR-005 | LLM-generated mappings shall be constrained to permitted assets and rejected when confidence or validation thresholds are not met. | AI Platform Lead | Schema, enum, and unresolved-match tests. |

## **4.4 Asset registry, provenance, and rights**

Assets are both creative inputs and governed business property. The asset registry must make provenance and permitted use operationally enforceable.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-AST-001 | Every asset shall have an asset ID, version, checksum, media type, creator/source, acquisition method, owner, rights status, restrictions, retention class, and storage locations. | Asset Steward | Asset completeness report. |
| REQ-AST-002 | The system shall prevent use of an asset whose rights state is expired, rejected, unknown, or incompatible with the intended distribution. | Rights Operations Lead | Policy tests block prohibited combinations. |
| REQ-AST-003 | Character identity and performance capture shall record actor consent, permitted uses, territories, duration, revocation process, and deletion obligations. | Legal/Privacy Lead | Consent record linked to every derived identity asset. |
| REQ-AST-004 | Derived assets shall preserve lineage to every source asset, transformation, model, prompt, seed, and operator. | Data Governance Lead | Lineage traversal from master to source. |
| REQ-AST-005 | Duplicate ingestion shall be detected by checksum and dispositioned without creating ambiguous identities. | Asset Platform Engineer | Duplicate fixture test. |

## **4.5 Rafa data platform**

Rafa is not a loose collection of tables. It is the governed transactional backbone for company and production state.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-DATA-001 | Rafa shall enforce normalized identifiers, foreign keys, constrained states, version numbers, audit events, and timestamps for governed entities. | Data Platform Lead | Migration tests and schema audit. |
| REQ-DATA-002 | Semantic embeddings shall supplement, never replace, relational identifiers and verified metadata. | ML Platform Lead | Queries return IDs from validated candidates. |
| REQ-DATA-003 | Production writes shall use service identities and least-privilege roles; direct human DML shall be restricted and audited. | Database Administrator | Role matrix and audit test. |
| REQ-DATA-004 | All migrations shall be forward-reviewed, reversible or compensating, tested on representative data, and deployed through CI/CD. | Data Platform Lead | Migration evidence package. |
| REQ-DATA-005 | Rafa shall support point-in-time recovery and quarterly restore verification. | Infrastructure Lead | Restore report with measured RPO/RTO. |

## **4.6 Orchestration and queueing**

Orchestration translates approved intent into durable, observable, recoverable execution without hiding logic inside individual machines.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-ORCH-001 | Kestra workflows shall be decomposed into independently testable subflows with typed inputs, explicit outputs, retry policy, timeout, and error path. | Orchestration Engineer | Workflow lint and integration tests. |
| REQ-ORCH-002 | Every execution shall use idempotency keys and shall not duplicate chargeable work after retries or callback replays. | Platform Architect | Replay and duplicate-delivery tests. |
| REQ-ORCH-003 | Work shall be leased to workers with expiration and heartbeat; abandoned leases shall return to a recoverable state. | Rendering Platform Lead | Worker termination test recovers job. |
| REQ-ORCH-004 | Concurrency shall be controlled by resource class, provider quota, project budget, priority, and operational health. | SRE Lead | Load test and queue policy report. |
| REQ-ORCH-005 | Workflow completion shall require artifact validation and evidence persistence, not only a successful HTTP status. | Quality Engineering Lead | False-success tests are rejected. |

## **4.7 Unreal Engine structural rendering**

The structural render establishes reproducible geometry, timing, camera, motion, and control passes. It is treated as a production service, not a manually operated desktop session.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-UE-001 | An authenticated edge gateway shall validate manifests, translate paths, control Unreal, supervise the process, and report normalized status. | Rendering Platform Lead | End-to-end test without direct public UE access. |
| REQ-UE-002 | UE templates shall be versioned with project files, plugins, presets, maps, render configurations, and content dependencies. | Unreal Technical Director | Reproducible checkout-to-render test. |
| REQ-UE-003 | Each structural render shall emit the required beauty/control passes, frame manifest, checksums, engine version, project revision, render configuration, and logs. | Rendering Engineer | Artifact package schema passes. |
| REQ-UE-004 | A render shall fail when required assets, plugins, map layers, camera bindings, or output destinations are unavailable. | QA Engineer | Fault-injection suite. |
| REQ-UE-005 | The edge worker shall validate output frame count, resolution, naming, storage persistence, and checksum before marking completion. | Rendering Platform Lead | Artifact verifier report. |

## **4.8 AI polish and provider adapters**

Generative services are replaceable capabilities. The company owns the contract, lineage, approval process, and quality gate even when a vendor produces pixels.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-AI-001 | Provider integrations shall implement a normalized submit, status, cancel, result, cost, and error contract. | AI Media Integration Lead | Adapter contract suite. |
| REQ-AI-002 | Provider credentials, model IDs, versions, parameters, quotas, and pricing assumptions shall be configuration, not hard-coded workflow logic. | Platform Engineer | Configuration review and secret scan. |
| REQ-AI-003 | Long-running work shall use asynchronous job IDs and durable polling or callbacks with bounded intervals and terminal-state handling. | Workflow Automation Lead | Restart-during-poll test. |
| REQ-AI-004 | The system shall preserve structural constraints through configured control strength, validation, and human review; provider output shall never be presumed conformant. | Creative Technology Lead | Comparison and quality review evidence. |
| REQ-AI-005 | Every provider run shall record input hashes, model/version, parameters, seed if available, timestamps, status history, output hashes, and attributable cost. | FinOps Lead | Cost and lineage reconciliation. |

## **4.9 Audio, lip-sync, editorial, and delivery**

Post-production turns accepted shots into a conformant release package. Editorial state must remain linked to the underlying approved shot versions.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-POST-001 | Dialogue work shall preserve script version, performer/voice rights, pronunciation, timing, source audio, processed audio, and final sync lineage. | Audio Lead | Audio cue and rights package. |
| REQ-POST-002 | Lip-sync shall occur at the approved stage defined by the shot pipeline and shall be validated for timing and visual quality. | Post-Production Lead | Sync QC result and decision. |
| REQ-POST-003 | Timeline assembly shall derive clip order, source path, duration, frame rate, handles, and transitions from authoritative records. | Editorial Systems Engineer | EDL/XML round-trip test. |
| REQ-POST-004 | Technical QC shall validate codec, container, resolution, frame rate, duration, audio channels, loudness, captions, corruption, and checksums. | Media QC Lead | Machine-readable QC report. |
| REQ-POST-005 | Release shall require creative approval, technical pass, rights clearance, delivery specification conformance, and delivery receipt. | Release Manager | Signed release package. |

## **4.10 Learning, evaluation, and model development**

The data flywheel is valuable only when provenance, consent, quality, and evaluation discipline are stronger than the temptation to collect everything.

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| REQ-ML-001 | Only explicitly eligible, licensed, consented, sanitized, and quality-approved traces may enter evaluation or training datasets. | Data/ML Steward | Dataset eligibility query and signed review. |
| REQ-ML-002 | Dataset records shall include provenance, intended use, exclusions, quality score, redaction status, and split assignment. | ML Data Engineer | Dataset card and sample audit. |
| REQ-ML-003 | Evaluation sets shall be versioned, protected from training leakage, and tied to business and production failure modes. | ML Evaluation Lead | Evaluation registry and leakage check. |
| REQ-ML-004 | Local model migration shall require benchmark parity, quality acceptance, safety review, operational support, rollback, and measured cost benefit. | AI Platform Lead | Migration gate report. |
| REQ-ML-005 | Training and inference experiments shall capture configuration, code revision, data version, hardware, metrics, artifacts, and approver. | ML Operations Lead | Reproducible experiment record. |

# **5\. Non-functional requirements**

## **5.1 Reliability and recoverability**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-REL-001 | No single worker failure shall lose an approved manifest or completed artifact. | SRE Lead | Worker kill test and artifact audit. |
| NFR-REL-002 | Critical state changes shall be transactionally persisted before external side effects are acknowledged. | Platform Architect | Failure-injection test. |
| NFR-REL-003 | Production services shall define SLOs, error budgets, escalation, and degraded modes before launch. | SRE Lead | Signed service readiness review. |
| NFR-REL-004 | Every critical datastore and artifact tier shall have documented RPO/RTO and tested restoration. | Infrastructure Lead | Restore drill report. |

## **5.2 Performance and capacity**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-PERF-001 | Queue wait, render time, provider latency, transfer time, and QC time shall be measured separately. | Observability Engineer | Latency dashboard with percentile views. |
| NFR-PERF-002 | Capacity models shall include GPU memory, storage throughput, network throughput, provider quota, and human review capacity. | Capacity Manager | Monthly capacity forecast. |
| NFR-PERF-003 | The system shall support priority classes and budget-aware throttling. | Platform Lead | Policy and load-test evidence. |
| NFR-PERF-004 | Performance changes shall be benchmarked on representative fixtures before production promotion. | Performance Engineer | Benchmark report. |

## **5.3 Security and privacy**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-SEC-001 | All human and service access shall use unique identities, least privilege, strong authentication, and auditable authorization. | Security Lead | Quarterly access review. |
| NFR-SEC-002 | Secrets shall be held in approved secret stores and never in workflows, source code, documents, or logs. | Security Engineer | Automated secret scan. |
| NFR-SEC-003 | Network boundaries shall deny direct public access to databases, NAS, worker administration, and UE control. | Network Engineer | External attack-surface scan. |
| NFR-SEC-004 | Sensitive media and personal data shall be encrypted in transit and at rest and governed by retention and deletion. | Privacy Lead | Control test and deletion drill. |

## **5.4 Maintainability and portability**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-MNT-001 | Business rules shall live in versioned contracts, policies, or services rather than undocumented node expressions. | Software Architect | Architecture review. |
| NFR-MNT-002 | All production repositories shall include ownership, setup, tests, runbook, release, rollback, and support information. | Engineering Manager | Repository readiness audit. |
| NFR-MNT-003 | Provider and engine upgrades shall be isolated behind compatibility tests and staged rollout. | Release Engineering Lead | Upgrade rehearsal report. |
| NFR-MNT-004 | Schemas and APIs shall have compatibility policy, deprecation windows, and consumer tests. | API Governance Lead | Contract registry. |

## **5.5 Observability and financial control**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-OBS-001 | Every execution shall carry correlation IDs from intake through delivery. | Observability Engineer | Trace continuity test. |
| NFR-OBS-002 | Logs shall be structured, redacted, time-synchronized, retained by class, and searchable by project/job. | SRE Lead | Log audit. |
| NFR-OBS-003 | Provider, cloud, storage, and labor costs shall be attributable to project and production stage. | FinOps Lead | Monthly cost reconciliation. |
| NFR-OBS-004 | Dashboards shall expose quality, throughput, latency, cost, reliability, queue, and capacity trends. | Operations Analytics Lead | Executive and operational dashboards. |

## **5.6 Accessibility and usability**

| ID | Requirement | Owner | Evidence / acceptance |
| :---- | :---- | :---- | :---- |
| NFR-UX-001 | Human approval experiences shall present version, context, consequences, cost, and clear approve/reject/revise actions. | Product Designer | Usability test. |
| NFR-UX-002 | Operational interfaces shall support keyboard access, readable contrast, clear error recovery, and non-color-only status. | Accessibility Owner | Accessibility review. |
| NFR-UX-003 | Delivery workflows shall support required captions, transcripts, audio description, and accessible documentation where contracted. | Post-Production Lead | Delivery accessibility checklist. |

# **6\. Data, security, compliance, and intellectual-property controls**

## **6.1 Data classification**

| Class | Examples | Minimum controls | Default retention |
| :---- | :---- | :---- | :---- |
| Public | Published marketing material, public documentation. | Integrity review; approved release. | Business-defined. |
| Internal | Process documentation, non-sensitive schedules, synthetic fixtures. | Authenticated access; company sharing only. | Life of relevance \+ archive review. |
| Confidential | Client briefs, source media, unreleased content, pricing, contracts. | Least privilege, encryption, audit logs, restricted sharing. | Contract or records schedule. |
| Restricted personal | Actor scans, biometric/identity coefficients, HR records, IDs. | Need-to-know, explicit consent/legal basis, encryption, access review, deletion procedure. | Minimum necessary; consent and legal schedule. |
| Secrets | API keys, private keys, recovery codes, admin credentials. | Secret manager only; rotation; no document or log storage. | Until rotation/revocation. |

## **6.2 Required governance controls**

* Asset ingestion shall capture provenance and rights before the asset becomes discoverable for production.  
* Identity and biometric-derived assets require explicit role-based access and a documented purpose limitation.  
* Every external provider transfer must be covered by an approved vendor record, data handling review, and project-specific eligibility check.  
* Logs and datasets must redact secrets, personal identifiers, and client-confidential text not required for the defined purpose.  
* Legal hold overrides routine deletion; the hold reason and releasing authority must be recorded.  
* Offboarding must revoke access, rotate shared credentials, transfer ownership, and confirm return or deletion of governed assets.  
* Delivery packages must include rights and attribution obligations when applicable; distribution outside approved scope is blocked.

## **6.3 Threat model priorities**

| Threat | Preventive controls | Detection | Response owner |
| :---- | :---- | :---- | :---- |
| Unauthorized remote render control | Private network, authenticated gateway, allowlisted commands, signed jobs. | Gateway auth failures, network alerts. | Security \+ Rendering Platform |
| Malicious or malformed manifest | JSON Schema, size limits, enum validation, path normalization, policy engine. | Validation error metrics and audit events. | Application Security |
| Ransomware or destructive NAS access | Separate identities, snapshots, immutable backup, segmented network, no workstation admin write. | Snapshot anomaly and endpoint alerts. | Infrastructure |
| Prompt or file exfiltration through provider | Eligibility policy, redaction, vendor review, least data transfer. | Provider transfer audit and DLP rules. | Privacy/Legal |
| Supply-chain compromise | Pinned dependencies, signed releases, SBOM, scans, controlled plugins. | Vulnerability and integrity monitoring. | Security Engineering |
| Cost abuse or runaway loops | Budgets, quotas, idempotency, maximum attempts, circuit breakers. | Spend anomaly and execution-rate alerts. | FinOps \+ Platform |
| Training data contamination | Eligibility filters, provenance, evaluation separation, human review. | Dataset audit and leakage tests. | ML Governance |

# **7\. Organization, hiring sequence, and decision rights**

## **7.1 Initial functional organization**

| Function | Accountability | Initial leader |
| :---- | :---- | :---- |
| Executive/Founder | Mission, capital allocation, risk appetite, final executive decisions. | CEO/Founder |
| Studio & Product | Creative roadmap, product requirements, project portfolio, client value. | Head of Studio / CPO |
| Platform Engineering | Rafa, APIs, orchestration, edge gateway, CI/CD, observability. | VP/Head of Engineering |
| Rendering & Virtual Production | UE templates, camera, environment, character, render reliability. | Unreal Technical Director |
| AI Media & ML | Provider adapters, prompt/schema systems, evaluation, local inference, dataset governance. | Head of AI/ML |
| Post-Production | Audio, lip-sync, editorial, conformance, QC, delivery. | Post-Production Lead |
| Operations & Security | IT, facilities, identity, network, storage, support, continuity, security. | COO / Head of Operations |
| People & Finance | Recruiting, onboarding, performance, payroll, budgeting, procurement. | People Operations / Finance Lead |
| Legal & Rights | Contracts, IP, consent, privacy, vendor terms, distribution rights. | Legal Counsel / Rights Manager |

## **7.2 Hiring waves**

| Wave | Trigger | Priority hires | Exit capability |
| :---- | :---- | :---- | :---- |
| Wave 0: Founding design | Before vendor commitments and production build. | Fractional CTO/architect, technical product manager, production systems consultant, legal/privacy counsel. | Approved architecture, requirements, plan, budget, contracts. |
| Wave 1: Reliable single-shot | Funding for first implementation tranche. | Senior backend/data engineer, DevOps/SRE, Unreal TD, workflow automation engineer, QA automation. | One governed shot runs end to end with recovery and evidence. |
| Wave 2: Pilot production | Single-shot gate passes and pilot slate is approved. | Producer, asset pipeline engineer, AI media integration engineer, post-production lead, IT/operations generalist. | Small batch production with approvals, cost, and QC. |
| Wave 3: Repeatable studio | Pilot economics and quality meet thresholds. | Engineering manager, data steward, security engineer, production coordinator, media QC, finance/people ops. | Multiple projects with predictable delivery and on-call. |
| Wave 4: Scale and productization | Demand, margin, and reliability justify specialized teams. | Product manager, ML evaluation lead, customer success/sales, additional rendering/post specialists. | Standard offers, capacity planning, partner ecosystem. |
| Wave 5: Local intelligence and R\&D | Dataset and benchmark gates justify model ownership. | ML research engineer, MLOps engineer, research producer, data operations team. | Controlled local inference/training and proprietary evaluation advantage. |

## **7.3 Decision-rights framework**

| Decision | Recommends | Approves | Consulted | Informed |
| :---- | :---- | :---- | :---- | :---- |
| Architecture standard | Chief/Lead Architect | Head of Engineering | Security, Ops, Studio | All engineering |
| Project authorization | Producer/Product | Executive Sponsor | Finance, Legal, Engineering | Delivery teams |
| Creative gate | Creative Director | Executive Producer/Director | Technical leads, Rights | Production team |
| Production release | Release Manager | Producer \+ Rights owner | QC, Security, Post | Client/Stakeholders |
| Provider onboarding | AI Platform \+ Procurement | Security/Legal \+ Budget owner | Privacy, Engineering | Operations |
| Production incident severity | Incident Commander | On-call policy authority | Service owners | Leadership by severity |
| Hiring decision | Hiring panel | Hiring manager \+ budget owner | People Ops | Interview panel |
| Roadmap/funding gate | PMO/Executive team | CEO/Board as applicable | Function leads | Company |

# **8\. Program roadmap and stage gates**

## **8.1 Eighteen-month implementation roadmap**

| Phase | Timing | Primary scope | Promotion gate |
| :---- | :---- | :---- | :---- |
| Phase 0 \- Company and architecture foundation | Weeks 1-6 | Charters, requirements, architecture decisions, repositories, environments, security baseline, vendor review, staffing. | Gate G0: executive authorization and risk acceptance. |
| Phase 1 \- Atomic render and storage proof | Weeks 4-10 | UE template, private edge gateway, one camera/character/environment, structural render to NAS, artifact verifier. | Gate G1: repeatable manual-trigger render, 20 consecutive passes. |
| Phase 2 \- Rafa data backbone | Weeks 6-14 | Core schema, migrations, audit, asset/rights registry, manifest schema, test fixtures, backups. | Gate G2: authoritative row-to-artifact lineage and restore test. |
| Phase 3 \- Durable orchestration | Weeks 10-20 | Kestra subflows, job lease, retries, failure recovery, correlation, telemetry, dashboards. | Gate G3: fault-injection suite and idempotent replay. |
| Phase 4 \- Human director workflow | Weeks 14-26 | n8n intake, approval gates, immutable versions, notifications, budget warnings. | Gate G4: approved brief-to-structural render with full audit. |
| Phase 5 \- Provider-neutral polish | Weeks 20-32 | Adapter contract, one approved provider, async lifecycle, cost capture, result validation, selection. | Gate G5: provider substitution test and bounded cost. |
| Phase 6 \- Audio/editorial/release | Weeks 26-40 | Audio rights, TTS/recording, lip-sync, timeline export, automated QC, release package. | Gate G6: end-to-end pilot delivery. |
| Phase 7 \- Pilot slate | Months 10-14 | Small production slate, scheduling, support, quality review, cost model, customer/creator feedback. | Gate G7: quality, reliability, margin, and cycle-time targets. |
| Phase 8 \- Scale readiness | Months 14-18 | Multi-project capacity, security hardening, DR, workforce, standard offers, operating cadences. | Gate G8: production service review and scale funding. |

## **8.2 Gate acceptance matrix**

| Gate | Required evidence | Approver | Restriction until pass |
| :---- | :---- | :---- | :---- |
| G0 | Approved requirements, architecture, budget, risk register, role ownership, vendor due diligence plan. | CEO/Founder \+ Head of Engineering | No implementation spend beyond discovery. |
| G1 | 20 consecutive renders from the same manifest; deterministic naming; valid checksums; no manual UE manipulation after job start. | Rendering Platform Lead | No orchestration complexity added. |
| G2 | Schema migrations pass; one project-to-master lineage query; access roles; backup and restore evidence. | Data Platform Lead | No production data accepted. |
| G3 | Retries do not duplicate work; worker death recovers; timeouts and alerts work; dashboards reconcile. | SRE Lead | Concurrency capped at one. |
| G4 | Approvals are version-specific, authenticated, durable, and auditable; rejected versions cannot run. | Product \+ Security | No unsupervised production. |
| G5 | Submit/poll/cancel/result contract passes; cost captured; provider failure recovered; result technically validated. | AI Platform Lead | Single provider quota and budget caps. |
| G6 | Master conforms to delivery spec; creative, technical, rights, and accessibility sign-offs attached. | Release Manager | Pilot-only distribution. |
| G7 | Pilot meets agreed quality, on-time delivery, cost, support, and customer value thresholds. | Executive Sponsor | No broad sales or headcount scaling. |
| G8 | Capacity plan, SLOs, on-call, DR, hiring, unit economics, and annual roadmap approved. | Executive Team | Scale funding remains gated. |

# **9\. Requirement implementation packages**

The following packages are sized for assignment to teams. Each package must be decomposed into repository issues with estimated effort, named owner, dependencies, risks, test plan, and evidence destination. The package ID remains the traceability parent.

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| PKG-001 | Program and governance setup | Technical Program Manager | None | Charter, RAID log, decision log, requirements baseline, gate calendar, reporting cadence. |
| PKG-002 | Identity, access, and secret baseline | Security Engineer | PKG-001 | SSO/MFA, service accounts, role matrix, secret manager, access review, break-glass. |
| PKG-003 | Repository and CI foundation | Platform Engineer | PKG-001/002 | Repositories, branch policy, templates, tests, scans, artifact retention, release tags. |
| PKG-004 | Rafa core database | Data Platform Engineer | PKG-003 | Migrations, entities, state transitions, audit, roles, fixtures, restore. |
| PKG-005 | Manifest and API contracts | Backend/API Engineer | PKG-004 | JSON Schema, OpenAPI, compatibility policy, contract tests, examples. |
| PKG-006 | NAS and artifact management | Infrastructure Engineer | PKG-002 | Storage tiers, shares, service identities, snapshots, backup, checksums, lifecycle. |
| PKG-007 | Edge worker gateway | Rendering Platform Engineer | PKG-005/006 | Authenticated job lease, UE control, process supervision, path translation, verifier. |
| PKG-008 | Unreal template project | Unreal TD | PKG-006/007 | Plugins, maps, camera, character, Remote Control preset, MRQ configs, test scene. |
| PKG-009 | Kestra execution platform | Orchestration Engineer | PKG-004/005/007 | Subflows, retries, leases, triggers, errors, observability, deployment. |
| PKG-010 | n8n director experience | Workflow Automation Engineer | PKG-004/005/009 | Intake, approvals, notifications, edit/regenerate loops, audit persistence. |
| PKG-011 | AI provider gateway | AI Integration Engineer | PKG-005/006/009 | Normalized adapters, async status, costs, result validation, circuit breakers. |
| PKG-012 | Media QC and delivery | Media Systems Engineer | PKG-006/011 | ffprobe/QC, audio, captions, timeline export, delivery package, checksums. |
| PKG-013 | Observability and FinOps | SRE/FinOps Engineer | All platform packages | Correlation, metrics, logs, dashboards, budgets, alerts, cost attribution. |
| PKG-014 | Security hardening and threat validation | Security Lead | All platform packages | Threat model, scans, penetration test, logging, incident runbooks. |
| PKG-015 | Pilot operations | Producer \+ Release Manager | G6 | Pilot slate, scheduling, acceptance, support, retrospectives, economics. |
| PKG-016 | Dataset governance and evaluation | ML Data Lead | PKG-004/011/012 | Eligibility policy, curation jobs, dataset cards, eval sets, access, audit. |

## **9.1 Representative child tasks**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| GOV-001 | Define requirement change workflow | Technical Program Manager | PKG-001 | Approved template, weekly triage, audit trail, baseline version tag. |
| SEC-002 | Create service identity inventory | Security Engineer | PKG-002 | Every service has owner, purpose, privileges, rotation, and disable procedure. |
| CI-003 | Implement pull-request quality gate | Platform Engineer | PKG-003 | Unit, lint, schema, dependency, secret, and container scans block merge on failure. |
| DB-004 | Create project/sequence/scene/shot schema | Data Engineer | PKG-004 | Migration, constraints, fixtures, rollback, integration tests. |
| DB-005 | Implement generic state-transition event | Data Engineer | PKG-004 | Optimistic locking, allowed transitions, actor, reason, evidence, audit tests. |
| API-006 | Publish production-manifest JSON Schema | API Engineer | PKG-005 | Valid and invalid fixture corpus; CI contract test. |
| STO-007 | Create artifact naming and checksum service | Infrastructure Engineer | PKG-006 | Versioned path, SHA-256, content metadata, duplicate detection. |
| EDGE-008 | Implement signed job lease endpoint | Backend Engineer | PKG-007 | Auth, authorization, heartbeat, expiration, replay protection. |
| UE-009 | Create atomic scene and MRQ preset | Unreal TD | PKG-008 | Camera, character, environment, depth/normal/beauty passes, repeatable output. |
| ORCH-010 | Build structural-render subflow | Kestra Engineer | PKG-009 | Typed inputs, lease, trigger, wait, verify, success/failure state, retry. |
| N8N-011 | Build version-specific approval gate | n8n Engineer | PKG-010 | Approve/reject/revise persists exact artifact version and actor. |
| AI-012 | Implement provider adapter contract tests | AI Integration Engineer | PKG-011 | Submit/status/cancel/result/cost/error pass against mock and sandbox. |
| QC-013 | Implement media technical validator | Media Systems Engineer | PKG-012 | Machine-readable report covers delivery specification. |
| OBS-014 | Create end-to-end correlation dashboard | Observability Engineer | PKG-013 | Trace intake \-\> job \-\> render \-\> provider \-\> QC \-\> delivery. |
| DR-015 | Run database and NAS restore drill | Infrastructure Lead | PKG-006/013 | Measured RPO/RTO, gaps, remediation owners. |
| PILOT-016 | Run ten-shot pilot release | Producer | G6 | Ten shots delivered with cost, quality, time, incidents, and retrospective. |

# **10\. Quality governance and definition of done**

## **10.1 Universal definition of done**

* The requirement or ticket is linked to its parent requirement and architecture decision.  
* The implementation is reviewed by an accountable technical owner and, where applicable, security, data, rights, or operations.  
* Automated tests cover success, validation failure, dependency failure, retry/replay, authorization failure, and recovery.  
* Operational telemetry, meaningful errors, and correlation IDs are present.  
* Documentation includes setup, configuration, support ownership, runbook, rollback, and known limitations.  
* No secret, personal data, unlicensed asset, or production-only value appears in source control or test fixtures.  
* Evidence is attached at the defined location and the acceptance criterion is explicitly marked pass/fail.  
* The change has been exercised in the required environment and promoted through the approved release process.  
* Downstream consumers and change impact have been evaluated; compatibility and migration are documented.  
* The service owner accepts ongoing support, SLO, cost, capacity, and lifecycle responsibility.

## **10.2 Quality layers**

| Layer | Primary question | Example evidence | Owner |
| :---- | :---- | :---- | :---- |
| Contract quality | Is the input/output valid and compatible? | Schema tests, API consumer tests. | API/Data Lead |
| Functional quality | Does the feature perform the required behavior? | Unit and integration test report. | Engineering |
| Production quality | Does the media meet creative and technical expectations? | Creative review, QC report. | Studio/Post |
| Operational quality | Can it be observed, recovered, and supported? | Runbook drill, alerts, SLO dashboard. | SRE/Ops |
| Security/privacy quality | Is access, data use, and exposure controlled? | Threat review, scan, access test. | Security/Privacy |
| Business quality | Does the result meet value, schedule, and unit economics? | Portfolio review, cost/margin report. | Executive/Finance |

# **11\. Risk register**

| ID | Risk | Impact | Likelihood | Mitigation | Owner |
| :---- | :---- | :---- | :---- | :---- | :---- |
| R-001 | Architecture grows faster than proven use cases. | High | High | Stage-gated funding; one-shot-first; kill criteria. | Executive Sponsor |
| R-002 | Source notes contain speculative or version-specific implementation claims. | High | Medium | Technical validation spikes; vendor docs; compatibility tests. | Chief Architect |
| R-003 | Insufficient rights for scans, voices, training traces, or source IP. | High | High | Rights registry; counsel review; consent; eligibility policy. | Legal/Rights |
| R-004 | n8n or Kestra becomes a hidden monolith. | Medium | High | Typed contracts, subflows, code review, ownership boundaries. | Platform Architect |
| R-005 | Unreal automation is unreliable in unattended operation. | High | Medium | Edge supervisor, health checks, reproducible templates, fault testing. | Rendering Lead |
| R-006 | Provider APIs change, fail, or become uneconomic. | Medium | High | Adapter contract, multi-provider capability, budget caps, local benchmark plan. | AI Platform Lead |
| R-007 | NAS becomes single point of failure or ransomware blast radius. | Medium | High | Segmentation, snapshots, immutable/offsite backup, restore drills. | Infrastructure Lead |
| R-008 | Costs cannot be attributed to projects. | High | Medium | Correlation and cost event requirements before pilot. | FinOps |
| R-009 | Quality review becomes the throughput bottleneck. | Medium | High | Review standards, sampling, role capacity, structured decisions, automation. | Studio Operations |
| R-010 | Team hires tool operators without system-design capability. | Medium | High | Competency-based hiring, practical tests, architecture onboarding. | Head of Engineering |
| R-011 | Generated trace collection creates privacy or data contamination. | High | Medium | Eligibility pipeline, redaction, separate evaluation set, audits. | ML Governance |
| R-012 | Company scales headcount before repeatable demand and margins. | High | Medium | Hiring waves tied to gates and workload thresholds. | CEO/Finance |

# **12\. Change control and traceability**

## **12.1 Change classes**

| Class | Examples | Minimum approval | Required evidence |
| :---- | :---- | :---- | :---- |
| Editorial | Clarity, spelling, non-operative formatting. | Document owner. | Change log. |
| Requirement | New/changed behavior, acceptance, scope, policy. | Product/Business owner \+ affected technical owner. | Impact analysis and revised traceability. |
| Architecture | System boundary, data ownership, security model, provider strategy. | Architecture authority \+ Security/Ops. | ADR, alternatives, migration/rollback. |
| Production emergency | Immediate mitigation for incident. | Incident Commander under emergency policy. | Incident log, time-bounded exception, retrospective. |
| Policy/Legal | Rights, privacy, employment, retention, distribution. | Authorized legal/leadership owner. | Approved policy and implementation plan. |

## **12.2 Requirement status**

* Draft \- authored but not yet reviewed.  
* Reviewed \- domain owners agree the requirement is understandable and testable.  
* Approved \- authorized for planning and implementation.  
* Implemented \- implementation evidence exists.  
* Verified \- independent acceptance passed.  
* Operational \- service owner accepts support and measurement.  
* Deprecated \- replacement and sunset are approved.  
* Retired \- production use and retained obligations are closed.

## **12.3 Traceability record template**

**Example traceability record**

requirement\_id: REQ-UE-003  
parent\_objective: BO-02\_REPEATABLE\_PRODUCTION  
owner: Rendering Platform Lead  
implementation\_items:  
  \- repo: rafa-render-edge  
    issue: EDGE-143  
  \- repo: rafa-unreal-template  
    issue: UE-087  
tests:  
  \- test\_atomic\_render\_package  
  \- test\_missing\_control\_pass\_fails  
evidence:  
  \- staging://evidence/G1/atomic-render-20-run-report.json  
  \- staging://evidence/G1/artifact-checksum-audit.csv  
status: VERIFIED  
approved\_by: architecture-review-board

# **13\. Templates**

## **13.1 Requirement template**

ID:  
Title:  
Business outcome:  
Requirement:  
Rationale:  
Scope:  
Out of scope:  
Owner:  
Dependencies:  
Security/privacy/rights considerations:  
Acceptance criteria:  
Negative tests:  
Operational evidence:  
Cost/capacity impact:  
Rollback or retirement:

## **13.2 Architecture decision template**

ADR ID and title:  
Status:  
Context:  
Decision:  
Alternatives considered:  
Consequences:  
Security and privacy:  
Data ownership:  
Operational ownership:  
Migration plan:  
Rollback:  
Validation date and evidence:

## **13.3 Promotion gate packet**

Gate:  
Entry criteria:  
Requirements verified:  
Test summary:  
Security/privacy/rights review:  
Operational readiness:  
Cost and capacity:  
Known risks and exceptions:  
Rollback:  
Approvers and decisions:  
Evidence index:  
Decision: PASS | CONDITIONAL | FAIL

## **13.4 Employment task statement**

Task ID and title:  
Business reason:  
Exact deliverables:  
Repositories/systems:  
Inputs and dependencies:  
Implementation constraints:  
Acceptance tests:  
Evidence to submit:  
Definition of done:  
Support handoff:  
Estimated level and skills:  
Manager:

# **14\. Source-note reconciliation and validation policy**

The Rafa folder contains detailed design notes that establish the intended direction: PostgreSQL/pgvector as the data backbone; n8n for interactive control; Kestra for durable orchestration; Unreal Engine for structural rendering; NAS for artifacts; and generative services for polish. Those notes are treated as design inputs, not automatically as production-ready specifications. Product names, endpoint paths, plugin names, model identifiers, commands, licensing terms, and performance claims must be confirmed during implementation against the approved versions and contracts.

| Source concept | Normalized requirement | Validation action |
| :---- | :---- | :---- |
| One giant Kestra YAML | Use independently versioned subflows with typed contracts. | Prototype and lint on approved Kestra version. |
| n8n as master orchestrator | n8n owns human interaction and external job lifecycle; Kestra owns durable compute execution. | End-to-end responsibility test and failure recovery review. |
| Direct UE Remote Control | Use a private, authenticated edge worker as the control boundary. | Threat model and penetration test. |
| Provider-specific Veo/Seedance endpoints | Use normalized provider adapters and capability configuration. | Sandbox contract test for each approved vendor. |
| Postgres EDL string generation | Timeline data is authoritative; export implementation must pass round-trip conformance in target editor. | Automated test project imported into each supported NLE. |
| FLAME coefficients and MetaHuman mapping | Identity/performance representation is an approved asset contract; engine mapping is a validated adapter. | Research spike, licensing review, deformation and continuity test. |
| Cloud-first then local SGLang | Migration occurs only after benchmark, quality, support, safety, and cost gates. | Reproducible benchmark and rollback plan. |

# **15\. Executive implementation checklist**

* Appoint executive sponsor, head of engineering/architect, technical program manager, producer, and legal/privacy advisor.  
* Approve the architecture responsibility split and prohibit uncontrolled direct access to core production systems.  
* Authorize only Phase 0 and Phase 1 until G1 evidence passes.  
* Require a project cost model and rights plan before any pilot content is accepted.  
* Fund the initial Wave 1 team around data/platform, Unreal, orchestration, infrastructure, and QA rather than a broad creative headcount.  
* Review gate evidence monthly and stop, redirect, or fund the next tranche explicitly.  
* Treat Business Playbook, Operations Manual, and Onboarding Playbook as controlled systems with owners and revision cadence.