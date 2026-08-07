# 

# **Rafa Operations Manual**

## ***Five-year physical, administrative, IT, security, facilities, software, procurement, service management, and continuity operating standard***

| Control | Value |
| :---- | :---- |
| Document ID | RAFA-OPS-001 |
| Planning horizon | Five years, with quarterly control review and annual capacity refresh |
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

# **Purpose and operating standard**

This manual defines how the company establishes and administers the infrastructure employees depend on: legal and administrative records, identities, devices, desks, offices, remote work, network, render nodes, NAS, cloud, software, support, procurement, security, maintenance, backup, incident response, and disaster recovery. It is designed to make growth repeatable and to prevent critical infrastructure from existing only in one person's memory.

**Operational rule:** No system is considered production-ready until it has an owner, inventory record, approved configuration, access model, monitoring, backup/recovery plan, maintenance schedule, vendor/support path, cost center, and retirement procedure.

## **Static contents**

* 1\. Operations governance and service model  
* 2\. Site, facilities, desks, studios, safety, and physical security  
* 3\. Identity, employee lifecycle, endpoints, and remote work  
* 4\. Network, internet, VPN, DNS, certificates, and segmentation  
* 5\. Servers, render nodes, NAS, storage, backup, and recovery  
* 6\. Cloud, SaaS, software catalog, licenses, and configuration  
* 7\. Procurement, inventory, finance administration, and vendors  
* 8\. Service desk, incidents, problems, changes, and maintenance  
* 9\. Business continuity, crisis management, and disaster recovery  
* 10\. Five-year operations roadmap, capacity gates, and runbooks

# **1\. Operations governance and service model**

## **1.1 Operations mandate**

* Provide employees and contractors with secure, functional, documented work environments by their authorized start date.  
* Maintain accurate ownership, inventory, configuration, access, support, license, warranty, cost, and lifecycle records.  
* Protect confidential media, personal data, credentials, infrastructure, and intellectual property through layered controls.  
* Operate predictable support, incident, change, maintenance, backup, and recovery processes.  
* Forecast capacity and refresh infrastructure before it constrains delivery or creates unmanaged risk.  
* Retire access, equipment, data, contracts, and services cleanly when people, projects, or vendors leave.

## **1.2 Service catalog**

| Service | Scope | Accountable owner |
| :---- | :---- | :---- |
| Identity and access | SSO, MFA, groups, service identities, privileged access, reviews. | Head of IT/Security |
| Endpoint computing | Laptops/workstations, OS, patching, protection, configuration, support. | Endpoint Administrator |
| Production workstations | GPU nodes, displays, color, drivers, UE/toolchain, performance. | Rendering Operations Lead |
| Network | Internet, switches, Wi-Fi, VLANs, VPN/overlay, firewall, DNS, certificates. | Network Engineer |
| Storage and backup | NAS, shares, quotas, snapshots, replication, archive, restore. | Storage Administrator |
| Cloud and SaaS | Accounts, subscriptions, configuration, integration, cost, renewal. | Cloud/SaaS Administrator |
| Collaboration | Email, calendar, documents, chat, meetings, knowledge base. | IT Operations |
| Service desk | Requests, incidents, problems, changes, asset issues, onboarding/offboarding. | Service Desk Lead |
| Facilities | Office/studio, desks, power, cooling, access, safety, maintenance. | Facilities/Operations Manager |
| Business continuity | Continuity plans, contacts, recovery priorities, exercises. | COO/BCM Owner |
| Procurement and vendors | Requests, quotes, orders, receiving, invoices, contracts, renewals. | Procurement/Finance Ops |
| Records administration | Corporate, HR, financial, contract, rights, and operational records. | Operations/Legal/People Ops |

## **1.3 Service tiering**

| Tier | Examples | Availability/support | Recovery expectation |
| :---- | :---- | :---- | :---- |
| Tier 0 \- Safety/legal | Emergency communications, building access, legal/HR records. | Immediate response by designated owners. | Alternate manual process and secure copies. |
| Tier 1 \- Production critical | Rafa DB/API, network, NAS, render control, identity, backups. | Monitored; on-call during production windows. | Defined RPO/RTO and tested restoration. |
| Tier 2 \- Business critical | Email, collaboration, finance, project management, source control. | Business-hours support with escalation. | Vendor/admin recovery and export strategy. |
| Tier 3 \- Standard | Individual productivity applications and peripheral services. | Service desk during published hours. | Replace/reinstall from standard configuration. |
| Tier 4 \- Experimental | Research sandboxes and uncommitted tools. | Best effort; no production dependency. | May be recreated or retired. |

## **1.4 Operations responsibility model**

| Activity | Responsible | Accountable | Consulted | Evidence |
| :---- | :---- | :---- | :---- | :---- |
| User onboarding | IT \+ People Ops | Hiring manager | Security/Facilities | Day-one readiness checklist. |
| Privileged access | IT/Security | System owner | Manager | Approved access request and review. |
| Production change | Service owner | Change authority | Security/Ops/Users | Change and rollback record. |
| Incident response | On-call responders | Incident Commander | Owners/Legal/Comms | Incident timeline and postmortem. |
| Asset purchase | Procurement | Budget owner | IT/Security/Finance | PO, receipt, inventory. |
| Backup/restore | Infrastructure | Service owner | Security/Data owner | Backup and restore test. |
| Facilities safety | Facilities | COO | Employees/Emergency services | Inspection and drill. |
| Vendor renewal | Vendor owner | Budget owner | Security/Legal/Finance | Annual review and decision. |

