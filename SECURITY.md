# Security Policy

Rafa is designed around least privilege, private control planes, auditable authorization, and evidence-based operations.

## Authorized security work only

Security examples, network diagrams, access-control patterns, incident procedures, and testing guidance in this repository are intended for **systems you own or are explicitly authorized to assess**.

This repository does not grant permission to access, scan, probe, exploit, disrupt, modify, monitor, or collect data from third-party systems.

## Reporting a vulnerability

If you discover a security issue in a Rafa implementation or in materials maintained by Stoneforge Labs:

- do not publish credentials, personal data, confidential information, exploit details, or sensitive logs;
- preserve evidence without expanding access beyond what is necessary to understand the issue;
- avoid destructive testing, denial of service, persistence, data exfiltration, or lateral movement;
- report the issue privately through the repository owner's approved security contact or private reporting mechanism;
- include affected component, version, reproduction conditions, impact, and any safe mitigation you identified.

If a private reporting channel has not yet been published, contact the repository owner through an existing trusted organizational channel rather than opening a public issue containing sensitive details.

## Security expectations for implementations

Implementers should, at minimum:

- use unique human and service identities with least privilege;
- require strong authentication and protect privileged access;
- keep secrets in approved secret-management systems rather than source control or documentation;
- keep databases, NAS administration, render control, and management interfaces off the public internet;
- segment networks and restrict service-to-service flows;
- pin and review dependencies, plugins, actions, and infrastructure changes;
- log and audit significant access and state changes without exposing secrets;
- validate inputs, paths, manifests, artifacts, and provider responses;
- maintain backups, restore procedures, incident response, and recovery tests;
- review access when people, roles, projects, vendors, or environments change.

## Sensitive information

Do not commit:

- passwords, API keys, private keys, recovery codes, tokens, cookies, or production connection strings;
- customer-confidential or unreleased media unless the repository and access model are explicitly approved for it;
- personal, biometric, identity, HR, financial, or regulated data without a documented lawful purpose and approved controls;
- proprietary third-party content or source code without permission.

## Responsible disclosure and law

Security research and testing must comply with applicable law, contracts, organizational policy, and authorization scope. When the scope is unclear, obtain written authorization before testing.

See [RESPONSIBLE_USE.md](./RESPONSIBLE_USE.md) for the broader lawful-use and compliance policy.
