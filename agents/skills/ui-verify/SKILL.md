---
name: ui-verify
description: Verify a running web application's rendered UI using Playwright screenshots, critical user-flow assertions, and browser error checks. Use after UI implementation or changes; not for data-only tasks or general server setup.
---

# UI Verify

## Goal
Verify that the **actual running application** renders correctly and meets relevant UI acceptance criteria. A screenshot alone does not prove functional correctness. Prefer focused, repeatable checks over large test suites.

## 1. Select scope and evidence
1. Prioritize the official task README and root `AGENTS.md`.
2. Read only relevant requirements (`R-ID`) in `docs/PLAN.md` and status (`T-ID`) in `docs/PROGRESS.md`, if present.
3. Identify the target route, expected page state, critical user interaction, and applicable error/empty state.
4. Never invent UI requirements, URLs, credentials, outcomes, or file paths.

## 2. Inspect the runtime
1. Detect the existing framework, test runner, Playwright availability, browser binaries, and actual server address/port.
2. Prefer **already installed** Playwright packages, browsers, tests, and configuration.
3. If the server is not running, use the **documented existing startup command** only if permitted. Keep the server running in its own terminal; do not build a separate server-management system.
4. Before a new dependency, large browser download, or substantial configuration change, explain the cost and seek approval.
5. If Playwright/browser execution is blocked, **do not bypass restrictions**. Use the permitted in-environment Browser preview and manual checks where feasible; explicitly mark Playwright screenshot verification **not completed**.

## 3. Capture and verify the real page
For each prioritized `R-ID` / `T-ID`:
1. Open the **running app**, not a mock/static replacement, in a real Playwright browser session within the provided workspace.
2. Wait for a meaningful page element or ready state using Playwright locators/auto-retrying assertions. Do not rely on fixed sleeps or `networkidle` alone.
3. Capture a PNG **after the page has rendered**. For a flow with meaningful state change, also capture the post-interaction UI.
4. Save to `artifacts/ui-verify/` unless the official task specifies another output directory. Use names like `R-01-main.png`, `R-02-after-submit.png`.
5. Confirm screenshot files exist and are nonempty. If image inspection is supported, inspect for blank/clipped UI, obstructing overlays, and layout issues. If not, say **captured but not visually inspected**.
6. Verify the critical flow using actual actions and assertions: visible content, navigation, form submission, and relevant error states. Inspect console/page errors and failed requests; distinguish intentional error tests from defects.
7. Use existing test conventions. A snapshot comparison (`toHaveScreenshot`) is optional and is valid only with a trustworthy baseline in a comparable environment.

A minimal Playwright example (replace the placeholder with the **actual** requirement):

```js
await expect(page.getByRole('heading', { name: 'EXPECTED_TITLE' })).toBeVisible();
await page.screenshot({ path: 'artifacts/ui-verify/R-01-main.png', fullPage: true });
```

## 4. Diagnose and re-check failures
1. Record **expected vs. observed** behavior and a reproducible check.
2. Fix only within the approved scope. Ask before major architecture or requirement changes.
3. Re-run the **same failing check**, plus a minimal related smoke test.
4. Capture a new screenshot if the UI changed.
5. Never weaken assertions, delete tests, or mark a failed requirement complete merely to produce a passing result.

## 5. Record evidence with minimal documentation
- Update `docs/PROGRESS.md` only for meaningful test, defect, fix, or re-verification events, using its existing structure.
- Connect `R-ID` / `T-ID` to the **actual test command, result, screenshot path, remaining problem, and re-test outcome**.
- Add important mechanisms or risks to `docs/DEFENSE.md` only when useful for understanding the submitted implementation.
- Do not create extra management Markdown files or duplicate long logs.

## 6. Safety, scope, and time
- Use only permitted in-workspace tools and URLs. Never export screenshots/task data, Git Push, or bypass exam restrictions.
- Do not capture passwords, API keys, private data, or other secrets; use safe test fixtures.
- Do not overwrite the official README or alter required submission structure to accommodate testing.
- Prioritize **one rendered screenshot + core user flow + significant failure state**. Expand only for required acceptance criteria or available time.
- Distinguish **passed**, **failed**, **untested**, and **blocked**. A screenshot created without image inspection or a flow assertion is not a full UI pass.

## Final report
Report briefly:
- `R-ID` / `T-ID` and tested route(s)
- Executed command and browser/tool
- Screenshot path(s)
- Screenshot capture, visual inspection, and functional assertion results **separately**
- Reproduced errors, fixes/re-checks, remaining limitations, and next action