# **2\. Site, facilities, desks, studios, safety, and physical security**

## **2.1 Site selection requirements**

| Dimension | Requirement | Validation |
| :---- | :---- | :---- |
| Power | Adequate circuits for GPU workstations, storage, network, displays, cooling, and growth; labeled and documented. | Licensed electrical assessment and load map. |
| Cooling | Thermal capacity supports sustained render load, not only office occupancy. | Heat-load calculation and temperature logging. |
| Network | At least two viable internet options where business continuity requires; structured cabling and secure equipment area. | Carrier survey and cabling plan. |
| Physical security | Controlled entry, visitor process, secure equipment/storage area, key/badge administration. | Security assessment. |
| Fire/life safety | Code-compliant alarms, extinguishers, egress, occupancy, emergency plans. | Inspection and posted plan. |
| Ergonomics | Adjustable furniture, appropriate displays, lighting, noise control, accessibility. | Workspace assessment. |
| Insurance/lease | Permitted use, equipment value, data/media operations, access, restoration, liability. | Legal/insurance review. |
| Expansion | Space/power/network can accommodate forecast or has a defined alternate site/remote strategy. | Five-year capacity scenario. |

## **2.2 Workspace standards by role**

| Role/workspace | Minimum setup | Purpose |
| :---- | :---- | :---- |
| Standard office/remote employee | Managed laptop, dock, two monitors or approved equivalent, keyboard/mouse, headset, webcam, surge protection, ergonomic chair/desk. | General productivity and secure access. |
| Software engineer | High-memory development machine or remote environment, dual monitors, hardware security key, test access. | Compilation, containers, local fixtures. |
| Unreal/render developer | Approved GPU workstation, high-refresh/color-capable displays as required, local NVMe scratch, 25/10GbE where justified, UPS. | Interactive development and test renders. |
| Color/post-production | Calibrated reference-capable display, monitoring/audio, control surface if required, high-throughput storage network, quiet environment. | Conform, color, audio, QC. |
| Producer/project manager | Portable managed device, reliable meeting setup, project dashboards, secure external presentation. | Coordination and client review. |
| Facilities/service desk bench | Spare devices, diagnostics, secure imaging area, ESD protection, labeled parts and loaners. | Provisioning and repair. |
| Shared review room | Approved display/projector, calibrated audio where relevant, conferencing, controlled access, no persistent local sensitive media. | Group creative and release review. |

## **2.3 Desk deployment checklist**

* Record desk/room ID, assigned user or shared purpose, furniture, displays, docks, peripherals, network ports, and power circuit.  
* Verify adjustable chair/desk range, monitor height, cable management, lighting, ventilation, trip hazards, and accessibility needs.  
* Label company-owned equipment with asset tag and inventory record before assignment.  
* Connect endpoints only to the correct network zone; unmanaged personal devices use guest network.  
* Test audio/video, power recovery, network throughput, storage access, and required software.  
* Provide secure lock/storage where physical media or restricted equipment may be handled.  
* Photograph or diagram shared production desk configuration for reproducible restoration.  
* Obtain user sign-off or record exceptions and remediation owner.

## **2.4 Production equipment room standard**

* Restricted badge/key access; visitor log and escort.  
* Dedicated, labeled power distribution; UPS sizing based on actual load and safe shutdown; no unsupported daisy chaining.  
* Environmental monitoring for temperature, humidity, smoke/water as appropriate, and alerts routed to owners.  
* Rack and cable labeling at both ends; current rack elevation, port map, circuit map, and device inventory.  
* Clear separation of management, storage, production, and external cabling; spare capacity documented.  
* No combustible storage, drinks, or unrelated materials; documented maintenance access and safe lifting.  
* Backup configuration and recovery access stored outside the room under controlled conditions.

## **2.5 Visitor and physical access procedure**

1. Host submits visitor name, organization, purpose, date/time, areas required, and confidentiality status.  
2. Reception/host verifies authorization and any NDA or safety briefing.  
3. Issue time-bounded visitor credential; visitors remain escorted in restricted areas.  
4. Prohibit photography, recording, removable media, and access to confidential material unless explicitly approved.  
5. Collect credential and record departure. Escalate lost badge/key immediately.  
6. Review employee and vendor physical access quarterly and revoke on role or contract change.

## **2.6 Safety and emergency minimums**

| Scenario | Immediate action | Owner preparation |
| :---- | :---- | :---- |
| Fire/smoke | Evacuate, call emergency services, do not fight unsafe fire, account at assembly point. | Egress map, alarms, extinguishers, drills. |
| Medical emergency | Call emergency services, provide trained first aid/AED support if available. | Contacts, supplies, trained volunteers. |
| Electrical/UPS issue | Isolate area if safe; do not touch damaged/high-voltage equipment; contact qualified service. | Circuit labels, shutdown procedure, vendor. |
| Water leak | Protect people first; isolate power if safe; stop source; move portable assets only if safe. | Sensors, shutoff location, recovery supplies. |
| Extreme heat/cooling failure | Stop or throttle equipment per runbook; protect data and hardware. | Temperature alerts, graceful shutdown order. |
| Security threat | Follow law enforcement/building direction; do not confront; preserve evidence. | Emergency communications and access lockdown. |

# **3\. Identity, employee lifecycle, endpoints, and remote work**

## **3.1 Joiner-mover-leaver control**

