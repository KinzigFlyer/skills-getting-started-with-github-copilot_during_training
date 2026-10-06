## Plan: Add FastAPI Backend Tests

Add a root-level `tests/` suite for the current FastAPI app using pytest and FastAPI's `TestClient`. Cover the API's route behavior, isolate tests from its mutable in-memory activity data, and satisfy the repository exercise checks.

**Steps**
1. Add `pytest` to the existing runtime/development dependency list in `requirements.txt`; keep current dependencies intact.
2. Create `/workspaces/skills-getting-started-with-github-copilot_during_training/tests/test_app.py`. Import `app` and `activities` from `src.app`, provide a `TestClient` fixture, and snapshot/restore the global `activities` dictionary around each test using a deep copy.
3. Add focused route tests: root redirects to `/static/index.html`; `GET /activities` returns seeded activity data; signup succeeds; a repeated signup for the same activity and email returns 400 and leaves exactly one matching participant; signup for an unknown activity returns 404; participant deletion succeeds, missing registration returns 404, and unknown activity returns 404.
4. Run the tests from the repository root and confirm the workflow's checks for a `pytest` dependency and a `tests/` directory are satisfied. Do not change the workflow unless the actual test invocation is intended to become a CI requirement.

**Relevant files**
- `/workspaces/skills-getting-started-with-github-copilot_during_training/src/app.py` — source of routes and in-memory state; no app refactor is needed.
- `/workspaces/skills-getting-started-with-github-copilot_during_training/requirements.txt` — add pytest, preserving FastAPI, Uvicorn, httpx, and watchfiles.
- `/workspaces/skills-getting-started-with-github-copilot_during_training/pytest.ini` — already sets `pythonpath = .`, supporting imports through the `src` namespace package; leave unchanged unless validation disproves this.
- `/workspaces/skills-getting-started-with-github-copilot_during_training/.github/workflows/4-step.yml` — checks for pytest in requirements and the presence of a tests directory; it does not appear to execute the test suite in the inspected section.
- `/workspaces/skills-getting-started-with-github-copilot_during_training/tests/test_app.py` — new route tests.

**Verification**
1. Install dependencies with `python -m pip install -r requirements.txt` where needed.
2. Run `python -m pytest` from the repository root.
3. Confirm pytest reports all cases passing and that test mutations do not persist across cases due to fixture cleanup.

**Decisions**
- Put `tests/` at repository root, matching the exercise's directory check.
- Use pytest fixtures and `TestClient`; avoid production code changes or external server/network dependencies.
- Focus on currently implemented route behavior; do not add tests for capacity limits, persistence, authentication, or frontend browser behavior, none of which are implemented as backend contracts.