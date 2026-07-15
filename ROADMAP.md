# Tempest Lab Systems Roadmap

Tempest Lab Systems is the flagship AngryAtom reference architecture for modern self-hosted infrastructure.

This roadmap tracks the direction of the platform and the documentation that supports it.

## Current Focus

- Strengthen architecture documentation.
- Expand operational runbooks.
- Improve service catalog examples.
- Continue security posture review and remediation tracking.
- Document public/private service exposure decisions.
- Capture realistic failure modes and recovery paths.

## Short-Term Improvements

- Add a filled service catalog example with sanitized details.
- Add a public edge rollback runbook.
- Add a DNS and reverse proxy troubleshooting runbook.
- Add a post-restart validation checklist as a standalone runbook.
- Add a backup and restore validation outline.
- Add a lightweight architecture decision record format.

## Medium-Term Improvements

- Document identity provider architecture and user lifecycle flows.
- Expand MFA, break-glass, onboarding, and offboarding documentation.
- Add monitoring coverage notes for critical services.
- Add a security review checklist for public-facing services.
- Document storage pressure, media ingest, and recovery expectations.
- Add exported architecture diagrams where useful for quick scanning.

## Long-Term Direction

- Mature Tempest into a reusable reference architecture for self-hosted infrastructure.
- Extract mature tools into separate repositories when they deserve their own lifecycle.
- Connect approved public lab services, automation utilities, and security tooling back to the platform where appropriate.
- Use Tempest as the operating foundation for future product, security, AI, and automation work.

## Project Boundaries

Tempest documents real engineering patterns while avoiding sensitive operational details.

Public documentation should not include:

- Secrets or credentials.
- Private public-IP details.
- Sensitive screenshots.
- Live attack instructions.
- Personal data.
- Information that would materially weaken the environment.

## Success Standard

Tempest should leave a technical reader thinking:

This is not just a collection of hosted services. This is an engineered platform with architecture, operations, security thinking, recovery discipline, and documentation quality.