| Stage | Trigger | Required actions | Evidence |
| :---- | :---- | :---- | :---- |
| Pre-hire | Accepted offer and cleared conditions. | Create onboarding ticket, role/access profile, device/workspace order, manager/buddy, start schedule. | Readiness plan. |
| Joiner | Authorized start date. | Identity, MFA, device, baseline groups, software, facilities, training; no excess access. | Day-one checklist. |
| Mover | Approved role/project/location change. | Re-evaluate groups, privileged roles, licenses, devices, data, physical access. | Access delta and manager approval. |
| Temporary elevation | Approved support/change/incident need. | Just-in-time role, time limit, ticket, monitoring, automatic revocation. | Elevation log. |
| Leave/absence | Approved leave or legal/HR trigger. | Apply access, communication, delegation, device, and confidentiality policy. | People/IT record. |
| Leaver | Termination/contract end. | Disable identity, revoke sessions/tokens, recover assets, rotate shared secrets, transfer ownership, preserve/delete records. | Offboarding attestation. |

## **3.2 Access request workflow**

7. Requester selects system, role, business purpose, project, data classification, duration, and approving manager/system owner.  
8. Policy engine or IT checks employment/contract status, mandatory training, role compatibility, segregation of duties, and license availability.  
9. System/data owner approves least-privilege role; security/privacy approval is added for restricted or privileged access.  
10. Provision through group/role automation where possible; record exact entitlements and expiration.  
11. Notify user of permitted use, handling, support, and review obligations.  
12. Review at the defined cadence and on role/project change; revoke unused or unjustified access.

## **3.3 Identity baseline**

| Control | Standard |
| :---- | :---- |
| Primary identity | One unique company identity per person; aliases do not create independent authorization. |
| MFA | Required for all company services; phishing-resistant hardware/passkey methods for privileged and high-risk roles where supported. |
| Password | Use identity-provider and password-manager policy; prohibit reuse and shared credentials. |
| Groups | Role/project/system groups with named owner and purpose; avoid direct individual grants where possible. |
| Privileged access | Separate/admin role or account, just-in-time elevation, strong MFA, ticket and audit. |
| Service identities | Non-human, single purpose, owner, scoped permissions, rotation, no interactive login unless required. |
| Break glass | Minimal emergency identities, offline recovery credentials, monitored use, quarterly test. |
| Review | Quarterly for privileged/restricted access; at least semiannual for standard access; immediate on mover/leaver. |

## **3.4 Endpoint standards**

| Area | Minimum standard |
| :---- | :---- |
| Procurement | Approved models with business support, warranty, parts availability, security capability, and known driver/tool compatibility. |
| Enrollment | Device management before user handoff; unique asset record and assigned owner. |
| Encryption | Full-disk encryption with recovery key escrow under controlled access. |
| Authentication | Company identity, MFA, screen lock, no shared local accounts; local admin removed by default. |
| Protection | Endpoint detection/anti-malware, host firewall, exploit protections, device control per risk. |
| Patching | OS/browser/security patches within policy; production GPU/driver changes use compatibility and maintenance windows. |
| Configuration | Baseline policies, approved applications, logging, time sync, browser controls, secure remote support. |
| Data | Company data in approved storage; local restricted media minimized, encrypted, and cleared through lifecycle. |
| Backup | User files synchronized to approved service; local system is replaceable. |
| Disposal | Secure wipe/crypto erase, verification, inventory update, certified recycling or return. |

## **3.5 Endpoint provisioning procedure**

Provisioning ticket:  
  employee\_id:  
  start\_date:  
  manager:  
  location:  
  role\_profile:  
  device\_profile:  
  peripherals:  
  software\_profile:  
  required\_project\_access:  
  restricted\_data\_access:  
  accommodations:  
  shipping\_or\_desk:  
  approvers:  
  assigned\_technician:  
  readiness\_due:

13. Receive approved ticket with sufficient lead time and confirm stock, shipping, workspace, and identity prerequisites.  
14. Reserve and asset-tag device; update inventory with serial, warranty, assignment, cost center, and lifecycle date.  
15. Apply approved firmware and OS image, device management, encryption, security, patch, and configuration baselines.  
16. Install role-based software from managed catalog; do not embed production credentials.  
17. Run automated compliance and hardware diagnostics; record results.  
18. Prepare MFA/hardware keys and sealed recovery instructions according to policy.  
19. Deliver securely; verify recipient; guide first login and MFA; confirm support path.  
20. Obtain acknowledgement, close readiness evidence, and schedule early compliance check.

## **3.6 Remote work standard**

* Use managed company equipment for company work unless a written exception is approved.  
* Use company VPN/zero-trust access for private services; do not expose services through home router port forwarding.  
* Protect screens and conversations from household/public observation; use headset and privacy measures for confidential work.  
* Store company data only in approved services; removable media requires approval and encryption.  
* Maintain safe, ergonomic workspace and reliable internet/power appropriate to the role; report constraints before production commitments.  
* Travel or public networks require VPN and heightened care; never leave equipment unattended or in checked luggage where avoidable.  
* Report loss, theft, suspected compromise, or accidental disclosure immediately through the incident channel.

# **4\. Network, internet, VPN, DNS, certificates, and segmentation**

## **4.1 Network architecture standard**

Internet Provider A \----+  
                        \+---- \[Firewall pair / approved gateway\]  
Internet Provider B \----+          |  
                                   \+-- VLAN 10 Corporate  
                                   \+-- VLAN 20 Management  
                                   \+-- VLAN 30 Production Services  
                                   \+-- VLAN 40 Render Workers  
                                   \+-- VLAN 50 Storage Data  
                                   \+-- VLAN 60 Guest / Untrusted  
                                   \+-- VLAN 70 Recovery  
                                   \+-- VLAN 80 Voice/Facilities as needed

