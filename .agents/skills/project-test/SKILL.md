---
name: project-test
description: Independently verify the whole project against the official task statement and docs/PLAN.md using executable checks, changed-input runs, failure injection, and a secrets scan. Use for mid-phase or final QA; for rendered-UI checks use ui-verify instead.
---

# Project Test

## Goal
Decide, from **actual execution evidence**, whether each requirement is met. Do not trust any AI completion report, including your own. Report `PASS / FAIL / NOT VERIFIED` per requirement.

## 1. Scope
1. Read the official task statement first, then `AGENTS.md`, `docs/PLAN.md` (R-ID, acceptance criteria, "verification method", forbidden/pitfall items) and `docs/PROGRESS.md`.
2. Build a checklist of every required R-ID, plus every required submission file or config the task statement lists. Never invent requirements.
3. Never modify existing source code, PLAN, QUESTION, or DEFENSE. Adding new checker scripts or tests is allowed, only as new files in the project's test location. Record results only in `docs/PROGRESS.md` (Test columns).

## 2. Verify rules with executable checks
1. For each rule in the task statement (limits, deadlines, capacity, uniqueness, coverage, etc.), look for an existing independent checker. If none exists, write a small standalone script or test that reads the **produced output** and prints PASS/FAIL per rule. It must not import the implementation's own decision logic.
2. Run it on real output. Also run it on a deliberately broken sample to prove the checker can fail.
3. Check forbidden/pitfall items listed in PLAN directly (for example, a rule that must not be shortcut).

## 3. Change the input and rerun
1. Change an input the task says can change (data values, operator-set parameters, selected range) and regenerate.
2. Confirm the result changes correctly and is not hard-coded, cached from the previous input, or accidentally identical.
3. Check parameter defaults are not baked into code: change each operator-adjustable value and confirm the output follows.
4. Check date/time and direction/unit boundaries (midnight crossing, asymmetric values, inclusive/exclusive ends) where the task involves them.

## 4. Failure and boundary behavior
1. Invalid or missing input: the user sees a specific message; existing data and displayed results stay unchanged.
2. Empty input, repeated or overlapping requests, slow or failed dependencies, failure while saving: no half-written state, no stale result presented as current.
3. Restart the service and confirm persisted state survives, when persistence is required.
4. Where a role/permission model exists, confirm each role can and cannot do what the task says.

## 5. Submission and security
1. Start the service following only the documented steps, without editing source. Report anything undocumented that was needed.
2. Scan source, config, and Git history (`git log -p`, `git grep`) for secrets. Credentials must come from runtime environment variables only.
3. Compare produced files against the task's required deliverables list (names, formats, locations).

## 6. Independent observation
Confirm at least one core result by a means other than the app's own report: curl or direct file read, a browser check, or recomputing one value by hand. For rendered UI screenshots use `ui-verify`.

## Report
For each R-ID: verdict, command or action, observed result, evidence location. For each FAIL: expected vs. observed and a reproduction. Separate verified facts from inference. After Dev fixes a FAIL, rerun the same check, and keep the failure and fix history.
