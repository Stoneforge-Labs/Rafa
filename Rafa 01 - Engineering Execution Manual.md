# 

# **Rafa Engineering Execution Manual**

## ***Implementation architecture, repository plan, code and configuration references, verification procedures, engineering roles, and assignable delivery backlog***

| Control | Value |
| :---- | :---- |
| Document ID | RAFA-ENG-001 |
| Planning horizon | Eighteen-month platform build with long-term maintainability provisions |
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

# **Engineering mandate**

This manual tells the implementation organization how to build and operate the Rafa production platform. It is deliberately prescriptive at system boundaries and evidence requirements while remaining provider-neutral where vendor syntax or licensing may change. Every code and configuration block is a reference implementation: the assigned engineer must validate it against the approved versions, add tests, and record any divergence through an architecture decision.

**Implementation rule:** Do not begin with AI agents, broad parallelism, or full-length content. First prove a private, repeatable, one-shot structural render from a versioned manifest; then add Rafa state, durable orchestration, human approvals, provider polish, editorial, and scale.

## **Static contents**

* 1\. Engineering operating model and architecture  
* 2\. Repositories, environments, developer workstation, and delivery workflow  
* 3\. Rafa PostgreSQL schema, migrations, roles, views, and state transitions  
* 4\. Production manifest, JSON Schema, OpenAPI, and API contracts  
* 5\. Identity, secrets, network, and storage implementation  
* 6\. Edge render gateway and Unreal Engine automation  
* 7\. Kestra flows and n8n human-director workflows  
* 8\. Provider-neutral AI polish gateway  
* 9\. Audio, editorial, media QC, timeline, and delivery  
* 10\. Observability, FinOps, SRE, backup, and disaster recovery  
* 11\. CI/CD, testing strategy, release, rollback, and support  
* 12\. Engineering organization, hiring specifications, epics, and ticket catalog

# **1\. Engineering operating model and architecture**

## **1.1 Engineering principles**

| Principle | Required behavior |
| :---- | :---- |
| Contract first | Define JSON Schema, OpenAPI, SQL constraints, and artifact contracts before wiring tools together. |
| Source-of-truth discipline | Rafa owns durable business and workflow state; tools consume and report state through controlled interfaces. |
| Idempotent side effects | Every chargeable or destructive external action has an idempotency key and a replay strategy. |
| Private edge control | The gateway authenticates jobs and controls local applications. UE, NAS administration, and databases are never public endpoints. |
| Small services, visible workflows | Use declarative workflows for orchestration and small tested services for validation, security, path translation, and local application control. |
| Immutable production versions | Approved manifests and artifacts are append-only; revisions create new versions and lineage. |
| Evidence before status | A job is complete only after artifact checks and evidence persistence. |
| Observability in the design | Correlation, metrics, logs, cost, and quality events are part of each feature, not a later instrumentation project. |
| Version pinning and compatibility | Pin runtime dependencies and prove upgrades through staging fixtures. |
| Operational ownership | Every service, database, workflow, share, queue, and provider has an owner, SLO, runbook, and retirement plan. |

## **1.2 Logical topology**

Internet / Approved Enterprise Inputs  
                 |  
         \[WAF / Identity-Aware Proxy\]  
                 |  
       \+---------+----------+  
       |                    |  
 \[n8n interaction\]    \[Approved APIs\]  
       |                    |  
       \+---------+----------+  
                 |  
          \[Rafa API layer\]  
                 |  
      \+----------+-----------+  
      |                      |  
 \[PostgreSQL\]          \[Kestra cluster\]  
 source of truth       durable execution  
      |                      |  
      \+-----------+----------+  
                  |  
          \[Private overlay/VPN\]  
                  |  
       \[Edge Render Gateway API\]  
        auth / lease / supervisor  
                  |  
        \+---------+---------+  
        |                   |  
 \[Unreal render node\]   \[NAS artifact services\]  
        |                   |  
        \+---------+---------+  
                  |  
       \[Provider Gateway / n8n\]  
        async external lifecycle  
                  |  
 \[Post, QC, timeline, delivery, archive\]

## **1.3 Sequence: one shot from approval to master**

| \# | Component | Action | Key outputs |
| :---- | :---- | :---- | :---- |
| 1 | n8n | Accepts approved shot plan version and calls Rafa API to create a render request. | shot\_version\_id, idempotency\_key |
| 2 | Rafa API | Validates state, rights, assets, budget, and manifest; creates queued job in one transaction. | job\_id, manifest\_version |
| 3 | Kestra | Receives event/webhook, acquires job, invokes render subflow, writes correlation. | execution\_id |
| 4 | Edge gateway | Authenticates Kestra service identity, leases job, downloads/reads manifest, translates storage paths. | lease\_token, local\_work\_dir |
| 5 | Unreal | Loads approved project revision, applies manifest, renders configured passes. | frame sequence, logs |
| 6 | Edge gateway | Checks frame count, resolution, passes, checksums, and NAS persistence; reports result. | structural\_package\_id |
| 7 | Rafa/Kestra | Transitions to structural-ready and emits provider-polish request if approved. | polish\_job\_id |
| 8 | n8n/provider gateway | Submits asynchronous job, polls or receives callback, records cost/status, downloads result. | provider\_job\_id, polished\_asset\_id |
| 9 | QC/post | Runs technical QC, human review, audio/lip-sync/editorial, and final conformance. | qc\_report, master\_asset\_id |
| 10 | Release | Records approvals, delivery package, receipt, archive, and dataset eligibility decision. | release\_id, archive\_manifest |

# **2\. Repositories, environments, and developer workflow**

## **2.1 Repository map**

| Repository | Scope | Code owner |
| :---- | :---- | :---- |
| rafa-platform | Rafa API, domain services, DB access, state machine, audit. | Backend Lead |
| rafa-db | PostgreSQL migrations, roles, seed fixtures, views, restore tests. | Data Platform Lead |
| rafa-contracts | JSON Schema, OpenAPI, event schemas, examples, compatibility tests. | API Governance Lead |
| rafa-kestra | Kestra namespaces, flows, templates, plugin inventory, deployment. | Orchestration Lead |
| rafa-n8n | Exported n8n workflows, credentials map, node documentation, tests. | Automation Lead |
| rafa-render-edge | Authenticated edge API, worker, supervisor, artifact verifier, path map. | Rendering Platform Lead |
| rafa-unreal-template | UE project, plugins, maps, presets, Python/Blueprint bridge, test scene. | Unreal TD |
| rafa-provider-gateway | Provider-neutral adapter interface, vendor adapters, polling, costs. | AI Integration Lead |
| rafa-media-tools | ffprobe/ffmpeg QC, audio checks, captions, timeline exporters. | Media Systems Lead |
| rafa-observability | Dashboards, alerts, log pipelines, SLOs, synthetic tests. | SRE Lead |
| rafa-infrastructure | IaC, network, secrets, storage, backup, environments. | Infrastructure Lead |
| rafa-evals | Evaluation fixtures, scoring, dataset policy, model benchmarks. | ML Evaluation Lead |
| rafa-docs | Architecture decisions, runbooks, service catalog, procedures. | Technical Program Manager |

## **2.2 Standard repository structure**

.  
├── README.md  
├── CODEOWNERS  
├── LICENSE-or-INTERNAL-NOTICE  
├── SECURITY.md  
├── CONTRIBUTING.md  
├── docs/  
│   ├── architecture.md  
│   ├── runbook.md  
│   ├── release.md  
│   ├── rollback.md  
│   └── decisions/  
├── src/  
├── tests/  
│   ├── unit/  
│   ├── contract/  
│   ├── integration/  
│   └── fixtures/  
├── deploy/  
│   ├── dev/  
│   ├── staging/  
│   └── prod/  
├── scripts/  
├── .github/workflows/  
├── pyproject.toml | package.json | build.gradle  
├── Dockerfile  
├── docker-compose.dev.yml  
└── renovate.json

## **2.3 Branch and release policy**

* Default branch is protected. Changes require pull request, passing checks, code-owner approval, and resolved review comments.  
* Feature branches are short-lived. Rebase or merge according to the approved repository standard; do not maintain unofficial long-running forks.  
* Every production deployment references an immutable commit and release tag. Container images use digest pinning in production.  
* Schema and API changes use compatibility review. Breaking changes require a migration plan, deprecation window, consumer inventory, and rollback.  
* Emergency changes follow the incident policy, require a time-bounded exception, and receive retrospective review within two business days.

## **2.4 Local development composition**

**Reference docker-compose.dev.yml; pin approved image digests before production use**

services:  
  postgres:  
    image: pgvector/pgvector:pg16  
    environment:  
      POSTGRES\_DB: rafa  
      POSTGRES\_USER: rafa\_dev  
      POSTGRES\_PASSWORD\_FILE: /run/secrets/postgres\_password  
    ports: \["127.0.0.1:5432:5432"\]  
    volumes:  
      \- rafa\_pg:/var/lib/postgresql/data  
      \- ./db/init:/docker-entrypoint-initdb.d:ro  
    healthcheck:  
      test: \["CMD-SHELL", "pg\_isready \-U rafa\_dev \-d rafa"\]  
      interval: 5s  
      timeout: 3s  
      retries: 20  
    secrets: \[postgres\_password\]

  rafa-api:  
    build: ./services/rafa-api  
    environment:  
      DATABASE\_URL\_FILE: /run/secrets/database\_url  
      LOG\_LEVEL: INFO  
    ports: \["127.0.0.1:8081:8080"\]  
    depends\_on:  
      postgres: {condition: service\_healthy}  
    secrets: \[database\_url\]

  mock-provider:  
    build: ./test/mock-provider  
    ports: \["127.0.0.1:8090:8090"\]

  minio:  
    image: minio/minio  
    command: server /data \--console-address ":9001"  
    ports: \["127.0.0.1:9000:9000", "127.0.0.1:9001:9001"\]

volumes:  
  rafa\_pg:

secrets:  
  postgres\_password:  
    file: ./.secrets/postgres\_password  
  database\_url:  
    file: ./.secrets/database\_url