Private cloud/remote workers connect through approved identity-aware VPN/overlay.  
Default inter-zone policy: deny.

## **4.2 Network build procedure**

21. Inventory current and five-year endpoint, render, storage, cloud, remote, voice, facility, and guest demand.  
22. Create logical and physical diagrams, IP plan, VLANs, DHCP/DNS/NTP, routing, firewall policy, management plane, and certificate approach.  
23. Select business-supported firewall, switches, Wi-Fi, optics/cabling, UPS, monitoring, and spare strategy.  
24. Stage configuration offline, back it up, review management access, and test failover before cutover.  
25. Label racks, switches, ports, cables, wall jacks, circuits, and uplinks; record in inventory/configuration system.  
26. Validate segmentation, throughput, latency, MTU, storage paths, internet failover, VPN, monitoring, logging, and guest isolation.  
27. Publish support contacts, diagrams, configurations, recovery access, maintenance, and change procedure.

## **4.3 Firewall rule record**

Rule ID:  
Business service:  
Source zone/identity:  
Destination zone/service:  
Protocol/port:  
Direction:  
Purpose:  
Data classification:  
Owner:  
Approver:  
Logging:  
Start date:  
Expiration/review date:  
Test:  
Rollback:

## **4.4 DNS and certificate rules**

* Use centrally administered internal and public DNS with named zone owners and change logging.  
* Production services use stable service names; do not embed raw IPs in workflows or documents.  
* Certificates are issued by approved internal/public authorities, inventoried, automatically renewed where possible, and alerted before expiry.  
* Private keys are generated and stored in approved systems; never sent through chat or documents.  
* Certificate revocation/rotation procedure is tested, including edge devices and service clients.  
* Split-horizon or private DNS designs must be documented and included in disaster recovery.

## **4.5 25/10GbE storage/render acceptance**

| Test | Method | Pass evidence |
| :---- | :---- | :---- |
| Link | Verify negotiated speed, optics/DAC compatibility, errors, MTU. | Interface report and zero unexpected errors. |
| Throughput | Measure sustained sequential read/write with representative file sizes and safe test location. | Benchmark vs approved floor. |
| Latency | Measure metadata and small-file behavior relevant to frame sequences. | p50/p95 and comparison. |
| Concurrency | Simulate expected render/read/QC clients without saturating critical path. | Capacity test and headroom. |
| Failure | Disconnect link/port and validate failover or graceful failure. | No corruption; alerts and recovery. |
| Persistence | Validate application close, storage flush, checksum, snapshot/backup. | End-to-end checksum evidence. |

# **5\. Servers, render nodes, NAS, storage, backup, and recovery**

## **5.1 Hardware lifecycle**

| Lifecycle stage | Required controls |
| :---- | :---- |
| Plan | Workload, performance, compatibility, security, power/cooling, warranty, support, depreciation, disposal. |
| Approve | Business case, budget, architecture/security review, owner, location, lifecycle. |
| Receive | Verify order/condition, serials, asset tag, warranty, inventory, secure staging. |
| Build | Firmware, baseline config, identity, encryption, monitoring, backup, tests, documentation. |
| Operate | Health, capacity, patches, maintenance, spare/repair, warranty, configuration backup. |
| Refresh | Forecast performance/risk, migrate, validate, update inventory and financial records. |
| Retire | Remove service/data/access, sanitize, confirm backup/records, dispose/return, close contract. |

## **5.2 Render node baseline**

| Component | Baseline requirement |
| :---- | :---- |
| Hardware | Approved CPU/GPU/RAM/storage/PSU/cooling; serials and firmware recorded; spare/recovery path. |
| OS | Managed supported OS, time sync, full-disk encryption, service account, host firewall, endpoint protection. |
| Drivers | Pinned approved GPU and device drivers; benchmark and rollback package. |
| Software | Approved UE/project versions, plugins, edge gateway, monitoring, storage client; license compliance. |
| Network | Render VLAN, private management, storage path, no inbound public access. |
| Storage | Local scratch separated from authoritative NAS; quotas, cleanup, low-space alerts. |
| Execution | Restricted service account, allowlisted commands, process supervision, logs/crash capture. |
| Recovery | Rebuild from documented image/configuration; configuration and asset dependencies in source control. |

## **5.3 Reference Windows render-node bootstrap**

\# Reference only: execute through approved configuration management.  
$ErrorActionPreference \= "Stop"

$RequiredDirs \= @(  
  "D:\\Rafa\\Scratch",  
  "D:\\Rafa\\Logs",  
  "D:\\Rafa\\Config"  
)  
foreach ($dir in $RequiredDirs) {  
  New-Item \-Path $dir \-ItemType Directory \-Force | Out-Null  
}

\# Set ACLs to the dedicated render service identity.  
\# Do not use broad Everyone/Users write permissions.  
icacls "D:\\Rafa" /inheritance:r  
icacls "D:\\Rafa" /grant "RAFA\\svc-render:(OI)(CI)M"  
icacls "D:\\Rafa" /grant "RAFA\\RenderAdmins:(OI)(CI)F"

\# Install signed edge-gateway release from internal artifact repository.  
\# Verify signature and SHA-256 before installation.  
\# Register service with automatic delayed start and recovery actions.  
\# Enroll logging, metrics, EDR, patch, and configuration management.

## **5.4 NAS share and ACL model**

