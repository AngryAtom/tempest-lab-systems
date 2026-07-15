# Cyber Range And SOC Lab Direction

The security side of the lab is meant for legal, contained practice.

The goal is to build both sides:

- Defensive visibility and alerting.
- Safe offensive practice against intentionally vulnerable lab targets.

No public guide in this repo should encourage attacking third-party systems.

## Lab Principles

- Only test systems you own or have explicit permission to test.
- Keep practice targets isolated.
- Log and monitor practice activity.
- Prefer known intentionally vulnerable apps for demos.
- Document what each scenario teaches.
- Avoid publishing live attack paths against real infrastructure.

## Defensive Stack Goals

| Capability | Purpose |
| --- | --- |
| Honeypots | Learn what probes look like |
| Web access logs | Detect suspicious requests |
| Host telemetry | Track auth, service, and process events |
| Container telemetry | Understand what services are doing |
| Alert broker | Send meaningful notifications |
| Case notes | Capture what happened and what was learned |

## Practice Range Goals

| Component | Purpose |
| --- | --- |
| Attack box | Tooling workstation inside the lab |
| Vulnerable targets | Safe systems to attack |
| Scenario notes | Repeatable exercises |
| Monitoring | Observe both attack and defense sides |
| Reset process | Return targets to known-good state |

## Example Scenarios

- Web probe detection.
- Fake SSH/Telnet login attempt.
- Suspicious path scan against a reverse proxy.
- Public service rate-limit test.
- Vulnerable web app walkthrough inside an isolated network.

## Documentation Pattern

Each scenario should include:

- Objective.
- Lab-only scope.
- Target.
- Tools.
- Commands.
- Expected telemetry.
- Cleanup/reset.
- Lessons learned.

## Lessons Learned

- Legal scope is the first control.
- Telemetry makes practice more valuable.
- Reset paths matter as much as exploit paths.
- Good security labs teach restraint as well as technique.

