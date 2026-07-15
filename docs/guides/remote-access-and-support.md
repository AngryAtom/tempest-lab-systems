# Remote Access And Support Workflow

Remote access is not just "can I connect?" It also needs device ownership notes, user permission boundaries, and a support workflow that does not depend on memory.

## Goals

- Reach the lab safely while offsite.
- Support user devices when they need help.
- Keep admin services private.
- Track which device belongs to which person.
- Avoid giving every VPN device access to every lab service.

## Remote Access Layers

| Layer | Purpose |
| --- | --- |
| VPN | Private network reachability |
| DNS | Clean service names |
| ACLs | Limit who can reach what |
| Remote desktop relay | Help users and reach workstations |
| Password manager | Store setup details and recovery notes |
| Access inventory | Know who owns which account/device |

## Device Inventory Template

| Owner | Device | Role | VPN | Remote Support | Notes |
| --- | --- | --- | --- | --- | --- |
| `example-user` | `example-laptop` | Standard user | Yes | Optional | Media + file sync |
| `operator` | `admin-desktop` | Admin | Yes | Yes | Lab management |

## VPN Access Principle

Being on the VPN should not automatically mean access to everything.

Recommended:

- Owners/admins can reach management services.
- Standard users can reach user-facing services.
- Guests can reach only explicitly allowed services.
- New users get no broad access until assigned to an access group.

## Remote Desktop Notes

Self-hosted remote desktop relays are useful, but they are not full user management systems by themselves.

Track:

- Device ID.
- Device owner.
- Unattended access status.
- Password/key location.
- Last validation date.
- Whether the device is on VPN, LAN, or public network.

## Lessons Learned

- Remote access needs inventory and policy, not just connectivity.
- VPN DNS and VPN routing are separate problems.
- Remote support tools should be documented like production support tools.
- Passwords belong in the password manager, not runbooks.