| Share/prefix | Writers | Readers | Special controls |
| :---- | :---- | :---- | :---- |
| Source | Asset ingestion service and authorized stewards. | Assigned project services/users. | Quarantine first; provenance/rights before release. |
| Manifests | Rafa artifact service. | Workflow and assigned workers. | Immutable approved versions. |
| Structural | Assigned render service identity. | Post/QC/assigned teams. | Per-project/per-job write scope; snapshots. |
| Polish | Provider gateway. | Post/QC/assigned teams. | Quarantine then validation. |
| Masters | Release service/authorized post lead. | Restricted distribution roles. | Immutable, checksum, retention, backup priority. |
| Delivery | Release service. | Authorized delivery operators/client mechanism. | Time-bounded links, receipt, expiry. |
| System | Infrastructure/service owners. | Services by need. | No general user access. |
| Recovery | Backup service only. | Recovery admins under procedure. | Immutable/offline protections. |

## **5.5 Backup policy**

| Data class | Primary | Snapshot/version | Secondary/offsite | Validation |
| :---- | :---- | :---- | :---- | :---- |
| Rafa database | Production database. | Continuous/PITR and daily backups. | Encrypted independent repository. | Quarterly PITR and schema reconcile. |
| Approved source/assets | NAS governed share. | Frequent snapshots/versioning. | Offsite/immutable copy by class. | Sample monthly; project quarterly. |
| Structural/intermediate | NAS project share. | Snapshot during active production. | Retention based on reproducibility/cost. | Checksum and restore sample. |
| Approved masters/delivery | NAS master/archive. | Immutable snapshot. | Offsite and, where justified, second medium/account. | Quarterly restore; annual full project. |
| Configurations/workflows | Git/config backup. | Every approved change. | Independent organization/export. | Rebuild staging. |
| Employee endpoints | Approved cloud/company storage. | File versioning. | Service backup if required. | User/file restore sample. |

## **5.6 NAS restore procedure**

28. Open restore ticket; record requester, project/assets, point in time, reason, urgency, classification, and approval.  
29. Confirm whether legal hold, current production writes, or ransomware suspicion changes the recovery path.  
30. Identify authoritative backup/snapshot and verify backup health before restoration.  
31. Restore to isolated recovery location first; do not overwrite production automatically.  
32. Validate checksums, file counts, permissions, metadata, and required application behavior.  
33. Obtain data/service owner approval; move or reconcile through controlled change.  
34. Record elapsed time, RPO achieved, evidence, issues, and cleanup; update recovery metrics and remediation.

# **6\. Cloud, SaaS, software catalog, licenses, and configuration**

## **6.1 Software approval categories**

| Category | Process |
| :---- | :---- |
| Standard approved | Available through managed catalog; license and support already approved. |
| Role approved | Provisioned automatically or by request for defined roles/projects. |
| Restricted | Requires security/privacy/legal/data review and named purpose. |
| Experimental | Sandbox only, no production/client/restricted data, time-bounded owner and cost. |
| Prohibited | Known unacceptable security, rights, licensing, data use, or support risk. |
| Retiring | No new adoption; migration and removal date published. |

## **6.2 New software/SaaS request**

35. Requester documents problem, users, data types, integrations, administrator, cost, alternatives, and urgency.  
36. IT evaluates compatibility, deployment, support, endpoint/network impact, identity/SSO, logging, export, and lifecycle.  
37. Security/privacy/legal evaluate permissions, data use, training, subprocessors, terms, retention, deletion, and incident response.  
38. Finance/procurement validate price, commitment, renewal, utilization, vendor, and contract authority.  
39. Owner completes sandbox pilot with synthetic/approved data and success criteria.  
40. Approvers assign category, restrictions, configuration standard, support owner, license pool, review, and exit plan.  
41. IT deploys and documents; inventory and renewal calendars are updated.

## **6.3 Software catalog record**

software\_id:  
name:  
publisher:  
category:  
business\_owner:  
technical\_owner:  
approved\_use:  
prohibited\_use:  
data\_classes\_allowed:  
identity\_and\_mfa:  
admin\_roles:  
license\_model:  
cost\_center:  
renewal\_date:  
version\_policy:  
deployment\_method:  
configuration\_baseline:  
logging\_and\_backup:  
support\_and\_vendor:  
export\_exit\_plan:  
review\_date:

## **6.4 License management**

* Assign named licenses through groups when possible and reclaim promptly on role change or inactivity.  
* Maintain purchased, assigned, available, renewal, true-up, and utilization records.  
* Prohibit shared named-user licenses or credentials where terms do not permit.  
* Track production plugins, fonts, codecs, stock, models, and content licenses—not only office software.  
* Review auto-renewals at least 90 days before commitment and test data export/transition for critical SaaS.  
* Reconcile vendor invoices to license inventory and authorized users.

## **6.5 Cloud account/project baseline**

| Control | Requirement |
| :---- | :---- |
| Organization | Central company ownership; no production resources in personal accounts. |
| Environments | Separate development, staging, production, and recovery accounts/projects where practical. |
| Identity | SSO/workload identities, least privilege, no shared root; break-glass controlled. |
| Network | Private services, controlled egress, firewall/security groups, logging, DNS/certificates. |
| Secrets | Managed secret store and rotation; no plaintext environment files in repositories. |
| Logging | Admin, identity, network, storage, and service logs centralized and retained by policy. |
| Cost | Budgets, tags/labels, alerts, committed spend register, project attribution. |
| Backup | Configuration/IaC in source; data backup/restore independent of the primary failure domain. |
| Policy | Approved regions, services, data classes, encryption, public exposure, and exception process. |

# **7\. Procurement, inventory, finance administration, and vendors**

## **7.1 Purchase-to-pay procedure**