## **2.5 Developer setup acceptance**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| DEV-001 | Automate repository bootstrap and prerequisite checks. | Developer Experience Engineer | Repository created | \`make doctor\` reports versions, missing tools, and safe remediation. |
| DEV-002 | Create synthetic media and manifest fixtures. | QA Engineer | Contracts baseline | Fixtures are license-safe, deterministic, small, and cover valid/invalid cases. |
| DEV-003 | Implement one-command local test stack. | Platform Engineer | Compose/IaC | \`make dev-up\`, health check, tests, and \`make dev-down\` documented. |
| DEV-004 | Implement pre-commit and CI parity. | Developer Experience Engineer | CI policy | Local checks match blocking CI checks and complete within target time. |
| DEV-005 | Create environment configuration inventory. | Release Engineer | Service catalog | Every variable has owner, type, source, secret classification, default, and environment scope. |

# **3\. Rafa PostgreSQL implementation**

## **3.1 Data modeling rules**

* Use UUID primary keys for externally referenced entities and bigint identity keys only for high-volume internal event tables where justified.  
* Store all timestamps as timestamptz in UTC. Display conversion belongs in clients.  
* Use explicit status enums or constrained lookup tables with a controlled transition function; do not accept arbitrary status text.  
* Use JSONB for provider extension data and evolving payload snapshots, not as a substitute for indexed relational fields.  
* Store artifact bytes outside PostgreSQL. Store immutable URI, checksum, byte size, media metadata, storage class, and retention.  
* Audit every business-significant change using append-only events. Application tables may reflect current state; audit preserves the history.  
* Embedding dimensions and model IDs are versioned. Never mix embeddings from incompatible models in the same index without explicit partitioning.  
* Migrations are sequential, repeatable in clean environments, and tested forward and backward or with a compensating rollback.

## **3.2 Extensions and schemas**

CREATE EXTENSION IF NOT EXISTS pgcrypto;  
CREATE EXTENSION IF NOT EXISTS vector;  
CREATE EXTENSION IF NOT EXISTS citext;

CREATE SCHEMA IF NOT EXISTS core;  
CREATE SCHEMA IF NOT EXISTS production;  
CREATE SCHEMA IF NOT EXISTS assets;  
CREATE SCHEMA IF NOT EXISTS workflow;  
CREATE SCHEMA IF NOT EXISTS rights;  
CREATE SCHEMA IF NOT EXISTS finance;  
CREATE SCHEMA IF NOT EXISTS audit;  
CREATE SCHEMA IF NOT EXISTS ml;

## **3.3 Core schema reference**

**Core production schema; refine namespaces and constraints in migration review**

CREATE TYPE core.project\_state AS ENUM (  
  'DRAFT','AUTHORIZED','ACTIVE','PAUSED','CANCELLED','COMPLETED','ARCHIVED'  
);

CREATE TYPE production.approval\_decision AS ENUM (  
  'APPROVED','REJECTED','REVISION\_REQUIRED','EXPIRED'  
);

CREATE TYPE workflow.job\_state AS ENUM (  
  'QUEUED','LEASED','RUNNING','WAITING\_EXTERNAL',  
  'SUCCEEDED','FAILED\_RETRYABLE','FAILED\_TERMINAL','CANCELLED'  
);

CREATE TABLE core.projects (  
  project\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  project\_code text NOT NULL UNIQUE,  
  name text NOT NULL,  
  sponsor\_subject text NOT NULL,  
  producer\_subject text NOT NULL,  
  state core.project\_state NOT NULL DEFAULT 'DRAFT',  
  budget\_currency char(3) NOT NULL DEFAULT 'USD',  
  budget\_amount numeric(14,2),  
  risk\_class text NOT NULL CHECK (risk\_class IN ('LOW','MEDIUM','HIGH','RESTRICTED')),  
  rights\_review\_required boolean NOT NULL DEFAULT true,  
  state\_version bigint NOT NULL DEFAULT 0,  
  created\_at timestamptz NOT NULL DEFAULT now(),  
  updated\_at timestamptz NOT NULL DEFAULT now()  
);

CREATE TABLE production.sequences (  
  sequence\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  project\_id uuid NOT NULL REFERENCES core.projects(project\_id),  
  sequence\_code text NOT NULL,  
  name text NOT NULL,  
  frame\_rate\_num integer NOT NULL DEFAULT 24 CHECK (frame\_rate\_num \> 0),  
  frame\_rate\_den integer NOT NULL DEFAULT 1 CHECK (frame\_rate\_den \> 0),  
  UNIQUE(project\_id, sequence\_code)  
);

CREATE TABLE production.scenes (  
  scene\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  sequence\_id uuid NOT NULL REFERENCES production.sequences(sequence\_id),  
  scene\_code text NOT NULL,  
  sort\_order integer NOT NULL,  
  synopsis text,  
  UNIQUE(sequence\_id, scene\_code),  
  UNIQUE(sequence\_id, sort\_order)  
);

CREATE TABLE production.shots (  
  shot\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  scene\_id uuid NOT NULL REFERENCES production.scenes(scene\_id),  
  shot\_code text NOT NULL,  
  sort\_order integer NOT NULL,  
  duration\_frames integer NOT NULL CHECK (duration\_frames \> 0),  
  current\_version integer NOT NULL DEFAULT 1 CHECK (current\_version \> 0),  
  created\_at timestamptz NOT NULL DEFAULT now(),  
  UNIQUE(scene\_id, shot\_code),  
  UNIQUE(scene\_id, sort\_order)  
);

CREATE TABLE production.shot\_versions (  
  shot\_version\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  shot\_id uuid NOT NULL REFERENCES production.shots(shot\_id),  
  version integer NOT NULL CHECK (version \> 0),  
  status text NOT NULL CHECK (status IN (  
    'DRAFT','IN\_REVIEW','APPROVED','SUPERSEDED','WITHDRAWN'  
  )),  
  script\_excerpt text,  
  director\_notes text,  
  manifest jsonb NOT NULL,  
  manifest\_schema\_version text NOT NULL,  
  manifest\_sha256 char(64) NOT NULL,  
  created\_by\_subject text NOT NULL,  
  created\_at timestamptz NOT NULL DEFAULT now(),  
  approved\_at timestamptz,  
  UNIQUE(shot\_id, version),  
  UNIQUE(manifest\_sha256)  
);

## **3.4 Asset, artifact, rights, and lineage schema**

CREATE TYPE assets.asset\_kind AS ENUM (  
  'SOURCE\_IMAGE','SOURCE\_VIDEO','SOURCE\_AUDIO','MESH','TEXTURE','RIG',  
  'IDENTITY\_MODEL','PERFORMANCE\_CAPTURE','SCRIPT','MANIFEST',  
  'RENDER\_PASS','STRUCTURAL\_VIDEO','POLISHED\_VIDEO','MASTER\_VIDEO',  
  'CAPTION','TIMELINE','REPORT','LOG','DATASET'  
);

CREATE TABLE assets.assets (  
  asset\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  asset\_kind assets.asset\_kind NOT NULL,  
  display\_name text NOT NULL,  
  current\_version integer NOT NULL DEFAULT 1,  
  classification text NOT NULL CHECK (  
    classification IN ('PUBLIC','INTERNAL','CONFIDENTIAL','RESTRICTED\_PERSONAL')  
  ),  
  owner\_subject text NOT NULL,  
  created\_at timestamptz NOT NULL DEFAULT now()  
);

CREATE TABLE assets.asset\_versions (  
  asset\_version\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  asset\_id uuid NOT NULL REFERENCES assets.assets(asset\_id),  
  version integer NOT NULL,  
  storage\_uri text NOT NULL,  
  sha256 char(64) NOT NULL,  
  byte\_size bigint NOT NULL CHECK (byte\_size \>= 0),  
  mime\_type text NOT NULL,  
  media\_metadata jsonb NOT NULL DEFAULT '{}'::jsonb,  
  source\_system text,  
  created\_by\_subject text NOT NULL,  
  created\_at timestamptz NOT NULL DEFAULT now(),  
  UNIQUE(asset\_id, version),  
  UNIQUE(storage\_uri),  
  UNIQUE(sha256, byte\_size)  
);

CREATE TABLE rights.rights\_records (  
  rights\_record\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  asset\_id uuid NOT NULL REFERENCES assets.assets(asset\_id),  
  rights\_state text NOT NULL CHECK (  
    rights\_state IN ('PENDING','APPROVED','RESTRICTED','EXPIRED','REJECTED')  
  ),  
  licensor\_or\_subject text,  
  permitted\_uses jsonb NOT NULL DEFAULT '\[\]'::jsonb,  
  prohibited\_uses jsonb NOT NULL DEFAULT '\[\]'::jsonb,  
  territories jsonb NOT NULL DEFAULT '\[\]'::jsonb,  
  effective\_at timestamptz,  
  expires\_at timestamptz,  
  consent\_evidence\_asset\_version\_id uuid REFERENCES assets.asset\_versions(asset\_version\_id),  
  review\_owner\_subject text NOT NULL,  
  reviewed\_at timestamptz  
);

CREATE TABLE assets.lineage\_edges (  
  lineage\_edge\_id bigserial PRIMARY KEY,  
  parent\_asset\_version\_id uuid NOT NULL REFERENCES assets.asset\_versions(asset\_version\_id),  
  child\_asset\_version\_id uuid NOT NULL REFERENCES assets.asset\_versions(asset\_version\_id),  
  transformation\_type text NOT NULL,  
  workflow\_execution\_id text,  
  parameters jsonb NOT NULL DEFAULT '{}'::jsonb,  
  created\_at timestamptz NOT NULL DEFAULT now(),  
  CHECK (parent\_asset\_version\_id \<\> child\_asset\_version\_id),  
  UNIQUE(parent\_asset\_version\_id, child\_asset\_version\_id, transformation\_type)  
);

CREATE INDEX ON assets.lineage\_edges(parent\_asset\_version\_id);  
CREATE INDEX ON assets.lineage\_edges(child\_asset\_version\_id);

## **3.5 Workflow, approval, cost, and audit schema**

CREATE TABLE workflow.jobs (  
  job\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  job\_type text NOT NULL,  
  project\_id uuid NOT NULL REFERENCES core.projects(project\_id),  
  shot\_version\_id uuid REFERENCES production.shot\_versions(shot\_version\_id),  
  state workflow.job\_state NOT NULL DEFAULT 'QUEUED',  
  priority smallint NOT NULL DEFAULT 50 CHECK (priority BETWEEN 0 AND 100),  
  idempotency\_key text NOT NULL UNIQUE,  
  manifest\_asset\_version\_id uuid REFERENCES assets.asset\_versions(asset\_version\_id),  
  requested\_by\_subject text NOT NULL,  
  lease\_owner text,  
  lease\_token\_hash text,  
  lease\_expires\_at timestamptz,  
  attempt integer NOT NULL DEFAULT 0,  
  max\_attempts integer NOT NULL DEFAULT 3,  
  external\_job\_id text,  
  correlation\_id uuid NOT NULL DEFAULT gen\_random\_uuid(),  
  state\_version bigint NOT NULL DEFAULT 0,  
  queued\_at timestamptz NOT NULL DEFAULT now(),  
  started\_at timestamptz,  
  completed\_at timestamptz,  
  last\_error jsonb  
);

CREATE INDEX workflow\_jobs\_queue\_idx  
  ON workflow.jobs(state, priority DESC, queued\_at)  
  WHERE state IN ('QUEUED','FAILED\_RETRYABLE');

CREATE TABLE production.approvals (  
  approval\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  entity\_type text NOT NULL,  
  entity\_id uuid NOT NULL,  
  entity\_version text NOT NULL,  
  decision production.approval\_decision NOT NULL,  
  approver\_subject text NOT NULL,  
  rationale text,  
  requested\_changes jsonb NOT NULL DEFAULT '\[\]'::jsonb,  
  decided\_at timestamptz NOT NULL DEFAULT now(),  
  UNIQUE(entity\_type, entity\_id, entity\_version, approver\_subject, decision)  
);

CREATE TABLE finance.cost\_events (  
  cost\_event\_id bigserial PRIMARY KEY,  
  project\_id uuid NOT NULL REFERENCES core.projects(project\_id),  
  job\_id uuid REFERENCES workflow.jobs(job\_id),  
  provider text NOT NULL,  
  cost\_category text NOT NULL,  
  quantity numeric(18,6) NOT NULL,  
  unit text NOT NULL,  
  unit\_cost numeric(18,8),  
  currency char(3) NOT NULL,  
  amount numeric(14,4) NOT NULL,  
  pricing\_version text,  
  occurred\_at timestamptz NOT NULL,  
  recorded\_at timestamptz NOT NULL DEFAULT now()  
);

CREATE TABLE audit.events (  
  event\_id bigserial PRIMARY KEY,  
  occurred\_at timestamptz NOT NULL DEFAULT now(),  
  actor\_subject text NOT NULL,  
  actor\_type text NOT NULL CHECK (actor\_type IN ('HUMAN','SERVICE','SYSTEM')),  
  action text NOT NULL,  
  entity\_type text NOT NULL,  
  entity\_id text NOT NULL,  
  prior\_state jsonb,  
  new\_state jsonb,  
  reason text,  
  correlation\_id uuid,  
  source\_ip inet,  
  user\_agent text,  
  metadata jsonb NOT NULL DEFAULT '{}'::jsonb  
) PARTITION BY RANGE (occurred\_at);

## **3.6 Camera, environment, character, and semantic indexes**

CREATE TABLE production.camera\_presets (  
  camera\_preset\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  name citext NOT NULL UNIQUE,  
  focal\_length\_mm numeric(8,3) NOT NULL CHECK (focal\_length\_mm \> 0),  
  aperture\_f\_stop numeric(6,3) NOT NULL CHECK (aperture\_f\_stop \> 0),  
  sensor\_width\_mm numeric(8,3),  
  sensor\_height\_mm numeric(8,3),  
  focus\_mode text NOT NULL,  
  sequencer\_template\_asset\_path text,  
  semantic\_description text NOT NULL,  
  embedding\_model text,  
  embedding vector(1536),  
  version integer NOT NULL DEFAULT 1,  
  active boolean NOT NULL DEFAULT true  
);

CREATE TABLE production.environments (  
  environment\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  name citext NOT NULL UNIQUE,  
  coordinate\_system text NOT NULL DEFAULT 'WGS84',  
  latitude numeric(10,7),  
  longitude numeric(10,7),  
  altitude\_m numeric(12,3),  
  datetime\_utc timestamptz,  
  weather jsonb NOT NULL DEFAULT '{}'::jsonb,  
  map\_provider\_extensions jsonb NOT NULL DEFAULT '{}'::jsonb,  
  semantic\_description text NOT NULL,  
  embedding\_model text,  
  embedding vector(1536),  
  version integer NOT NULL DEFAULT 1,  
  active boolean NOT NULL DEFAULT true,  
  CHECK ((latitude IS NULL AND longitude IS NULL)  
      OR (latitude BETWEEN \-90 AND 90 AND longitude BETWEEN \-180 AND 180))  
);

CREATE TABLE production.characters (  
  character\_id uuid PRIMARY KEY DEFAULT gen\_random\_uuid(),  
  name citext NOT NULL UNIQUE,  
  identity\_asset\_version\_id uuid NOT NULL REFERENCES assets.asset\_versions(asset\_version\_id),  
  body\_rig\_asset\_version\_id uuid REFERENCES assets.asset\_versions(asset\_version\_id),  
  approved\_voice\_id text,  
  semantic\_description text NOT NULL,  
  embedding\_model text,  
  embedding vector(1536),  
  version integer NOT NULL DEFAULT 1,  
  active boolean NOT NULL DEFAULT true  
);

\-- Build only after representative volume and query plan validation.  
CREATE INDEX camera\_embedding\_hnsw  
ON production.camera\_presets  
USING hnsw (embedding vector\_cosine\_ops)  
WHERE embedding IS NOT NULL AND active;

CREATE INDEX environment\_embedding\_hnsw  
ON production.environments  
USING hnsw (embedding vector\_cosine\_ops)  
WHERE embedding IS NOT NULL AND active;

## **3.7 Transactional job creation**

CREATE OR REPLACE FUNCTION workflow.create\_structural\_render\_job(  
  p\_project\_id uuid,  
  p\_shot\_version\_id uuid,  
  p\_manifest\_asset\_version\_id uuid,  
  p\_idempotency\_key text,  
  p\_requested\_by text  
) RETURNS workflow.jobs  
LANGUAGE plpgsql  
SECURITY DEFINER  
SET search\_path \= pg\_catalog, workflow, production, rights, assets  
AS $$  
DECLARE  
  v\_shot production.shot\_versions;  
  v\_job workflow.jobs;  
BEGIN  
  SELECT \* INTO STRICT v\_shot  
  FROM production.shot\_versions  
  WHERE shot\_version\_id \= p\_shot\_version\_id  
    AND status \= 'APPROVED'  
  FOR SHARE;

  \-- Rights eligibility belongs in a dedicated policy function.  
  IF NOT rights.is\_shot\_version\_eligible(p\_shot\_version\_id, 'STRUCTURAL\_RENDER') THEN  
    RAISE EXCEPTION 'Shot version is not eligible for structural rendering';  
  END IF;

  INSERT INTO workflow.jobs(  
    job\_type, project\_id, shot\_version\_id, manifest\_asset\_version\_id,  
    idempotency\_key, requested\_by\_subject  
  ) VALUES (  
    'STRUCTURAL\_RENDER', p\_project\_id, p\_shot\_version\_id,  
    p\_manifest\_asset\_version\_id, p\_idempotency\_key, p\_requested\_by  
  )  
  ON CONFLICT (idempotency\_key)  
  DO UPDATE SET idempotency\_key \= EXCLUDED.idempotency\_key  
  RETURNING \* INTO v\_job;

  RETURN v\_job;  
END;  
$$;

## **3.8 Lease acquisition and heartbeat**

\-- Atomic lease acquisition; execute under a service role.  
WITH candidate AS (  
  SELECT job\_id  
  FROM workflow.jobs  
  WHERE state IN ('QUEUED','FAILED\_RETRYABLE')  
    AND job\_type \= :job\_type  
    AND attempt \< max\_attempts  
  ORDER BY priority DESC, queued\_at  
  FOR UPDATE SKIP LOCKED  
  LIMIT 1  
)  
UPDATE workflow.jobs j  
SET state \= 'LEASED',  
    lease\_owner \= :worker\_id,  
    lease\_token\_hash \= encode(digest(:lease\_token, 'sha256'), 'hex'),  
    lease\_expires\_at \= now() \+ interval '10 minutes',  
    attempt \= attempt \+ 1,  
    state\_version \= state\_version \+ 1  
FROM candidate c  
WHERE j.job\_id \= c.job\_id  
RETURNING j.\*;

\-- Heartbeat must match owner and hashed token.  
UPDATE workflow.jobs  
SET lease\_expires\_at \= now() \+ interval '10 minutes',  
    state\_version \= state\_version \+ 1  
WHERE job\_id \= :job\_id  
  AND lease\_owner \= :worker\_id  
  AND lease\_token\_hash \= encode(digest(:lease\_token, 'sha256'), 'hex')  
  AND state IN ('LEASED','RUNNING')  
RETURNING state\_version;

## **3.9 Row-level and service-role policy**

REVOKE ALL ON SCHEMA core, production, assets, workflow, rights, finance, audit FROM PUBLIC;

CREATE ROLE rafa\_api NOLOGIN;  
CREATE ROLE rafa\_orchestrator NOLOGIN;  
CREATE ROLE rafa\_edge\_worker NOLOGIN;  
CREATE ROLE rafa\_readonly NOLOGIN;

GRANT USAGE ON SCHEMA core, production, assets, workflow, rights TO rafa\_api;  
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA core, production, assets, workflow, rights TO rafa\_api;

GRANT USAGE ON SCHEMA workflow, assets TO rafa\_orchestrator;  
GRANT SELECT ON workflow.jobs, assets.asset\_versions TO rafa\_orchestrator;  
GRANT EXECUTE ON FUNCTION workflow.create\_structural\_render\_job(uuid,uuid,uuid,text,text) TO rafa\_api;

\-- Prefer narrowly scoped security-definer functions for lease/report actions  
\-- instead of broad table update access.

## **3.10 Database tests and evidence**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| DB-101 | Build clean migration from empty database. | Data Platform Engineer | Schema design | CI creates DB, applies all migrations, loads fixtures, runs tests. |
| DB-102 | Test every foreign key and constrained state. | Database QA Engineer | DB-101 | Invalid fixtures fail with expected SQLSTATE and audit. |
| DB-103 | Prove idempotent job creation. | Backend Engineer | create function | Concurrent identical requests return one job. |
| DB-104 | Prove lease recovery. | SRE/Data Engineer | lease functions | Expired worker lease returns to retryable queue without duplicate artifacts. |
| DB-105 | Benchmark vector and relational lookup. | Database Performance Engineer | representative fixtures | Query plans and p95 satisfy approved threshold. |
| DB-106 | Run point-in-time restore exercise. | DBA | backup pipeline | Restored environment reconciles row counts and checksums. |

# **4\. Contracts and APIs**

## **4.1 Production manifest JSON Schema**

**Excerpted normative manifest schema; split into reusable schema modules in the contracts repository**

{  
  "$schema": "https://json-schema.org/draft/2020-12/schema",  
  "$id": "https://contracts.rafa.internal/production-manifest/1-0-0",  
  "title": "Rafa Production Manifest",  
  "type": "object",  
  "additionalProperties": false,  
  "required": \[  
    "schema\_version","manifest\_id","project","shot","timeline",  
    "environment","camera","characters","render","rights","lineage"  
  \],  
  "properties": {  
    "schema\_version": {"const": "1.0.0"},  
    "manifest\_id": {"type": "string", "format": "uuid"},  
    "project": {  
      "type": "object",  
      "additionalProperties": false,  
      "required": \["project\_id","project\_code"\],  
      "properties": {  
        "project\_id": {"type": "string", "format": "uuid"},  
        "project\_code": {"type": "string", "pattern": "^\[A-Z0-9\]\[A-Z0-9\_-\]{2,31}$"}  
      }  
    },  
    "shot": {  
      "type": "object",  
      "additionalProperties": false,  
      "required": \["shot\_id","shot\_version\_id","shot\_code","duration\_frames"\],  
      "properties": {  
        "shot\_id": {"type": "string", "format": "uuid"},  
        "shot\_version\_id": {"type": "string", "format": "uuid"},  
        "shot\_code": {"type": "string", "maxLength": 64},  
        "duration\_frames": {"type": "integer", "minimum": 1, "maximum": 100000},  
        "director\_notes": {"type": "string", "maxLength": 20000}  
      }  
    },  
    "timeline": {  
      "type": "object",  
      "required": \["fps\_numerator","fps\_denominator","start\_frame"\],  
      "properties": {  
        "fps\_numerator": {"type": "integer", "minimum": 1},  
        "fps\_denominator": {"type": "integer", "minimum": 1},  
        "start\_frame": {"type": "integer", "minimum": 0}  
      }  
    },  
    "environment": {  
      "type": "object",  
      "required": \["environment\_id","environment\_version"\],  
      "properties": {  
        "environment\_id": {"type": "string","format": "uuid"},  
        "environment\_version": {"type": "integer","minimum": 1},  
        "latitude": {"type": \["number","null"\],"minimum": \-90,"maximum": 90},  
        "longitude": {"type": \["number","null"\],"minimum": \-180,"maximum": 180},  
        "altitude\_m": {"type": \["number","null"\]},  
        "datetime\_utc": {"type": \["string","null"\],"format": "date-time"},  
        "weather": {"type": "object"}  
      }  
    },  
    "camera": {  
      "type": "object",  
      "required": \["camera\_preset\_id","focal\_length\_mm","aperture\_f\_stop"\],  
      "properties": {  
        "camera\_preset\_id": {"type": "string","format": "uuid"},  
        "focal\_length\_mm": {"type": "number","exclusiveMinimum": 0},  
        "aperture\_f\_stop": {"type": "number","exclusiveMinimum": 0},  
        "transform": {  
          "$ref": "\#/$defs/transform"  
        },  
        "target\_transform": {  
          "$ref": "\#/$defs/transform"  
        },  
        "sequencer\_template": {"type": \["string","null"\],"maxLength": 512}  
      }  
    },  
    "characters": {  
      "type": "array","minItems": 0,"maxItems": 100,  
      "items": {  
        "type": "object","additionalProperties": false,  
        "required": \["character\_id","identity\_asset\_version\_id","transform"\],  
        "properties": {  
          "character\_id": {"type": "string","format": "uuid"},  
          "identity\_asset\_version\_id": {"type": "string","format": "uuid"},  
          "performance\_asset\_version\_id": {"type": \["string","null"\],"format": "uuid"},  
          "transform": {"$ref": "\#/$defs/transform"}  
        }  
      }  
    },  
    "render": {  
      "type": "object","additionalProperties": false,  
      "required": \["render\_profile","output\_root","required\_passes"\],  
      "properties": {  
        "render\_profile": {"type": "string","maxLength": 128},  
        "output\_root": {"type": "string","maxLength": 1024},  
        "required\_passes": {  
          "type": "array","uniqueItems": true,  
          "items": {"enum": \["beauty","depth","normal","motion","object\_id","cryptomatte"\]}  
        }  
      }  
    },  
    "rights": {  
      "type": "object","additionalProperties": false,  
      "required": \["eligibility\_decision\_id","permitted\_operation"\],  
      "properties": {  
        "eligibility\_decision\_id": {"type": "string","format": "uuid"},  
        "permitted\_operation": {"const": "STRUCTURAL\_RENDER"}  
      }  
    },  
    "lineage": {  
      "type": "object","required": \["source\_asset\_versions"\],  
      "properties": {  
        "source\_asset\_versions": {  
          "type": "array","uniqueItems": true,  
          "items": {"type": "string","format": "uuid"}  
        }  
      }  
    }  
  },  
  "$defs": {  
    "vector3": {  
      "type": "object","additionalProperties": false,  
      "required": \["x","y","z"\],  
      "properties": {  
        "x": {"type": "number"}, "y": {"type": "number"}, "z": {"type": "number"}  
      }  
    },  
    "transform": {  
      "type": "object","additionalProperties": false,  
      "required": \["location","rotation","scale"\],  
      "properties": {  
        "location": {"$ref": "\#/$defs/vector3"},  
        "rotation": {"$ref": "\#/$defs/vector3"},  
        "scale": {"$ref": "\#/$defs/vector3"}  
      }  
    }  
  }  
}

## **4.2 Normalized job API**

**Reference OpenAPI excerpt; replace identity URL and scopes through approved environment configuration**

openapi: 3.1.0  
info:  
  title: Rafa Job API  
  version: 1.0.0  
paths:  
  /v1/render-jobs:  
    post:  
      operationId: createRenderJob  
      security: \[{oidc: \[render.jobs.create\]}\]  
      parameters:  
        \- in: header  
          name: Idempotency-Key  
          required: true  
          schema: {type: string, maxLength: 200}  
      requestBody:  
        required: true  
        content:  
          application/json:  
            schema:  
              type: object  
              required: \[project\_id, shot\_version\_id\]  
              properties:  
                project\_id: {type: string, format: uuid}  
                shot\_version\_id: {type: string, format: uuid}  
      responses:  
        "201":  
          description: Job created  
        "200":  
          description: Existing idempotent job returned  
        "409":  
          description: Entity is not in an eligible state  
  /v1/render-jobs/{job\_id}:  
    get:  
      operationId: getRenderJob  
      security: \[{oidc: \[render.jobs.read\]}\]  
  /v1/render-jobs/{job\_id}/cancel:  
    post:  
      operationId: cancelRenderJob  
      security: \[{oidc: \[render.jobs.cancel\]}\]  
components:  
  securitySchemes:  
    oidc:  
      type: oauth2  
      flows:  
        clientCredentials:  
          tokenUrl: https://identity.example.invalid/oauth2/token  
          scopes:  
            render.jobs.create: create render jobs  
            render.jobs.read: read render jobs  
            render.jobs.cancel: cancel render jobs

## **4.3 Event envelope**

{  
  "event\_id": "uuid",  
  "event\_type": "workflow.job.state\_changed",  
  "event\_version": "1.0.0",  
  "occurred\_at": "RFC3339 timestamp",  
  "producer": "rafa-api",  
  "correlation\_id": "uuid",  
  "causation\_id": "uuid-or-null",  
  "subject": "workflow-job/uuid",  
  "data": {  
    "job\_id": "uuid",  
    "prior\_state": "RUNNING",  
    "new\_state": "SUCCEEDED",  
    "state\_version": 7,  
    "evidence\_asset\_version\_ids": \["uuid"\]  
  }  
}

## **4.4 Compatibility policy**

| Change | Compatibility class | Required action |
| :---- | :---- | :---- |
| Add optional field with safe default | Backward compatible | Minor schema version; consumer test. |
| Add enum value consumed by strict clients | Potentially breaking | Inventory consumers, feature flag, staged rollout. |
| Rename/remove field | Breaking | New major version, migration, deprecation window. |
| Change semantic meaning or units | Breaking | New field or major version; never silently reinterpret. |
| Add required field | Breaking for producers | New major version or two-phase optional-to-required migration. |
| Provider extension change | Adapter-local if isolated | Adapter tests and stored raw payload version. |

# **5\. Identity, secrets, network, and storage**

## **5.1 Identity model**

| Identity | Authentication | Authorized scope | Prohibited |
| :---- | :---- | :---- | :---- |
| Human employee | SSO \+ phishing-resistant MFA where available. | Role-based application and support access. | Shared credentials, direct production DB writes. |
| Rafa API service | Workload identity or short-lived client credential. | Domain functions and controlled DB role. | NAS administration, UE control. |
| Kestra service | Workload identity and mTLS/private network. | Read job metadata, call edge/provider gateways, report results. | Human approval decisions. |
| Edge worker | Device identity \+ short-lived worker token. | Lease assigned job, read approved artifacts, write assigned output. | Cross-project browsing, rights override. |
| n8n service | Workload identity. | Create interaction records, read presentation data, call approved provider gateway. | Authoritative direct state edits. |
| Support admin | Just-in-time privileged role with MFA and ticket. | Time-bounded troubleshooting. | Routine use of standing admin. |

## **5.2 Network zones**

Zone 10 \- Corporate clients  
  Employee endpoints, managed laptops, office productivity.  
Zone 20 \- Management  
  Hypervisor, switch, NAS administration, monitoring, out-of-band access.  
Zone 30 \- Production services  
  Rafa API, PostgreSQL, Kestra, n8n, observability.  
Zone 40 \- Render workers  
  Edge gateway and GPU nodes; outbound only to approved services.  
Zone 50 \- Storage data  
  NAS data interfaces; restricted to approved service identities.  
Zone 60 \- Guest / untrusted  
  Internet only.  
Zone 70 \- Recovery  
  Backup targets, immutable replicas, recovery services.

Default policy: deny inter-zone traffic. Allow only documented service flows.

## **5.3 Minimum firewall flows**

| Source | Destination | Purpose | Control |
| :---- | :---- | :---- | :---- |
| n8n | Rafa API | Create/read interaction and approval records. | HTTPS, workload identity. |
| Kestra | Rafa API | Acquire/report jobs and evidence. | HTTPS, mTLS/OIDC. |
| Kestra | Edge gateway | Dispatch render job. | Private HTTPS, allowlist, signed request. |
| Edge gateway | Rafa API | Lease heartbeat and status. | Private HTTPS, device identity. |
| Edge gateway/render node | NAS data endpoint | Read approved inputs and write assigned outputs. | SMB/NFS identity, path ACL. |
| Provider gateway | Approved provider | Submit/status/download. | Outbound HTTPS via controlled egress. |
| Observability agents | Telemetry collectors | Metrics, logs, traces. | Authenticated ingestion. |
| Backup service | DB/NAS | Snapshot/backup/verification. | Dedicated account and network path. |

## **5.4 Artifact URI and directory standard**

rafa://{environment}/{project\_code}/{entity\_type}/{entity\_id}/{version}/{artifact\_role}/{filename}

NAS reference layout:  
/Rafa/  
  /Projects/{PROJECT\_CODE}/  
    /00\_Admin/  
    /10\_Source/  
    /20\_Manifests/  
    /30\_Structural/  
       /{SHOT\_CODE}/v{NNN}/beauty/  
       /{SHOT\_CODE}/v{NNN}/depth/  
       /{SHOT\_CODE}/v{NNN}/normal/  
       /{SHOT\_CODE}/v{NNN}/logs/  
    /40\_Polish/  
    /50\_Audio/  
    /60\_Editorial/  
    /70\_QC/  
    /80\_Masters/  
    /90\_Delivery/  
    /99\_Archive/  
  /SharedAssets/  
  /System/  
  /Quarantine/  
  /Recovery/

Rules:  
\- Services use IDs and resolver APIs; humans may see names.  
\- Approved versions are immutable.  
\- Temporary workspaces are separate and automatically expired.  
\- Checksums are computed before and after transfer.

## **5.5 Path translation contract**

{  
  "storage\_uri": "rafa://prod/PRJ001/shot/8d.../3/structural/beauty.exr",  
  "resolver\_version": "1.0.0",  
  "mount\_profile": "render-node-windows",  
  "resolved\_path": "R:\\Rafa\\Projects\\PRJ001\\30\_Structural\\SH010\\v003\\beauty.exr",  
  "read\_only": false,  
  "expires\_at": "timestamp",  
  "allowed\_prefix": "R:\\Rafa\\Projects\\PRJ001\\30\_Structural\\SH010\\v003"  
}

## **5.6 Storage acceptance tests**

* Service identity can read only assigned source prefixes and write only assigned output prefixes.  
* Render completion fails when output exists only on local scratch and has not been flushed to durable storage.  
* Checksum remains identical across upload, NAS write, provider transfer, download, archive, and restore.  
* Snapshots protect approved masters from ordinary service-account deletion.  
* Restore test can rebuild a project delivery package from backup and database metadata within the approved RTO.  
* Temporary caches expire without deleting authoritative artifacts or evidence.

# **6\. Edge render gateway and Unreal Engine automation**

## **6.1 Edge gateway responsibilities**

* Authenticate and authorize the caller and assigned job.  
* Acquire or validate a job lease and maintain heartbeat.  
* Fetch and validate the immutable production manifest and rights eligibility decision.  
* Resolve governed URIs to local paths while preventing traversal outside the assigned prefix.  
* Confirm UE project revision, plugin inventory, content dependencies, GPU, storage, and license readiness.  
* Start, monitor, timeout, cancel, and clean up the UE process.  
* Capture stdout/stderr, engine logs, crash reports, command configuration, and environment facts.  
* Verify required render passes, frame count, dimensions, hashes, and durable storage.  
* Report normalized success/failure and evidence; never mark success solely from process exit code.

## **6.2 Reference edge API**

**Reference Python service; implementation must include real token validation, dependency injection, persistence, and tests**

from \_\_future\_\_ import annotations

from enum import Enum  
from hashlib import sha256  
from pathlib import Path  
from typing import Annotated  
from uuid import UUID

from fastapi import Depends, FastAPI, Header, HTTPException, status  
from pydantic import BaseModel, Field

app \= FastAPI(title="Rafa Edge Render Gateway", version="1.0.0")

class JobState(str, Enum):  
    accepted \= "ACCEPTED"  
    running \= "RUNNING"  
    succeeded \= "SUCCEEDED"  
    failed \= "FAILED"

class RenderRequest(BaseModel):  
    job\_id: UUID  
    manifest\_uri: str \= Field(pattern=r"^rafa://")  
    manifest\_sha256: str \= Field(pattern=r"^\[0-9a-f\]{64}$")  
    lease\_token: str \= Field(min\_length=32, max\_length=2048)  
    idempotency\_key: str \= Field(min\_length=8, max\_length=200)

class RenderAccepted(BaseModel):  
    job\_id: UUID  
    state: JobState  
    edge\_execution\_id: UUID

def authorize\_service(authorization: Annotated\[str, Header()\]) \-\> str:  
    \# Replace with verified OIDC/mTLS implementation.  
    if not authorization.startswith("Bearer "):  
        raise HTTPException(status\_code=status.HTTP\_401\_UNAUTHORIZED)  
    subject \= validate\_short\_lived\_token(authorization.removeprefix("Bearer "))  
    require\_scope(subject, "edge.render.execute")  
    return subject

@app.post("/v1/renders", response\_model=RenderAccepted, status\_code=202)  
def create\_render(  
    request: RenderRequest,  
    caller: str \= Depends(authorize\_service),  
) \-\> RenderAccepted:  
    job \= rafa.get\_job(request.job\_id)  
    if job.idempotency\_key \!= request.idempotency\_key:  
        raise HTTPException(409, "Idempotency key mismatch")  
    if job.state not in {"LEASED", "RUNNING"}:  
        raise HTTPException(409, f"Job state {job.state} is not executable")

    manifest\_bytes \= artifact\_store.read\_bytes(request.manifest\_uri)  
    if sha256(manifest\_bytes).hexdigest() \!= request.manifest\_sha256:  
        raise HTTPException(422, "Manifest checksum mismatch")

    manifest \= contracts.validate\_manifest(manifest\_bytes)  
    policy.require\_manifest\_eligible(manifest, operation="STRUCTURAL\_RENDER")  
    resolved \= path\_resolver.resolve\_for\_job(manifest, request.job\_id)  
    execution \= supervisor.enqueue(job, manifest, resolved, caller)  
    return RenderAccepted(  
        job\_id=request.job\_id,  
        state=JobState.accepted,  
        edge\_execution\_id=execution.execution\_id,  
    )

## **6.3 Supervisor state machine**

| State | Entry action | Success transition | Failure transition |
| :---- | :---- | :---- | :---- |
| ACCEPTED | Persist execution and reserve local work directory. | PREFLIGHT | FAILED\_TERMINAL if invalid. |
| PREFLIGHT | Validate manifest, paths, storage, UE revision, plugins, GPU, license. | LAUNCHING | FAILED\_RETRYABLE or TERMINAL by code. |
| LAUNCHING | Build allowlisted command and start process under restricted account. | RUNNING | FAILED\_RETRYABLE. |
| RUNNING | Heartbeat, monitor logs/GPU/storage, enforce timeout/cancel. | VERIFYING | FAILED\_RETRYABLE; collect crash evidence. |
| VERIFYING | Check passes, frames, dimensions, hashes, storage flush. | REPORTING | FAILED\_TERMINAL for corrupt/missing artifacts. |
| REPORTING | Persist artifact/evidence and report to Rafa. | SUCCEEDED | RECOVERY\_PENDING. |
| CLEANUP | Release lease, remove temp, preserve required logs. | CLOSED | ALERT if cleanup incomplete. |

## **6.4 Safe command construction**

from dataclasses import dataclass  
from pathlib import Path  
import subprocess

@dataclass(frozen=True)  
class UnrealCommand:  
    editor\_exe: Path  
    project\_file: Path  
    map\_path: str  
    executor\_script: str  
    job\_config: Path  
    log\_file: Path

    def argv(self) \-\> list\[str\]:  
        for p in (self.editor\_exe, self.project\_file, self.job\_config):  
            if not p.is\_file():  
                raise FileNotFoundError(p)  
        if not self.map\_path.startswith("/Game/"):  
            raise ValueError("Map must be an allowlisted /Game path")  
        return \[  
            str(self.editor\_exe),  
            str(self.project\_file),  
            self.map\_path,  
            f"-ExecutePythonScript={self.executor\_script}",  
            f"-RafaJobConfig={self.job\_config}",  
            "-Unattended",  
            "-NoSplash",  
            "-NoSound",  
            "-UTF8Output",  
            "-stdout",  
            f"-abslog={self.log\_file}",  
        \]

def launch(cmd: UnrealCommand, timeout\_seconds: int) \-\> subprocess.CompletedProcess\[str\]:  
    \# Never use shell=True. Use a restricted service account and a job object/process group.  
    return subprocess.run(  
        cmd.argv(),  
        check=False,  
        text=True,  
        stdout=subprocess.PIPE,  
        stderr=subprocess.STDOUT,  
        timeout=timeout\_seconds,  
    )

## **6.5 Unreal project baseline**

| Area | Required baseline | Evidence |
| :---- | :---- | :---- |
| Version | Approved UE release pinned per template branch; upgrade matrix maintained. | Engine/version manifest. |
| Plugins | Remote Control, Python scripting where required, Movie Render Queue/Pipeline, approved geospatial/character plugins. | Plugin inventory and license review. |
| Maps | Atomic test map, approved production templates, deterministic startup map. | Map load smoke tests. |
| Remote Control | Private/local binding only; exposed preset/functions minimized and named. | Endpoint inventory and network test. |
| Camera | Master CineCamera, target actor option, lens/aperture/focus contract, sequencer templates. | Camera fixture renders. |
| Characters | Versioned identity/rig adapter with validation and fallback. | Deformation and continuity test. |
| Render | Versioned MRQ presets, beauty/control passes, frame naming, output variables. | Artifact contract test. |
| Automation | Single entry script/command consumes validated job config and returns normalized evidence. | Headless integration test. |

## **6.6 Reference engine configuration**

; Config/DefaultEngine.ini \- reference only; validate approved UE version.  
\[RemoteControl\]  
DefaultServerPort=30010

\[HTTPServer.Routing\]  
; Bind to loopback or a protected render-zone address.  
DefaultBindAddress=127.0.0.1

\[/Script/Engine.Engine\]  
; Configure approved defaults; avoid interactive dialogs in service execution.

\[/Script/MovieRenderPipelineCore.MoviePipelineQueueEngineSubsystem\]  
; Project-specific settings belong in versioned MRQ assets.

## **6.7 Reference Unreal Python executor**

**Reference UE Python. Exact APIs vary by approved version; build an automated compatibility fixture.**

import json  
import os  
import sys  
import unreal

def fail(message: str) \-\> None:  
    unreal.log\_error(message)  
    raise RuntimeError(message)

def command\_line\_value(prefix: str) \-\> str:  
    command\_line \= unreal.SystemLibrary.get\_command\_line()  
    for token in command\_line.split():  
        if token.startswith(prefix):  
            return token.split("=", 1)\[1\].strip('"')  
    fail(f"Missing required argument {prefix}")

job\_config\_path \= command\_line\_value("-RafaJobConfig=")  
with open(job\_config\_path, "r", encoding="utf-8") as f:  
    job \= json.load(f)

\# The edge gateway has already validated the manifest and produced a local job config.  
required \= \["job\_id", "map\_path", "camera", "output\_dir", "render\_profile"\]  
missing \= \[key for key in required if key not in job\]  
if missing:  
    fail(f"Missing fields: {missing}")

output\_dir \= os.path.abspath(job\["output\_dir"\])  
allowed\_root \= os.path.abspath(job\["allowed\_output\_root"\])  
if os.path.commonpath(\[output\_dir, allowed\_root\]) \!= allowed\_root:  
    fail("Output directory escapes allowed root")  
os.makedirs(output\_dir, exist\_ok=True)

editor \= unreal.get\_editor\_subsystem(unreal.LevelEditorSubsystem)  
if not editor.load\_level(job\["map\_path"\]):  
    fail(f"Could not load map {job\['map\_path'\]}")

\# Resolve assets by approved, versioned paths. Do not scan by display name in production.  
camera\_actor \= unreal.EditorLevelLibrary.get\_actor\_reference(job\["camera"\]\["actor\_path"\])  
if not camera\_actor:  
    fail("Camera actor not found")

cine\_component \= camera\_actor.get\_cine\_camera\_component()  
cine\_component.set\_editor\_property(  
    "current\_focal\_length", float(job\["camera"\]\["focal\_length\_mm"\])  
)  
cine\_component.set\_editor\_property(  
    "current\_aperture", float(job\["camera"\]\["aperture\_f\_stop"\])  
)

\# Invoke a project-owned Blueprint/Python adapter to apply environment,  
\# characters, performance, and sequence bindings. Each adapter returns validation.  
adapter\_result \= unreal.RafaProductionLibrary.apply\_job\_configuration(json.dumps(job))  
if not adapter\_result.success:  
    fail(adapter\_result.message)

queue\_subsystem \= unreal.get\_editor\_subsystem(unreal.MoviePipelineQueueSubsystem)  
queue \= queue\_subsystem.get\_queue()  
queue.delete\_all\_jobs()  
pipeline\_job \= queue.allocate\_new\_job(unreal.MoviePipelineExecutorJob)  
pipeline\_job.job\_name \= str(job\["job\_id"\])  
pipeline\_job.map \= unreal.SoftObjectPath(job\["map\_path"\])  
pipeline\_job.sequence \= unreal.SoftObjectPath(job\["sequence\_path"\])  
pipeline\_job.set\_configuration(unreal.load\_asset(job\["render\_profile"\]))

executor \= unreal.MoviePipelinePIEExecutor()  
queue\_subsystem.render\_queue\_with\_executor\_instance(executor)

## **6.8 Preflight and output report format**

{  
  "edge\_report\_version": "1.0.0",  
  "job\_id": "uuid",  
  "edge\_execution\_id": "uuid",  
  "node\_id": "render-5090-01",  
  "unreal": {  
    "engine\_version": "approved-version",  
    "project\_git\_commit": "sha",  
    "plugin\_manifest\_sha256": "sha256"  
  },  
  "preflight": {  
    "manifest\_valid": true,  
    "rights\_valid": true,  
    "storage\_read": true,  
    "storage\_write": true,  
    "gpu\_ready": true,  
    "required\_assets": true  
  },  
  "process": {  
    "started\_at": "timestamp",  
    "completed\_at": "timestamp",  
    "exit\_code": 0,  
    "peak\_gpu\_memory\_bytes": 0,  
    "log\_asset\_version\_id": "uuid"  
  },  
  "artifacts": \[  
    {  
      "role": "beauty",  
      "asset\_version\_id": "uuid",  
      "frame\_count": 96,  
      "width": 1920,  
      "height": 1080,  
      "sha256\_manifest": "sha256"  
    }  
  \],  
  "result": "SUCCEEDED"  
}

# **7\. Kestra durable execution**

## **7.1 Namespace and flow layout**

cinematic.rafa  
  00\_intake\_dispatch  
  01\_validate\_manifest  
  02\_structural\_render  
  03\_verify\_structural\_package  
  04\_request\_polish  
  05\_wait\_polish  
  06\_media\_qc  
  07\_audio\_lipsync  
  08\_timeline\_export  
  09\_release\_package  
  90\_failure\_router  
  91\_reconcile\_stuck\_jobs  
  92\_cost\_reconciliation  
  93\_dataset\_eligibility

## **7.2 Master subflow pattern**

**Reference Kestra flow; confirm plugin types and expressions against approved release**

id: 00\_shot\_master  
namespace: cinematic.rafa  
description: "Durable orchestration for one approved shot version."

inputs:  
  \- id: job\_id  
    type: STRING  
  \- id: correlation\_id  
    type: STRING

tasks:  
  \- id: validate  
    type: io.kestra.plugin.core.flow.Subflow  
    namespace: cinematic.rafa  
    flowId: 01\_validate\_manifest  
    wait: true  
    transmitFailed: true  
    inputs:  
      job\_id: "{{ inputs.job\_id }}"  
      correlation\_id: "{{ inputs.correlation\_id }}"

  \- id: render  
    type: io.kestra.plugin.core.flow.Subflow  
    namespace: cinematic.rafa  
    flowId: 02\_structural\_render  
    wait: true  
    transmitFailed: true  
    inputs:  
      job\_id: "{{ inputs.job\_id }}"  
      manifest\_uri: "{{ outputs.validate.outputs.manifest\_uri }}"  
      manifest\_sha256: "{{ outputs.validate.outputs.manifest\_sha256 }}"  
      correlation\_id: "{{ inputs.correlation\_id }}"

  \- id: verify  
    type: io.kestra.plugin.core.flow.Subflow  
    namespace: cinematic.rafa  
    flowId: 03\_verify\_structural\_package  
    wait: true  
    transmitFailed: true  
    inputs:  
      job\_id: "{{ inputs.job\_id }}"  
      edge\_report\_uri: "{{ outputs.render.outputs.edge\_report\_uri }}"

errors:  
  \- id: report\_failure  
    type: io.kestra.plugin.core.http.Request  
    uri: "{{ secret('RAFA\_API\_BASE\_URL') }}/v1/internal/jobs/{{ inputs.job\_id }}/fail"  
    method: POST  
    headers:  
      Authorization: "Bearer {{ secret('KESTRA\_RAFA\_TOKEN') }}"  
      X-Correlation-ID: "{{ inputs.correlation\_id }}"  
    contentType: application/json  
    body: |  
      {  
        "execution\_id": "{{ execution.id }}",  
        "error\_summary": "{{ errorLogs() | jq('map(.message) | join(" | ")') }}",  
        "retryable": true  
      }

## **7.3 Structural render subflow**

id: 02\_structural\_render  
namespace: cinematic.rafa

inputs:  
  \- {id: job\_id, type: STRING}  
  \- {id: manifest\_uri, type: STRING}  
  \- {id: manifest\_sha256, type: STRING}  
  \- {id: correlation\_id, type: STRING}

tasks:  
  \- id: acquire\_edge\_lease  
    type: io.kestra.plugin.core.http.Request  
    uri: "{{ secret('RAFA\_API\_BASE\_URL') }}/v1/internal/jobs/{{ inputs.job\_id }}/lease"  
    method: POST  
    headers:  
      Authorization: "Bearer {{ secret('KESTRA\_RAFA\_TOKEN') }}"  
      Idempotency-Key: "{{ inputs.job\_id }}:edge-lease:v1"  
    contentType: application/json  
    body: |  
      {"execution\_id":"{{ execution.id }}","lease\_seconds":600}

  \- id: start\_edge\_render  
    type: io.kestra.plugin.core.http.Request  
    uri: "{{ outputs.acquire\_edge\_lease.body.edge\_gateway\_url }}/v1/renders"  
    method: POST  
    headers:  
      Authorization: "Bearer {{ secret('KESTRA\_EDGE\_TOKEN') }}"  
      X-Correlation-ID: "{{ inputs.correlation\_id }}"  
      Idempotency-Key: "{{ inputs.job\_id }}:structural:v1"  
    contentType: application/json  
    readTimeout: PT30S  
    retry:  
      type: exponential  
      interval: PT5S  
      maxInterval: PT1M  
      maxAttempts: 4  
      maxDuration: PT5M  
    body: |  
      {  
        "job\_id":"{{ inputs.job\_id }}",  
        "manifest\_uri":"{{ inputs.manifest\_uri }}",  
        "manifest\_sha256":"{{ inputs.manifest\_sha256 }}",  
        "lease\_token":"{{ outputs.acquire\_edge\_lease.body.lease\_token }}",  
        "idempotency\_key":"{{ inputs.job\_id }}:structural:v1"  
      }

  \- id: wait\_for\_edge\_completion  
    type: io.kestra.plugin.core.flow.Until  
    condition: "{{ outputs.check\_edge\_status.body.state in \['SUCCEEDED','FAILED','CANCELLED'\] }}"  
    maxIterations: 240  
    tasks:  
      \- id: pause  
        type: io.kestra.plugin.core.flow.Sleep  
        duration: PT30S  
      \- id: check\_edge\_status  
        type: io.kestra.plugin.core.http.Request  
        uri: "{{ outputs.acquire\_edge\_lease.body.edge\_gateway\_url }}/v1/renders/{{ outputs.start\_edge\_render.body.edge\_execution\_id }}"  
        method: GET  
        headers:  
          Authorization: "Bearer {{ secret('KESTRA\_EDGE\_TOKEN') }}"  
          X-Correlation-ID: "{{ inputs.correlation\_id }}"

  \- id: require\_success  
    type: io.kestra.plugin.core.flow.If  
    condition: "{{ outputs.check\_edge\_status.body.state \!= 'SUCCEEDED' }}"  
    then:  
      \- id: fail  
        type: io.kestra.plugin.core.execution.Fail  
        errorMessage: "{{ outputs.check\_edge\_status.body.error.message }}"

outputs:  
  \- id: edge\_report\_uri  
    type: STRING  
    value: "{{ outputs.check\_edge\_status.body.edge\_report\_uri }}"

## **7.4 Reconciliation flow**

A scheduled reconciliation flow is mandatory because callbacks, worker processes, and workflow engines can fail between side effects and state persistence. The reconciler queries jobs whose leases or external waits exceed policy, inspects the edge/provider system by idempotency key, and transitions the job to the observed truthful state.

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| KES-101 | Implement flow validation and test namespace. | Kestra Engineer | Kestra environment | Every flow imports, lints, and executes against mocks. |
| KES-102 | Implement job lease and structural-render subflow. | Kestra Engineer | Rafa API \+ edge mock | Replay/failure tests prove no duplicate render. |
| KES-103 | Implement global failure router. | Platform Engineer | Error taxonomy | Retryable/terminal/cancel/security/budget paths are distinct. |
| KES-104 | Implement stuck-job reconciliation. | SRE Engineer | Status APIs | Injected lost callback is reconciled without manual DB edits. |
| KES-105 | Implement namespace RBAC and secret references. | Security Engineer | IAM baseline | Flow authors cannot read production secret values. |

# **8\. n8n interaction and human-director workflow**

## **8.1 Workflow ownership rules**

* n8n presents and collects human decisions; Rafa persists the decision as authoritative state.  
* n8n may call the provider gateway and manage asynchronous polling when the work is interaction-adjacent, but it must checkpoint job IDs and statuses in Rafa.  
* n8n does not move multi-gigabyte media through workflow memory. It uses signed URLs or storage references.  
* Credentials are connector-managed and environment-specific. Exports contain credential references, never secrets.  
* Every production workflow export is version controlled and deployed through the release process.  
* Complex validation, authorization, or reusable business logic belongs in tested services or database functions, not opaque expressions.

## **8.2 Director workflow topology**

Webhook / Internal Form  
  \-\> Authenticate and validate intake  
  \-\> Create draft project/shot in Rafa  
  \-\> Generate constrained story proposal  
  \-\> Gate A: plot/script approval  
  \-\> Resolve or create governed character assets  
  \-\> Gate B: identity/rights approval  
  \-\> Produce typed shot plan and cost forecast  
  \-\> Gate C: shot/camera/manifest approval  
  \-\> Create render job and invoke Kestra  
  \-\> Wait for structural-ready event  
  \-\> Present structural review  
  \-\> Gate D: accept / bounded revision / cancel  
  \-\> Submit polish variants through provider gateway  
  \-\> Poll/callback and present variants with cost  
  \-\> Gate E: choose master candidate  
  \-\> Audio/lip-sync/editorial/QC  
  \-\> Gate F: release approval

## **8.3 Approval callback payload**

{  
  "approval\_token": "opaque-one-time-token",  
  "entity\_type": "SHOT\_VERSION",  
  "entity\_id": "uuid",  
  "entity\_version": "3",  
  "decision": "APPROVED",  
  "rationale": "Camera and blocking approved. Preserve 85mm lens.",  
  "requested\_changes": \[\],  
  "client\_context": {  
    "workflow\_execution\_id": "n8n-execution-id",  
    "presented\_asset\_version\_ids": \["uuid"\]  
  }  
}

## **8.4 Constrained asset-mapping prompt**

SYSTEM ROLE  
You are a deterministic production asset resolver. You map a director's approved  
shot text to candidate assets that already exist in the supplied catalog.

RULES  
1\. Output valid JSON matching the supplied schema; no markdown or explanation.  
2\. Never invent an asset ID, version, camera, location, or character.  
3\. Use only candidates in VALID\_ASSETS.  
4\. If no candidate is defensible, return resolution\_status \= "UNRESOLVED".  
5\. Include evidence\_terms copied from the shot text and a confidence in \[0,1\].  
6\. The result is a proposal; a human or policy gate decides approval.

OUTPUT  
{  
  "resolution\_status": "RESOLVED|UNRESOLVED",  
  "character\_candidates": \[  
    {"asset\_id":"uuid","version":1,"confidence":0.0,"evidence\_terms":\["..."\]}  
  \],  
  "camera\_candidate": {  
    "camera\_preset\_id":"uuid","confidence":0.0,"evidence\_terms":\["..."\]  
  },  
  "environment\_candidate": {  
    "environment\_id":"uuid","version":1,"confidence":0.0,"evidence\_terms":\["..."\]  
  }  
}

VALID\_ASSETS  
{{ catalog\_json }}

SHOT  
{{ approved\_shot\_text }}

## **8.5 Async provider loop pattern**

| Node | Configuration | Failure handling |
| :---- | :---- | :---- |
| Submit job | POST normalized request with idempotency key; store provider job ID. | Retry transport only; resolve duplicate by idempotency. |
| Persist status | Rafa API transition to WAITING\_EXTERNAL with provider reference. | Workflow stops if persistence fails; reconciler can inspect provider. |
| Wait | Durable wait 30-120 seconds with jitter based on quota. | Maximum wall-clock and attempts. |
| Check status | GET provider gateway status, not raw vendor endpoint from every workflow. | Normalize rate limit, transient, terminal, cancelled. |
| Switch | completed \-\> result; failed \-\> report; queued/running \-\> wait. | Unknown status is not treated as success. |
| Download/register | Gateway streams result to governed storage and registers checksum. | Partial download is quarantined. |
| Notify/review | Present exact version, cost, and comparison to approver. | Expired review returns to pending/expired state. |

## **8.6 n8n deployment acceptance**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| N8N-101 | Build intake workflow with authentication, validation, and rate limits. | n8n Engineer | Rafa API | Negative tests reject anonymous, oversized, and malformed requests. |
| N8N-102 | Build reusable approval subworkflow. | n8n Engineer | Identity \+ Rafa approval API | Decision is version-specific, one-time, auditable, and restart-safe. |
| N8N-103 | Build structured edit/regenerate loop. | Workflow Product Engineer | Approval workflow | Revisions create new version; approved source remains immutable. |
| N8N-104 | Build provider polling subworkflow. | n8n Engineer | Provider gateway | Restart and lost callback tests pass. |
| N8N-105 | Create workflow export/deploy/test process. | Release Engineer | Git repository | Exports are diffable, environment-neutral, and deployed through CI. |

# **9\. Provider-neutral AI polish gateway**

## **9.1 Capability contract**

class VideoPolishProvider(Protocol):  
    provider\_name: str

    def capabilities(self) \-\> ProviderCapabilities: ...  
    def submit(self, request: PolishRequest) \-\> Submission: ...  
    def status(self, provider\_job\_id: str) \-\> NormalizedStatus: ...  
    def cancel(self, provider\_job\_id: str) \-\> CancelResult: ...  
    def fetch\_result(self, provider\_job\_id: str) \-\> ResultPackage: ...  
    def estimate\_cost(self, request: PolishRequest) \-\> CostEstimate: ...

class PolishRequest(BaseModel):  
    job\_id: UUID  
    input\_asset\_version\_id: UUID  
    prompt: str  
    negative\_prompt: str | None \= None  
    control\_strength: float \= Field(ge=0.0, le=1.0)  
    duration\_frames: int \= Field(gt=0)  
    fps\_numerator: int \= Field(gt=0)  
    fps\_denominator: int \= Field(gt=0)  
    width: int \= Field(gt=0)  
    height: int \= Field(gt=0)  
    seed: int | None \= None  
    variants: int \= Field(default=1, ge=1, le=16)  
    provider\_extensions: dict\[str, Any\] \= {}

## **9.2 Normalized status and error taxonomy**

| Normalized state/code | Meaning | Default action |
| :---- | :---- | :---- |
| QUEUED | Accepted but not processing. | Poll with backoff. |
| RUNNING | Processing. | Poll/callback; enforce wall-clock. |
| SUCCEEDED | Result available but not yet registered. | Fetch, verify, register. |
| FAILED\_TRANSIENT | Rate limit, provider 5xx, network, capacity. | Retry under budget/attempt policy. |
| FAILED\_INPUT | Unsupported format, dimensions, prompt, rights rejection. | Terminal; fix upstream. |
| FAILED\_POLICY | Provider or company policy rejected operation. | Terminal; escalate to policy owner. |
| FAILED\_BUDGET | Cost estimate or accrued cost exceeded allowed amount. | Stop and require budget approval. |
| CANCELLED | Cancelled by user/system/provider. | Persist terminal state and costs. |
| UNKNOWN | Provider returned unmapped state. | Do not assume success; alert adapter owner. |

## **9.3 Result validation**

* Verify provider response signature/authenticity where supported.  
* Download to quarantine, calculate checksum, and reject truncated/HTML/error payloads.  
* Probe container, codec, dimensions, frame rate, duration, audio, and corruption.  
* Compare duration and frame count to contract tolerance.  
* Register raw provider response and normalized metadata with secrets redacted.  
* Attribute cost using provider pricing version and observed quantity.  
* Create lineage edge from exact structural input and prompt/parameter artifact to output.  
* Move validated output to governed storage; never reference an expiring vendor URL as the durable master.

## **9.4 Provider adapter contract tests**

@pytest.mark.contract  
def test\_submit\_is\_idempotent(adapter, sandbox\_request):  
    first \= adapter.submit(sandbox\_request)  
    second \= adapter.submit(sandbox\_request)  
    assert first.normalized\_job\_key \== second.normalized\_job\_key

@pytest.mark.contract  
def test\_unknown\_provider\_state\_is\_not\_success(adapter, fake\_response):  
    fake\_response.status \= "brand\_new\_state"  
    normalized \= adapter.normalize\_status(fake\_response)  
    assert normalized.state \== "UNKNOWN"  
    assert normalized.terminal is False

@pytest.mark.contract  
def test\_result\_must\_match\_contract(adapter, completed\_job):  
    package \= adapter.fetch\_result(completed\_job.provider\_job\_id)  
    assert package.sha256  
    assert package.byte\_size \> 0  
    assert package.media.width \== completed\_job.request.width  
    assert abs(package.media.duration\_seconds \-  
               completed\_job.request.duration\_seconds) \<= 0.05

# **10\. Media QC, audio, timeline, and delivery**

## **10.1 Technical QC command pattern**

**Reference commands. Delivery standards and tolerances must be project-configured.**

ffprobe \-v error   \-show\_entries format=filename,format\_name,duration,size,bit\_rate   \-show\_entries stream=index,codec\_type,codec\_name,width,height,r\_frame\_rate,avg\_frame\_rate,pix\_fmt,channels,sample\_rate   \-of json   input.mp4 \> probe.json

ffmpeg \-v error \-i input.mp4 \-f null \- 2\> decode\_errors.log

ffmpeg \-hide\_banner \-nostats \-i input.mp4   \-filter\_complex ebur128=peak=true   \-f null \- 2\> loudness.log

## **10.2 Machine-readable QC policy**

{  
  "profile\_id": "DELIVERY\_HD\_24P\_STEREO\_V1",  
  "container": \["mov","mp4"\],  
  "video": {  
    "codecs": \["prores","h264","hevc"\],  
    "width": 1920,  
    "height": 1080,  
    "fps": {"numerator": 24, "denominator": 1, "tolerance": 0},  
    "duration\_tolerance\_seconds": 0.05,  
    "pixel\_formats": \["yuv420p","yuv422p10le"\]  
  },  
  "audio": {  
    "required": true,  
    "sample\_rate\_hz": 48000,  
    "channels": \[2\],  
    "integrated\_loudness\_lufs": \-23.0,  
    "loudness\_tolerance\_lu": 1.0,  
    "true\_peak\_max\_dbtp": \-1.0  
  },  
  "captions": {  
    "required": false,  
    "allowed\_formats": \["srt","vtt","itt"\]  
  }  
}

## **10.3 Correct frame-to-timecode helper**

**Non-drop-frame integer-rate helper only. Use a validated timecode library for 29.97/59.94 drop frame.**

CREATE OR REPLACE FUNCTION production.frames\_to\_tc(  
  p\_frames bigint,  
  p\_fps integer  
) RETURNS text  
LANGUAGE sql IMMUTABLE STRICT  
AS $$  
  SELECT lpad((p\_frames / (p\_fps \* 3600))::text, 2, '0') || ':' ||  
         lpad(((p\_frames / (p\_fps \* 60)) % 60)::text, 2, '0') || ':' ||  
         lpad(((p\_frames / p\_fps) % 60)::text, 2, '0') || ':' ||  
         lpad((p\_frames % p\_fps)::text, 2, '0');  
$$;

## **10.4 Timeline view**

CREATE OR REPLACE VIEW production.v\_sequence\_timeline AS  
WITH ordered AS (  
  SELECT  
    seq.sequence\_id,  
    seq.name AS sequence\_name,  
    seq.frame\_rate\_num,  
    seq.frame\_rate\_den,  
    sh.shot\_id,  
    sh.shot\_code,  
    sh.sort\_order,  
    sh.duration\_frames,  
    av.storage\_uri AS master\_uri,  
    sum(sh.duration\_frames) OVER (  
      PARTITION BY seq.sequence\_id  
      ORDER BY sc.sort\_order, sh.sort\_order  
      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  
    ) \- sh.duration\_frames AS record\_start\_frame,  
    sum(sh.duration\_frames) OVER (  
      PARTITION BY seq.sequence\_id  
      ORDER BY sc.sort\_order, sh.sort\_order  
      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  
    ) AS record\_end\_frame  
  FROM production.sequences seq  
  JOIN production.scenes sc ON sc.sequence\_id \= seq.sequence\_id  
  JOIN production.shots sh ON sh.scene\_id \= sc.scene\_id  
  JOIN production.shot\_deliverables sd  
    ON sd.shot\_id \= sh.shot\_id AND sd.role \= 'APPROVED\_MASTER'  
  JOIN assets.asset\_versions av  
    ON av.asset\_version\_id \= sd.asset\_version\_id  
)  
SELECT \* FROM ordered;

## **10.5 EDL/export rule**

The database provides authoritative ordering and frame math. A versioned exporter converts that data to CMX 3600 EDL, FCPXML, OTIO, or supported target formats. Do not rely on handcrafted string concatenation without round-trip tests. The exporter must account for source timecode, handles, reel identifiers, drop-frame rules, audio tracks, transitions, supported URI/path conventions, and target-editor limitations.

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| MED-101 | Implement ffprobe/decode QC service. | Media Systems Engineer | Artifact registry | JSON report, profile comparison, evidence registration. |
| MED-102 | Implement audio loudness and channel QC. | Audio Engineer | QC service | Synthetic pass/fail fixtures and report. |
| MED-103 | Implement timeline canonical data model. | Editorial Systems Engineer | Rafa schema | Ordering, handles, transitions, track assignments, approvals. |
| MED-104 | Implement EDL/FCPXML/OTIO exporters. | Editorial Systems Engineer | MED-103 | Round-trip fixture imports into each supported editor. |
| MED-105 | Build delivery package generator. | Release Engineer | QC \+ rights | Master, captions, cue sheet, checksums, metadata, rights/attribution, receipt. |

# **11\. Observability, FinOps, SRE, backup, and recovery**

## **11.1 Required correlation fields**

correlation\_id  
project\_id  
shot\_id  
shot\_version\_id  
job\_id  
workflow\_execution\_id  
edge\_execution\_id  
provider\_job\_id  
asset\_version\_id  
node\_id  
service  
environment  
release\_version

## **11.2 Metrics**

| Metric | Type | Labels | Purpose |
| :---- | :---- | :---- | :---- |
| rafa\_jobs\_queued | Gauge | job\_type, priority, environment | Queue pressure. |
| rafa\_job\_duration\_seconds | Histogram | job\_type, result | Stage latency. |
| rafa\_job\_attempts\_total | Counter | job\_type, result, error\_code | Retry/failure trend. |
| rafa\_edge\_gpu\_memory\_bytes | Gauge | node\_id, gpu | Capacity and OOM risk. |
| rafa\_storage\_write\_seconds | Histogram | storage\_class | Storage performance. |
| rafa\_provider\_cost\_amount | Counter | provider, model, project | Attributed API cost. |
| rafa\_provider\_status\_unknown\_total | Counter | provider | Adapter drift. |
| rafa\_qc\_failures\_total | Counter | profile, rule | Quality trend. |
| rafa\_approval\_wait\_seconds | Histogram | gate, project | Human bottleneck. |
| rafa\_delivery\_on\_time\_total | Counter | project\_type, result | Business delivery. |

## **11.3 SLO examples**

| Service | SLI | Initial target | Error-budget response |
| :---- | :---- | :---- | :---- |
| Rafa API | Successful eligible requests / total. | 99.5% monthly during pilot. | Freeze nonessential changes if budget exhausted. |
| Job dispatch | Queued eligible jobs leased within threshold. | 95% within 5 minutes at pilot load. | Capacity/queue review. |
| Structural render | Valid manifests producing verified package. | 98% excluding approved content errors. | Reliability corrective action. |
| Provider gateway | Correct normalized lifecycle and result handling. | 99% for supported provider responses. | Disable drifting adapter. |
| Delivery | Approved deliveries on or before commitment. | 95% pilot; raise by stage. | Executive review and root cause. |
| Restore | Successful restore within RTO. | 100% quarterly exercise. | Immediate remediation and retest. |

## **11.4 Alert examples**

groups:  
\- name: rafa-production  
  rules:  
  \- alert: RafaRenderQueueStalled  
    expr: rafa\_jobs\_queued{job\_type="STRUCTURAL\_RENDER"} \> 0  
          and rate(rafa\_jobs\_started\_total{job\_type="STRUCTURAL\_RENDER"}\[15m\]) \== 0  
    for: 15m  
    labels: {severity: page}  
    annotations:  
      summary: "Structural render queue is not draining"  
      runbook: "docs://runbooks/render-queue-stalled"

  \- alert: RafaProviderUnknownState  
    expr: increase(rafa\_provider\_status\_unknown\_total\[10m\]) \> 0  
    labels: {severity: ticket}  
    annotations:  
      summary: "Provider adapter received an unmapped state"

  \- alert: RafaProjectCostAnomaly  
    expr: rafa\_project\_cost\_hourly \> 2 \* avg\_over\_time(rafa\_project\_cost\_hourly\[7d\])  
    for: 10m  
    labels: {severity: ticket}

## **11.5 Backup and recovery tiers**

| Asset/system | Backup pattern | RPO | RTO | Test |
| :---- | :---- | :---- | :---- | :---- |
| PostgreSQL | Continuous WAL/PITR \+ daily full \+ encrypted offsite. | Defined by production stage; pilot target \<= 15 min. | Pilot target \<= 4 h. | Quarterly PITR. |
| NAS approved assets | Snapshots \+ replicated/offsite immutable backup. | After successful artifact registration; target \<= 4 h. | Tier-based; masters \<= 24 h. | Quarterly sample \+ annual project restore. |
| Workflow definitions | Git and environment backups. | Per merge/deploy. | \< 4 h. | Rebuild staging from source. |
| Secrets/config | Approved vault backup/recovery procedure. | Per change. | \< 4 h. | Break-glass exercise. |
| Observability | Configuration in Git; selected logs retained off-service. | Policy-based. | Best effort unless incident/legal need. | Query/archive test. |

# **12\. CI/CD, testing, release, and rollback**

## **12.1 Test pyramid**

| Layer | Scope | Run frequency | Examples |
| :---- | :---- | :---- | :---- |
| Static | Formatting, lint, type, schema, secret, dependency, IaC, container. | Every commit/PR. | ruff/mypy, JSON Schema lint, SQL lint, SBOM. |
| Unit | Pure domain logic, adapters, path rules, error mapping. | Every PR. | State transition, cost, URI resolver. |
| Contract | API/schema/provider/edge/workflow compatibility. | Every PR and scheduled sandbox. | OpenAPI consumer, provider mock, manifest fixtures. |
| Integration | Postgres, storage emulator, mock provider, orchestrator. | PR for affected components. | Create job \-\> lease \-\> result. |
| System | Staging with edge worker and UE atomic project. | Release candidate/nightly. | Approved manifest \-\> structural package. |
| Resilience | Process kill, network loss, timeout, duplicate callback, storage full. | Scheduled and pre-gate. | No duplicate work; truthful state. |
| Security | SAST/DAST, authz, secrets, dependency, image, network. | PR/release/scheduled. | Privilege and exposure tests. |
| Performance | Queue, DB, storage, render, API, provider simulation. | Pre-gate and material change. | Capacity thresholds. |
| Operational | Backup/restore, alerts, runbooks, rollback. | Quarterly/release. | Restore and incident game day. |

## **12.2 GitHub Actions reference**

**Pin all actions and dependencies to approved immutable revisions.**

name: quality  
on:  
  pull\_request:  
  push:  
    branches: \[main\]

permissions:  
  contents: read  
  security-events: write

jobs:  
  test:  
    runs-on: ubuntu-latest  
    steps:  
      \- uses: actions/checkout@\<PINNED\_COMMIT\_SHA\>  
      \- uses: actions/setup-python@\<PINNED\_COMMIT\_SHA\>  
        with: {python-version: "3.12"}  
      \- run: python \-m pip install \--require-hashes \-r requirements-dev.txt  
      \- run: make lint  
      \- run: make typecheck  
      \- run: make contracts  
      \- run: make unit  
      \- run: docker compose \-f docker-compose.test.yml up \-d \--wait  
      \- run: make integration  
      \- run: make sbom  
      \- run: make secret-scan  
      \- run: make image-scan  
      \- if: always()  
        run: docker compose \-f docker-compose.test.yml down \-v

## **12.3 Release packet**

* Release identifier, commit/digest, change summary, requirement traceability, and approved pull requests.  
* Migration plan, compatibility assessment, feature flags, configuration changes, and secret references.  
* Test results, security scans, performance comparison, and known exceptions.  
* Deployment procedure, smoke tests, monitoring window, rollback trigger, and rollback steps.  
* Service owners, support/on-call acknowledgement, documentation changes, and stakeholder communication.

## **12.4 Rollback rule**

Rollback is a designed capability, not a sentence in a release note. Application releases must preserve compatible data paths or use a forward-fix/compensating migration plan. Workflow releases must preserve in-flight executions or provide a migration/reconciliation procedure. UE template releases must keep the prior engine/project revision available until active work is completed or explicitly migrated.

# **13\. Engineering organization and hiring specifications**

| Role | Mandate | Minimum capability | First accountable outcome |
| :---- | :---- | :---- | :---- |
| Head of Engineering / Chief Architect | Own architecture, standards, technical portfolio, hiring, risk, and executive translation. | Distributed systems, data, media pipelines, security, operational leadership. | Approved architecture and staffed execution system. |
| Technical Program Manager | Own integrated plan, dependencies, gates, evidence, risks, vendor and cross-team cadence. | Complex software/hardware program delivery. | Gate packets and reliable portfolio reporting. |
| Backend/Data Platform Engineer | Build Rafa API, schema, state model, audit, migrations, performance. | PostgreSQL, APIs, transactions, testing, security. | Authoritative data backbone. |
| Platform/SRE Engineer | Build environments, CI/CD, orchestration, observability, reliability, recovery. | Containers, IaC, networks, secrets, SLOs. | Operable and recoverable platform. |
| Rendering Platform Engineer | Build edge gateway, process supervisor, path resolver, artifact verifier. | Python/Go/C\#, Windows/Linux services, GPU workloads. | Unattended render service. |
| Unreal Technical Director | Build UE template, camera/world/character adapters, MRQ, automation. | UE C++/Blueprint/Python, Sequencer, rendering. | Reproducible structural render. |
| Workflow Automation Engineer | Build n8n and Kestra workflows with tests and state discipline. | APIs, workflow engines, async patterns. | Human and execution workflows. |
| AI Media Integration Engineer | Build provider gateway, adapters, evaluation, cost and quality controls. | Media APIs, async jobs, ML systems, testing. | Replaceable polish capability. |
| Media Systems/Post Engineer | Build QC, audio, lip-sync, editorial exports, delivery automation. | FFmpeg, codecs, timecode, NLE integration. | Conformant master pipeline. |
| Security/Privacy Engineer | Threat model, IAM, network, secret, logging, vendor/data controls. | Application/cloud/security operations. | Production security readiness. |
| QA Automation Engineer | Create fixture corpus, contract/system/resilience tests, gate evidence. | Automation, API/media testing, failure injection. | Independent verification. |

## **13.1 Interview work-sample examples**

| Role | Work sample | Pass signals |
| :---- | :---- | :---- |
| Backend/Data | Design job lease and audit schema; write concurrency tests. | Correct transactions, failure thinking, migration discipline. |
| SRE/Platform | Design private cloud-to-edge path and recovery test. | Least privilege, observability, RPO/RTO, practical tradeoffs. |
| Unreal TD | Automate an atomic camera change and MRQ render from typed config. | Reproducibility, engine understanding, artifact verification. |
| Workflow | Model submit/poll/retry/cancel with idempotency. | Durable state, no duplicate side effects, clear error taxonomy. |
| AI Integration | Normalize two fictional provider APIs behind one contract. | Capability abstraction, cost/quality evidence, unknown-state safety. |
| Media Systems | Validate media and export a small tested timeline. | Frame-rate/timecode correctness, QC depth, round-trip testing. |
| Security | Threat-model edge render control and NAS access. | Attack paths, compensating controls, operational usability. |
| TPM | Turn a vague end-to-end goal into gates, dependencies, and evidence. | Critical path, risk, ownership, decision clarity. |

# **14\. Epic and ticket catalog**

The following backlog is intentionally detailed enough for employment statements of work and sprint planning. Managers must add effort, assignee, environment, and exact repository before execution. Ticket numbering should be preserved for traceability.

## **EPIC A \- Governance and developer platform**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-A-001 | Create repository organization, CODEOWNERS, PR templates, issue templates, and branch protections. | Developer Experience Engineer | Executive/engineering ownership | All repositories meet baseline audit. |
| ENG-A-002 | Create architecture decision process and decision index. | Chief Architect | Master requirements | ADR template, owners, review cadence, published index. |
| ENG-A-003 | Create environment configuration catalog and secret classification. | Release Engineer | Service inventory | All variables and secrets documented without values. |
| ENG-A-004 | Build local developer composition with mocks and synthetic fixtures. | Platform Engineer | Contracts | One-command startup and integration test. |
| ENG-A-005 | Build CI reusable workflows for code, schema, containers, and documents. | Developer Experience Engineer | Repositories | Blocking checks, pinned actions, evidence retention. |
| ENG-A-006 | Create service catalog with owner, SLO, runbook, data class, dependencies. | SRE Lead | Architecture | 100% production services registered. |

## **EPIC B \- Rafa database and domain API**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-B-001 | Write migration 001 for schemas, extensions, and roles. | Data Platform Engineer | DB environment | Clean apply/rollback or compensating plan. |
| ENG-B-002 | Implement projects, sequences, scenes, shots, and versioned manifests. | Data Platform Engineer | B-001 | Constraints, fixtures, unit and migration tests. |
| ENG-B-003 | Implement asset registry, versions, checksums, and lineage. | Data Platform Engineer | B-001 | Lineage traversal and duplicate tests. |
| ENG-B-004 | Implement rights records and eligibility function. | Backend/Legal Systems Engineer | B-003 | Prohibited operation tests and audit. |
| ENG-B-005 | Implement jobs, idempotency, leases, attempts, and transition events. | Backend Engineer | B-002 | Concurrency and worker-death tests. |
| ENG-B-006 | Implement approvals and version-specific decision API. | Backend Engineer | Identity | Authz and replay tests. |
| ENG-B-007 | Implement cost events and project cost views. | FinOps Engineer | Jobs/provider contract | Reconciliation fixture. |
| ENG-B-008 | Implement audit event partitioning, retention, and query API. | Data Engineer | B-001 | Tamper/retention/query tests. |
| ENG-B-009 | Implement semantic asset search with model-version separation. | ML/Data Engineer | Asset catalog | Quality and performance benchmark. |
| ENG-B-010 | Implement PITR backup, restore, and schema reconciliation tests. | DBA/SRE | Production-like DB | Documented restore evidence. |

## **EPIC C \- Contracts and API governance**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-C-001 | Publish production manifest schema modules and examples. | API Engineer | Domain model | Valid/invalid corpus in CI. |
| ENG-C-002 | Publish Rafa Job API OpenAPI and generated client policy. | API Engineer | B-005 | Consumer tests and versioning. |
| ENG-C-003 | Publish event envelope and state-change schemas. | Platform Engineer | B-005 | Producer/consumer compatibility tests. |
| ENG-C-004 | Publish artifact package and edge report schemas. | Rendering Platform Engineer | Storage/UE plan | Fixture and validator. |
| ENG-C-005 | Create API error taxonomy and retry guidance. | Chief Architect | All APIs | Every error maps to operator action. |
| ENG-C-006 | Create contract registry and deprecation process. | API Governance Lead | Schemas | Owner and consumer inventory. |

## **EPIC D \- Storage and infrastructure**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-D-001 | Design VLANs, routes, firewall policy, DNS, and certificate trust. | Network Engineer | Threat model | Reviewed network diagram and test. |
| ENG-D-002 | Configure NAS shares, service accounts, ACLs, quotas, and snapshots. | Storage Engineer | Identity model | Path isolation and snapshot test. |
| ENG-D-003 | Implement governed URI resolver and path-map service. | Backend Engineer | Artifact contract | Traversal and cross-project access tests. |
| ENG-D-004 | Implement artifact checksum manifest and durable-registration API. | Storage Platform Engineer | Asset registry | Transfer/restart/duplicate tests. |
| ENG-D-005 | Implement backup replication and immutable/offsite copy. | Infrastructure Engineer | D-002 | Restore and ransomware scenario drill. |
| ENG-D-006 | Implement render-node baseline image and configuration management. | Endpoint/Platform Engineer | Hardware | Rebuild node from source and inventory evidence. |

## **EPIC E \- Edge gateway and Unreal**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-E-001 | Implement edge gateway authentication and authorization. | Rendering Platform Engineer | Identity/network | mTLS/OIDC, scope, negative tests. |
| ENG-E-002 | Implement job lease, heartbeat, cancellation, and reconciliation client. | Backend Engineer | Rafa API | Worker death and replay tests. |
| ENG-E-003 | Implement path resolver client and scratch workspace isolation. | Rendering Platform Engineer | D-003 | Traversal, quota, cleanup tests. |
| ENG-E-004 | Implement preflight checks for UE, plugins, assets, GPU, storage, and rights. | Rendering Engineer | UE template/contracts | Fault fixtures and normalized codes. |
| ENG-E-005 | Implement process supervisor with timeout, cancellation, log capture, crash report. | Systems Engineer | E-004 | Kill/hang/OOM simulations. |
| ENG-E-006 | Build atomic UE project and master camera preset. | Unreal TD | Hardware/storage | 20 repeatable renders. |
| ENG-E-007 | Build environment/geospatial adapter. | Unreal Engineer | Approved plugin/version | Coordinate and lighting fixture tests. |
| ENG-E-008 | Build character identity/performance adapter. | Unreal Character TD | Approved asset contract | Continuity and invalid-input tests. |
| ENG-E-009 | Build MRQ profiles and required render passes. | Unreal Rendering Engineer | Contracts | Frame/package conformance. |
| ENG-E-010 | Implement post-render artifact verifier and edge report. | Rendering Platform Engineer | E-009/D-004 | Corruption/missing-frame tests. |

## **EPIC F \- Kestra orchestration**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-F-001 | Deploy development, staging, and production Kestra with RBAC. | Platform Engineer | Infrastructure/IAM | Environment isolation and backups. |
| ENG-F-002 | Create reusable HTTP/auth/retry/task templates. | Kestra Engineer | Contracts | Template tests and documentation. |
| ENG-F-003 | Implement validate-manifest subflow. | Kestra Engineer | Rafa API | Ineligible manifest cannot proceed. |
| ENG-F-004 | Implement structural-render subflow. | Kestra Engineer | Edge gateway | Idempotent success/retry/failure. |
| ENG-F-005 | Implement structural-package verifier subflow. | QA/Kestra Engineer | Artifact verifier | Evidence before success. |
| ENG-F-006 | Implement global failure routing and alerts. | SRE/Kestra Engineer | Error taxonomy | Correct routing by code. |
| ENG-F-007 | Implement stuck-job and orphan-artifact reconciler. | SRE Engineer | Status APIs | Lost callback and partial state tests. |
| ENG-F-008 | Implement concurrency, priority, and budget controls. | Platform/FinOps Engineer | Metrics/costs | Load and budget-stop tests. |

## **EPIC G \- n8n interaction plane**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-G-001 | Deploy n8n environments with SSO/RBAC and credential separation. | Platform Engineer | Identity | Environment and access audit. |
| ENG-G-002 | Build authenticated intake workflow. | n8n Engineer | Rafa API | Validation/rate-limit tests. |
| ENG-G-003 | Build plot/script review gate. | Workflow Product Engineer | Content service | Versioned approve/reject/revise. |
| ENG-G-004 | Build identity/rights review gate. | n8n Engineer | Asset/rights APIs | Ineligible assets blocked. |
| ENG-G-005 | Build shot-plan/cost review gate. | Workflow Engineer | Manifest/cost APIs | Exact version and forecast presented. |
| ENG-G-006 | Build structural review and bounded revision workflow. | n8n Engineer | Kestra events | Changes create impact-scoped new version. |
| ENG-G-007 | Build polish variant presentation and selection. | Product/Workflow Engineer | Provider gateway | Cost and lineage visible. |
| ENG-G-008 | Create workflow export, test, deployment, and rollback procedure. | Release Engineer | Git/CI | Reproducible promotion. |

## **EPIC H \- Provider gateway and AI evaluation**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-H-001 | Implement normalized provider interface and mock provider. | AI Integration Engineer | Contracts | Contract suite passes. |
| ENG-H-002 | Implement first approved video-polish adapter. | AI Integration Engineer | Vendor sandbox | Submit/status/cancel/result/cost tests. |
| ENG-H-003 | Implement durable result downloader and quarantine. | Backend Engineer | Storage | Partial/corrupt result tests. |
| ENG-H-004 | Implement provider capability and pricing registry. | FinOps/AI Engineer | Vendor review | Versioned configuration and forecast. |
| ENG-H-005 | Implement provider circuit breaker, quota, and budget guard. | Platform Engineer | Metrics/cost | Failure/rate/spend tests. |
| ENG-H-006 | Build structural-consistency evaluation fixtures. | ML Evaluation Engineer | UE outputs | Automated metrics plus human rubric. |
| ENG-H-007 | Implement second adapter substitution test. | AI Integration Engineer | H-001 | Same normalized request works without core rewrite. |

## **EPIC I \- Post-production and release**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-I-001 | Implement media probe and decode validator. | Media Systems Engineer | Artifact registry | Pass/fail JSON and fixtures. |
| ENG-I-002 | Implement loudness, channels, and audio integrity validator. | Audio Engineer | QC profiles | Reference tone/silence/failure fixtures. |
| ENG-I-003 | Implement TTS/recorded voice asset and rights workflow. | Audio Product Engineer | Rights/API | Voice lineage and consent enforced. |
| ENG-I-004 | Implement lip-sync provider/local adapter lifecycle. | AI/Post Engineer | Provider gateway | Async and QC tests. |
| ENG-I-005 | Implement canonical timeline schema and approved-master bindings. | Editorial Systems Engineer | Rafa schema | Version and change-impact tests. |
| ENG-I-006 | Implement EDL/FCPXML/OTIO exporters and round-trip suite. | Editorial Systems Engineer | I-005 | Target NLE imports match expected frames. |
| ENG-I-007 | Implement release checklist and package generator. | Release Engineer | QC/rights | Signed evidence and delivery receipt. |
| ENG-I-008 | Implement archive manifest and retention assignment. | Operations Engineer | Storage | Restore and disposition tests. |

## **EPIC J \- Observability, security, and operations**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| ENG-J-001 | Implement end-to-end structured logging and redaction. | Observability Engineer | Service code | Correlation search and secret tests. |
| ENG-J-002 | Implement metrics, traces, dashboards, and SLOs. | SRE Engineer | J-001 | Stage and executive dashboards. |
| ENG-J-003 | Implement project/provider cost attribution. | FinOps Engineer | Cost events | Invoice/sample reconciliation. |
| ENG-J-004 | Implement alert routing and on-call escalation. | SRE Lead | Runbooks | Synthetic alerts reach correct owner. |
| ENG-J-005 | Run threat model and remediate high risks. | Security Lead | System design | Approved threat report and tests. |
| ENG-J-006 | Implement dependency/SBOM/container/secret scanning. | Security Engineer | CI | Blocking policies and exception workflow. |
| ENG-J-007 | Run production penetration and access test. | External/Internal Security | Staging candidate | High/critical findings remediated. |
| ENG-J-008 | Run backup, restore, and disaster game day. | Infrastructure/SRE | Recovery environment | Measured RPO/RTO and remediation. |

# **15\. First eighteen months: engineering staffing and execution**

| Period | Team focus | Indicative staffed capabilities | Required proof |
| :---- | :---- | :---- | :---- |
| Months 0-2 | Architecture, contracts, atomic render, security and storage foundation. | Architect/HoE, TPM, backend/data, SRE, Unreal TD, QA; fractional legal/security. | G0 and early G1 evidence. |
| Months 2-4 | Rafa schema, artifact registry, edge gateway, repeatable UE package. | Add rendering platform, storage/network, developer experience. | G1 and G2. |
| Months 4-7 | Kestra durability, n8n approvals, telemetry, recovery. | Add workflow engineer, observability/SRE, product/producer. | G3 and G4. |
| Months 6-9 | Provider gateway, variants, cost/quality controls. | Add AI integration, ML evaluation, FinOps support. | G5. |
| Months 8-11 | Audio, editorial, QC, release and archive. | Add media systems, post lead, release manager, rights ops. | G6. |
| Months 10-14 | Pilot slate and reliability improvements. | Add production coordinator, support/IT, specialized QA. | G7. |
| Months 14-18 | Multi-project capacity, DR, security hardening, scale plan. | Engineering manager, security, data steward, additional render/post capacity. | G8. |

# **16\. Engineering handoff checklist**

* Every engineer receives the master requirements, this manual, relevant runbooks, contracts, repositories, and a named manager.  
* The manager maps each assigned ticket to requirement, architecture decision, environment, test plan, and evidence location.  
* No employee is asked to infer security, rights, data ownership, or production state behavior from a screenshot or conversation.  
* Work samples and probation goals use the same definition-of-done and gate evidence expected in production.  
* The first company milestone is not a demo. It is a repeatable, private, recoverable, evidence-producing atomic render service.