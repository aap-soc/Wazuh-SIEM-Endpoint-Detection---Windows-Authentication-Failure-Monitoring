# Triage Runbook - Windows Authentication Failures (Wazuh)

**Owner:** Andre Patterson · **Tier:** 1 (first-line triage) · **Last reviewed:** 2026-10

A short standard operating procedure (SOP) for triaging failed-logon alerts raised by this
Wazuh lab. It documents how a Tier 1 analyst would take the alert from detection to a
decision (close or escalate), and what to record on the way.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 1. What this detection catches

| Field | Value |
|---|---|
| Source | Wazuh (host-based IDS) — Windows agent |
| Trigger | Windows Security log, **Event ID 4625** (an account failed to log on) |
| Wazuh rule | **60122** (Windows logon failure) |
| MITRE ATT&CK | Credential Access — **T1110 Brute Force** (and sub-techniques, e.g. T1110.001 password guessing) |
| Typical cause | Mistyped password, expired credentials, service account misconfiguration, or a genuine brute-force / password-spray attempt |

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 2. Severity guide (starting point, adjust with context)

| Observation | Suggested severity |
|---|---|
| 1–3 failures, one account, then a success | Low — likely a typo |
| Many failures on one account in a short window | Medium — possible brute force |
| Failures across **many** accounts from one source | High — possible password spray |
| Failures from an **external / unexpected** source IP | High — escalate |
| Failures against a privileged / admin account | High — escalate |

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. Triage steps (Tier 1)

1. **Acknowledge** the alert so it isn't double-handled, and note the time you started.
2. **Gather context** from the alert and raw event:
   - Which **account** failed?
   - **Source** host / IP (internal or external?).
   - **How many** failures, over what **time window**?
   - Was there a **successful logon** immediately after?
   - Is the account a **normal user, a service account, or privileged**?
3. **Ask the triage questions:**
   - Does the pattern look like a human typo, or automated attempts?
   - One account (brute force) or many accounts (password spray)?
   - Is the source expected (the user's own device) or unfamiliar?
4. **Classify** the alert as a **false positive** or a **true positive** (see §4 and §5).
5. **Decide the action:**
   - False positive → **close** with a clear note (see §6).
   - True positive or uncertain → **escalate to Tier 2** with full context.
6. **Document** everything in the ticket (see §6) before you close or hand off.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 4. Likely false positives (close with a note)

- A single user, a few failures, then a successful logon from their own device → mistyped or recently changed password.
- A known service account failing right after a scheduled password rotation.
- A test or lab action you can account for.

Close these with a one-line reason so the decision is auditable.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 5. Escalate to Tier 2 if any of these are true

- Repeated failures with **no legitimate explanation**.
- **Many accounts** targeted from one source (spray) or a **high volume** against one account (brute force).
- Source IP is **external or unrecognised**.
- A **privileged / admin** account is involved.
- Failures followed by a **successful logon** you cannot explain.

When escalating, hand Tier 2 the full timeline and context below — don't make them re-gather it.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 6. What to record (ticket template)

```
Alert ID / rule:            Wazuh rule 60122 (Windows Event ID 4625)
Date/time (UTC):
Analyst:                    Andre Patterson (Tier 1)
Affected account(s):
Source host / IP:
Failure count / window:
Successful logon after?      (Y/N + detail)
MITRE ATT&CK:                T1110 Brute Force
Assessment:                  False positive / True positive / Uncertain
Reasoning:                   (1–2 lines on why)
Action taken:                Closed / Escalated to Tier 2
Notes for Tier 2:            (timeline + anything already checked)
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 7. Notes & limitations

- This is a lab detection built to demonstrate the Tier 1 triage workflow; thresholds would be
  tuned to a real environment's baseline.
- Event ID 4625 records the *failure* only; correlate with 4624 (successful logon) and 4740
  (account lockout) for a fuller picture.
- Next step for this lab: add a rule to flag **many failures followed by a success** on the same
  account (a stronger compromise signal than failures alone).
