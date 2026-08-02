# Sysmon PowerShell Policy Test Noise

## Symptom

After adding Sysmon telemetry to a Windows workstation, the SIEM dashboard showed a burst of high-severity alerts for script or executable file creation under the user's temp directory.

The most alarming alerts looked like suspicious file drops in a malware-friendly location.

At first, that looked like the workstation had serious problems. The volume was high enough to make the dashboard hard to use.

## Impact

The endpoint telemetry was technically working, but the signal was not analyst-friendly.

The alert flood caused three practical problems:

- High-severity counts became misleading.
- Real endpoint findings were harder to spot.
- Dashboard panels made the environment look worse than the evidence supported.

## Root Cause

Raw event samples showed that PowerShell was creating temporary files named like:

```text
__PSScriptPolicyTest_*.ps1
```

These files are created during PowerShell execution-policy checks.

The SIEM rule was broad by design: scripting files created under user temp folders are often worth watching. In this environment, however, PowerShell's own self-test pattern was producing repeated benign events.

This was not a compromise finding by itself. It was a telemetry tuning problem.

## Diagnosis

The useful diagnostic path was:

1. Find the top noisy rule in a short recent window.
2. Pull raw event samples for that rule.
3. Compare the image, target filename, user, and path.
4. Confirm whether the same pattern repeats.
5. Decide whether to tune at the SIEM rule layer or at the source telemetry layer.

The decisive clue was the repeated filename pattern:

```text
\AppData\Local\Temp\__PSScriptPolicyTest_*.ps1
```

The creating process was PowerShell, and the filenames followed the same execution-policy test shape.

## Fix

The cleaner fix was source-side tuning in the Sysmon configuration.

Instead of suppressing every script creation event under user temp, the Sysmon config was adjusted to exclude only the PowerShell policy-test filename pattern:

```text
\__PSScriptPolicyTest_
```

That preserves useful detections for suspicious script, executable, and persistence-related file creation while removing a known benign flood.

## Verification

Do not validate this kind of tuning with a large historical dashboard window.

Old alerts remain indexed, so a 24-hour view can still show the earlier spike after the fix is already working.

Better validation:

1. Re-apply the Sysmon config.
2. Restart or refresh the endpoint agent if needed.
3. Wait briefly for new events.
4. Check a short window such as the last two to five minutes.
5. Confirm the noisy rule is no longer dominating the stream.
6. Confirm the endpoint still sends other telemetry.

The goal is not "no endpoint alerts." The goal is "the remaining endpoint alerts deserve review."

## Prevention

- Treat new telemetry sources as noisy until proven otherwise.
- Tune with raw samples, not guesses.
- Prefer narrow source-side exclusions for known benign event patterns.
- Keep broad SIEM detections intact when they still catch useful behavior.
- Record tuning decisions so the next workstation can be onboarded repeatably.

## Lesson Learned

More telemetry is not automatically better telemetry.

Sysmon made the workstation much more visible, but the first pass also made the dashboard louder than useful. The mature move was not to disable the detector; it was to tune one well-understood benign pattern and keep the high-value signal.