42. Requester submits business purpose, specification, quantity, required date, cost center/project, vendor, quote, security/data relevance, and approver.  
43. IT/Operations validates standardization, compatibility, support, lifecycle, storage/power/network, and inventory implications.  
44. Security/legal/privacy review applies where systems, data, contracts, or rights are involved.  
45. Budget owner and finance approve within delegation; procurement obtains competitive quotes or records sole-source rationale.  
46. Issue purchase order or authorized commitment; no employee accepts unapproved auto-renewing terms.  
47. Receive and inspect goods/services; asset-tag and inventory before assignment; record license/contract/warranty.  
48. Match invoice to PO/contract and receipt; resolve variance; approve payment.  
49. Review utilization, performance, renewal, and retirement over lifecycle.

## **7.2 Asset inventory minimum**

| Field group | Required fields |
| :---- | :---- |
| Identity | Asset tag, serial, type, make/model, hostname, unique cloud/SaaS identifier. |
| Ownership | Business owner, technical owner, assigned person/service, cost center, project. |
| Location | Site/room/rack/desk or remote custodian; network zone. |
| Financial | Vendor, PO, purchase/lease date, cost, depreciation/expense class. |
| Support | Warranty, support contract, renewal, vendor contact, replacement path. |
| Configuration | OS/firmware/version, management enrollment, baseline, IP/interface where appropriate. |
| Security/data | Classification, encryption, privileged roles, backup, exposure, criticality. |
| Lifecycle | In-service date, review/refresh, status, retirement, wipe/disposal evidence. |

## **7.3 Spare and loaner policy**

* Maintain minimum spares based on failure rate, procurement lead time, role criticality, and number of standardized models.  
* Loaners are managed, encrypted, patched, inventoried, time-bounded, and returned through a wipe/revalidation process.  
* High-cost production spares have storage, battery/firmware maintenance, and quarterly functional test.  
* Parts removed from service are labeled with status; failed drives/media remain controlled until sanitized or destroyed.  
* Do not cannibalize production systems without updating inventory, configuration, warranty, and recovery capacity.

## **7.4 Vendor record**

vendor\_id:  
legal\_name:  
service\_or\_goods:  
business\_owner:  
technical\_owner:  
criticality:  
data\_media\_access:  
contract\_start\_end:  
renewal\_notice:  
annual\_commitment:  
payment\_terms:  
support\_and\_escalation:  
security\_privacy\_legal\_reviews:  
insurance\_and\_compliance:  
subprocessors\_or\_dependencies:  
business\_continuity:  
exit\_and\_data\_return:  
performance\_score:  
last\_review:  
next\_review:

# **8\. Service desk, incidents, problems, changes, and maintenance**

## **8.1 Request and incident severity**

| Priority | Definition | Initial response target | Examples | Escalation |
| :---- | :---- | :---- | :---- | :---- |
| P1 Critical | Safety, security, legal, or widespread production halt with no workaround. | Immediate/on-call. | Rafa/NAS unavailable, active compromise, major data loss. | Incident Commander, executives, legal/security. |
| P2 High | Major user/project impact or key production function degraded. | Within published urgent target. | Render queue down, critical employee unable to work. | Service owner and operations leader. |
| P3 Normal | Limited impact with workaround; standard request. | Business-hours service target. | Software issue, access request, peripheral failure. | Service desk/owner. |
| P4 Planned | Information, enhancement, future provisioning. | Scheduled. | New workspace, non-urgent software, documentation. | Queue and capacity planning. |

## **8.2 Ticket requirements**

* Requester/contact and affected users/projects.  
* Service, asset, location, environment, and data classification.  
* Observed behavior, expected behavior, start time, frequency, business impact, workaround.  
* Screenshots/logs only after sensitive information is reviewed/redacted.  
* Priority based on impact and urgency, not title of requester.  
* Owner, status, next action, customer communication, resolution, evidence, and closure confirmation.

## **8.3 Incident command procedure**

50. Detect and declare incident; assign severity, Incident Commander, technical lead, communications lead, and scribe.  
51. Protect people and data; contain active harm; preserve evidence; use approved emergency access.  
52. Establish incident channel/bridge and timeline; identify affected services, customers, projects, and obligations.  
53. Communicate on published cadence using facts, impact, actions, workaround, and next update—avoid speculation.  
54. Diagnose and mitigate; prefer reversible containment; record changes and decisions.  
55. Validate restoration, data integrity, security, queue/state reconciliation, and customer impact before closing.  
56. Complete post-incident review with root causes, contributing factors, control gaps, owners, due dates, and learning.

## **8.4 Change classes**

| Class | Examples | Approval and control |
| :---- | :---- | :---- |
| Standard | Pre-approved repeatable low-risk change. | Documented procedure, automated checks, logged execution. |
| Normal | Planned service/configuration/hardware change. | Risk/impact/test/rollback, owner approval, schedule and communication. |
| Major | Architecture, broad access, data migration, network/storage, critical upgrade. | Change board/architecture/security, rehearsal, extended monitoring. |
| Emergency | Urgent incident mitigation. | Incident authority, smallest reversible change, complete record and retrospective. |

## **8.5 Change record**

change\_id:  
service\_and\_owner:  
requester:  
reason:  
scope:  
affected\_users\_projects\_data:  
risk\_and\_impact:  
security\_privacy\_rights:  
prerequisites:  
implementation\_steps:  
validation:  
rollback:  
maintenance\_window:  
communications:  
approvers:  
executor:  
actual\_start\_end:  
result:  
evidence:  
follow\_up:

