# The Forge

The Forge is the CyberHub malware development and reverse engineering environment. It provides isolated sandbox workspaces for educational use, including analysis, detonation, and controlled tooling for malware research and secure software experimentation.

The Forge is planned and not yet fully implemented. This document describes the target capabilities and the intended integration points with CyberCore.

## Goals

1. Provide safe and isolated environments for malware analysis and reverse engineering
2. Support structured training and research workflows without exposing the broader CyberHub infrastructure
3. Enforce strict access controls, logging, and policy boundaries for high-risk activities
4. Provide repeatable, disposable sandboxes with reliable reset and teardown behavior

## Sandbox Model

Each Forge sandbox is a time-bounded allocation of one or more virtual machines provisioned for a user or approved group. Sandboxes are designed to be isolated from:

- The public internet
- Other user sandboxes
- Non-Forge CyberHub networks and services

A sandbox may include:

- A primary workstation VM (Windows or Linux)
- Optional supporting VMs (analysis services, logging collector, internal tooling)
- A controlled file transfer mechanism

## Access Model

- Interactive access is expected to be provided through RDP or a browser-based desktop gateway, depending on the chosen implementation.
- Direct inbound access should be limited to approved protocols and enforced at the sandbox boundary.
- Access credentials and session lifetimes are managed through CyberCore.

## Networking and Isolation

Forge sandboxes are expected to operate in a restricted network mode:

- No default route to the public internet
- No direct connectivity to other CyberHub environments
- Explicit allowlists for internal services only when required by a lab

If a scenario requires controlled outbound access, it should be implemented as an explicit exception with policy gating and auditing.

## File Transfer

Sandbox file movement should be controlled and auditable. The preferred model is a dedicated file transfer service or proxy that supports:

- Upload and download with per-user isolation
- Malware-safe storage handling and retention rules
- Optional scanning and metadata capture
- Full audit logging of transfers

The file transfer mechanism should not provide general network connectivity into or out of the sandbox.

## CyberCore Integration

The Forge relies on CyberCore as the control plane. CyberCore records allocations and triggers provisioning workflows.

CyberCore n8n workflows are expected to:

1. Create and manage sandbox allocations and TTL
2. Provision sandbox VMs and networks from templates
3. Apply isolation policies and firewall rules
4. Configure access artifacts and credentials
5. Drive sandbox reset, rebuild, and teardown operations
6. Record audit events and state transitions

## Safety and Policy Controls

The Forge is a high-risk module by design. It should include:

- Role-based access gating
- Explicit acceptable use rules
- Logging and audit trails for provisioning and access events
- Automated teardown on allocation expiration
- Operator controls for emergency suspend and forced teardown

## Supported Operating Systems

Initial targets are expected to include Windows and Linux workstation profiles. Additional OS profiles can be added as templates and tooling mature.

## Status

The Forge is planned. Sandbox templates, file transfer workflow, and CyberCore orchestration will be tracked as implementation begins.