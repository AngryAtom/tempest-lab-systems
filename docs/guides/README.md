# Guides

These guides come from real infrastructure build and recovery work. They focus on patterns, decisions, and failure modes from a self-hosted operational environment.

## Infrastructure

### Private DNS And Reverse Proxy Naming Layer

[Read the guide](private-dns-reverse-proxy.md)

How internal DNS rewrites, HTTPS routing, and clean service names make a self-hosted environment easier to operate.

### Public And Private Service Edge

[Read the guide](public-private-service-edge.md)

How to decide which services stay private, which can be exposed, and what monitoring and rollback controls belong at the edge.

### Remote Access And Support Workflow

[Read the guide](remote-access-and-support.md)

How VPN access, device inventory, and remote support tooling fit together without giving every device access to every service.

## Operations

### Monitoring That Helps Instead Of Making Noise

[Read the guide](monitoring-that-helps.md)

How to design checks that explain where a failure lives: service origin, reverse proxy, DNS, public route, or alerting layer.

### Nextcloud Drop Folder To Media Library

[Read the guide](nextcloud-drop-folder-to-media-library.md)

How Veldora-style Nextcloud drop folders feed Shuna-style Jellyfin libraries through a safe, logged ingest workflow.

### Veldora Nextcloud Operations

[Read the guide](veldora-nextcloud-operations.md)

How to operate a file-sync service as a controlled upload surface with ownership, scans, quotas, and recovery notes.

### Shuna Jellyfin Operations

[Read the guide](shuna-jellyfin-operations.md)

How to operate a media server with separate libraries, access paths, metadata review, monitoring, and post-ingest validation.

### Shuna Public Edge And Reverse Proxy

[Read the guide](shuna-public-edge-reverse-proxy.md)

How to publish a Jellyfin-style service safely through a reverse proxy, including no-domain testing, owned-domain setup, hardening, validation, and rollback.

## Identity And Security

### Identity And User Onboarding

[Read the guide](identity-and-user-onboarding.md)

How to think about central users, groups, MFA, service integration models, onboarding, offboarding, and break-glass access.

### Cyber Range And SOC Lab Direction

[Read the guide](cyber-range-and-soc-lab.md)

How to keep security practice legal, contained, measurable, and useful for defensive learning.

## Related Case Studies

- [Home media platform case study](../projects/home-media-platform.md)
- [Portainer local endpoint recovery](../incident-notes/portainer-stale-docker-socket.md)