## **8.6 Problem management**

57. Create a problem record for recurring incidents, unknown high-impact cause, or material near miss.  
58. Analyze chronology, technical and organizational causes, detection, control failures, and why impact was not limited.  
59. Create known error and workaround when permanent remediation is not immediate.  
60. Prioritize remediation by risk and recurrence; track through the same change and evidence process.  
61. Review effectiveness after implementation and close only when recurrence risk is demonstrably reduced or accepted.

## **8.7 Maintenance calendar**

| Frequency | Activities |
| :---- | :---- |
| Daily/continuous | Monitoring, backups, security alerts, queue/capacity, facility environmental alerts, critical tickets. |
| Weekly | Failed backups, patch exceptions, storage growth, license/credential alerts, inventory changes, open P1/P2 actions. |
| Monthly | OS/application patch cycle, restore sample, access anomalies, vendor/service health, spare check, UPS/environment review. |
| Quarterly | Privileged access review, full restore drill sample, network/firewall review, hardware health, emergency contacts, facility inspection. |
| Semiannual | Standard access review, software utilization, workstation benchmark/sample, internet failover, security awareness refresh. |
| Annual | DR exercise, vendor renewal review, insurance/asset valuation, lifecycle plan, penetration/security assessment as required, site safety drill. |

# **9\. Business continuity, crisis management, and disaster recovery**

## **9.1 Business impact priorities**

| Priority | Business process | Minimum continuity capability |
| :---- | :---- | :---- |
| 1 | People safety, emergency communication, payroll/legal obligations. | Contacts, alternate communications, secure records, authorized deputies. |
| 2 | Identity, Rafa data, NAS masters/source, network, core production coordination. | Recovery identities, backups, alternate access/site, restore runbooks. |
| 3 | Active client production and delivery. | Prioritized project list, alternate compute/provider/post paths, customer communication. |
| 4 | Sales, finance operations, collaboration, normal support. | SaaS continuity/export, remote work, manual procedures. |
| 5 | Research, experimental services, noncritical amenities. | Pause and recreate after core recovery. |

## **9.2 Scenario plans**

| Scenario | Continuity response | Owner |
| :---- | :---- | :---- |
| Primary office unavailable | Remote work activation, building contacts, equipment access restrictions, alternate review/production locations. | COO/Facilities |
| Internet outage | Secondary circuit/hotspot for critical coordination, local production continuation where safe, customer communication. | Network Lead |
| NAS outage | Stop authoritative writes, fail to approved replica/recovery only under procedure, reconcile after restore. | Infrastructure Lead |
| Ransomware/suspected compromise | Isolate, preserve evidence, invoke security incident, protect backups, do not restore into compromised environment. | Security Incident Commander |
| Render node loss | Requeue to spare/alternate node or cloud path, verify compatibility, update capacity forecast. | Rendering Operations |
| Provider outage/terms change | Circuit-break adapter, alternate provider/local/manual path, pause or obtain approval for changed result/cost. | AI Platform/Producer |
| Key person unavailable | Deputy, documented access/runbook, vendor support, executive prioritization. | Function leader |
| Cloud/SaaS outage | Vendor status/escalation, exported critical records, alternate communication/manual procedure. | Service owner |
| Data loss/corruption | Freeze affected writes, identify point in time, isolated restore, reconcile lineage and outputs. | Data/Infrastructure |

## **9.3 Disaster declaration and recovery**

62. Authorized leader declares disaster or recovery event, scope, priorities, and command structure.  
63. Confirm safety and active security threat; preserve evidence before restoration.  
64. Select recovery point and environment based on RPO, integrity, and business priority.  
65. Restore identity, network, secrets, Rafa database, storage metadata/assets, orchestration, then dependent services in documented order.  
66. Validate each service with technical and business owner tests; reconcile in-flight jobs and external provider states.  
67. Communicate status and limitations; resume prioritized work under capacity controls.  
68. Capture actual RPO/RTO, decisions, missing dependencies, costs, and remediation; schedule retest.

## **9.4 Recovery dependency order**

1\. Emergency contacts and command communications  
2\. Identity provider, MFA, break-glass, device access  
3\. DNS, certificates, private network/VPN, firewall  
4\. Secret manager and configuration sources  
5\. Rafa PostgreSQL and audit integrity  
6\. NAS/object storage metadata and priority assets  
7\. Rafa API and artifact resolver  
8\. Kestra/n8n and message/event paths  
9\. Edge gateway and render nodes  
10\. Provider gateway, media QC, post-production  
11\. Project/user services and normal business operations

# **10\. Five-year operations roadmap**

| Year | Theme | Primary proof |
| :---- | :---- | :---- |
| Year 1 \- Baseline and control | Inventory, identity, endpoint baseline, segmented network, NAS/backup, service desk, joiner/leaver, vendors, atomic production operations. | 100% critical assets/services owned and documented; first restore and incident exercises. |
| Year 2 \- Repeatable multi-project operations | Standard device/desk profiles, capacity planning, on-call, configuration management, stronger license/procurement, project isolation. | Predictable onboarding/support and production SLOs. |
| Year 3 \- Automation and scale | Automated provisioning, access reviews, patch compliance, storage lifecycle, FinOps, self-service catalog, service analytics. | Reduced manual lead time and measurable unit cost. |
| Year 4 \- Resilience and geographic/remote maturity | Alternate site/remote production capability, independent backups, provider/cloud failover, leadership deputies, advanced security. | Annual DR and business continuity objectives pass. |
| Year 5 \- Mature operations platform | Auditable control system, lifecycle refresh, multi-team service management, continuous improvement, selective external assurance. | Operations supports growth without hidden single points of failure. |

## **10.1 Capacity planning horizons**

| Horizon | Review |
| :---- | :---- |
| Weekly | Active project load, render queue, storage headroom, provider quota, staffing, incidents. |
| Monthly | Utilization, growth trend, workstation/NAS/network health, license pools, spare stock, cloud spend. |
| Quarterly | Pipeline forecast, project slate, hiring, procurement lead times, power/cooling, backup volume, vendor limits. |
| Annual | Five-year scenarios, site strategy, network/storage refresh, contracts, insurance, DR, lifecycle capital. |

# **11\. Operations task catalog**

| Task ID | Assignable work | Primary role | Dependencies | Definition of done |
| :---- | :---- | :---- | :---- | :---- |
| OPS-001 | Create service catalog and tier every service. | Operations Manager | Business/engineering inventory | Owner, criticality, SLO, support, dependencies, recovery. |
| OPS-002 | Build identity groups, role profiles, joiner/mover/leaver automation. | IAM Administrator | People/role data | Access tests and quarterly review. |
| OPS-003 | Standardize endpoint profiles and configuration management. | Endpoint Administrator | Approved hardware/software | Rebuild and compliance evidence. |
| OPS-004 | Design and deploy network zones and firewall policy. | Network Engineer | Site/security design | Segmentation and failover tests. |
| OPS-005 | Deploy NAS share/ACL/snapshot/backup standard. | Storage Administrator | Data model | Isolation, checksum, restore evidence. |
| OPS-006 | Create render-node build and maintenance baseline. | Rendering Operations | Engineering template | Node rebuild and benchmark. |
| OPS-007 | Implement asset inventory and purchase-to-pay workflow. | Procurement/IT Ops | Finance system | PO-to-assignment traceability. |
| OPS-008 | Implement software catalog and license reconciliation. | SaaS Administrator | Vendor inventory | Utilization and renewal control. |
| OPS-009 | Launch service desk, priority, SLA, escalation, and knowledge base. | Service Desk Lead | Service catalog | Ticket metrics and user communication. |
| OPS-010 | Implement incident, problem, and change management. | SRE/Operations | Service owners | Game-day and postmortem evidence. |
| OPS-011 | Build facilities, desk, physical access, safety, and visitor procedures. | Facilities Manager | Site | Inspection and onboarding readiness. |
| OPS-012 | Implement backup/restore schedule and recovery environment. | Infrastructure Lead | Critical services | Quarterly and annual tests. |
| OPS-013 | Create vendor due diligence and renewal calendar. | Vendor Manager | Legal/security/finance | No unmanaged renewals. |
| OPS-014 | Create five-year lifecycle and capital plan. | COO/Finance/IT | Inventory/capacity | Refresh and risk forecast. |
| OPS-015 | Run annual full business continuity exercise. | BCM Owner | Scenario plans | Executive report and closed remediation. |

# **12\. Runbooks and templates**

## **12.1 Day-one readiness**

Employee:  
Role/profile:  
Start date/time zone/site:  
Manager/buddy:  
Identity and MFA:  
Device/peripherals:  
Desk/shipping:  
Software/licenses:  
Groups/access:  
Physical access:  
Training:  
First meeting:  
Support contact:  
Exceptions:  
Readiness verified by:

## **12.2 Equipment assignment**

Asset tag/serial:  
Make/model:  
Assigned to:  
Location:  
Accessories:  
Condition:  
Encryption/management:  
Warranty:  
Issue date:  
Expected return/refresh:  
User acknowledgement:  
Technician:

## **12.3 Incident update**

Incident ID/severity:  
Start/time:  
Affected services/projects/users:  
Current impact:  
Known facts:  
Actions taken:  
Workaround:  
Risks/data/security/legal:  
Next actions:  
Next update:  
Incident Commander:

## **12.4 Restore request**

Requester:  
Service/project/assets:  
Reason:  
Classification:  
Desired point in time:  
RPO/RTO need:  
Approvals:  
Legal hold/security context:  
Recovery location:  
Validation owner:  
Result/evidence:

## **12.5 Vendor annual review**

Vendor/service:  
Owner:  
Criticality:  
Use and users:  
Data/media/access:  
Spend and utilization:  
SLA/support performance:  
Incidents/issues:  
Security/privacy/legal changes:  
Contract/price/renewal:  
Alternatives and exit:  
Decision: RENEW | RENEGOTIATE | REPLACE | RETIRE

## **12.6 Site opening checklist**

Lease/use/insurance approved:  
Power/load/cooling verified:  
Internet and network installed:  
Equipment room secure:  
Fire/life safety inspected:  
Access/visitor controls:  
Desks/ergonomics/accessibility:  
Inventory and spares:  
Cleaning/waste/recycling:  
Emergency contacts and plans:  
Operational test:  
Opening approval:

# **13\. Operations adoption checklist**

* Appoint COO/operations owner, IT/security owner, facilities owner, and named owners for identity, endpoints, network, storage, cloud, service desk, procurement, and continuity.  
* Inventory every current person, device, software subscription, vendor, account, service, storage location, key, badge, and contract before adding complexity.  
* Implement joiner/mover/leaver, endpoint, network segmentation, NAS backup, service desk, incident, and restore controls as the Year 1 minimum.  
* Tie every new hire and project greenlight to verified workspace, device, license, access, storage, support, and capacity readiness.  
* Refresh this five-year roadmap annually and after a material site, workforce, architecture, security, or business-model change.