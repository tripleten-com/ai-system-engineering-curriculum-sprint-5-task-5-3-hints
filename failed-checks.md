# When a check fails

The platform lists what failed under **Errors**. Find that check below, read the first question, and open the next one only if you need it.

<!-- core:start -->
## CI `verify` is red

<details><summary>Hint 1</summary>

Which step of `poe verify` failed? It runs `integrity-record`, `unit`, `start`, `ready`, `ingest`, `smoke`, `e2e-tests`, `student-tests` and `integrity-check`, in that order. The CI log names the step that stopped the run.

</details>

<details><summary>Hint 2</summary>

A failure before `student-tests` means the platform you inherited stopped working: a change of yours broke a supplied test or stopped a service from starting. A failure in `student-tests` means one of your own tests failed. Run the same step on your machine: `poe student-tests` runs only your tests, and `poe runtime-logs` prints the services' logs with every stored key replaced.

</details>

<details><summary>Hint 3</summary>

Does it pass on your machine but fail in CI? Look for something your machine has that a fresh clone doesn't: a file you never committed, a container left running from an earlier run, or a path that only exists on your computer.

</details>

## CI `secret-scan` is red

<details><summary>Hint 1</summary>

Which commit and which file does the scan name? The scan reads every commit in your pull request, not only the latest files.

</details>

<details><summary>Hint 2</summary>

If a real secret reached a commit, deleting it in a later commit doesn't remove it from history. Follow **What never goes into a submission** in the lesson on how the project is submitted: revoke or rotate the secret, then contact course support. If the match is a value you made up for a test, compare it with the rules in `.gitleaks.toml` and ask whether a fixture name would serve instead of a value.

</details>

## No test was added or changed

<details><summary>Hint 1</summary>

Does your pull request touch a file under `tests/student/`? The check looks at the pull request's changes, not at the tests that already exist.

</details>

<details><summary>Hint 2</summary>

A test you changed only in a commit that isn't on the pull request's branch doesn't count. Compare your branch with `main` and look for the test file in the diff.

</details>

## The evidence note is missing or incomplete

<details><summary>Hint 1</summary>

Is the note at exactly `docs/capstone/task-NN.md`, with this Task's number in two digits? Is it merged into `main`?

</details>

<details><summary>Hint 2</summary>

Does it have all three headings, **What I built**, **How I checked it** and **What I'd defend**, each with text under it? A heading with nothing under it, or a "TODO", fails the check.

</details>

## `poe demo` failed

<details><summary>Hint 1</summary>

Run `poe demo` on your machine from a fresh start of the stack. Does it exit 0, and does it end with its run summary? Read the last lines it prints before it stops.

</details>

<details><summary>Hint 2</summary>

Did this Task change something the demo depends on, such as a script, a configuration value or a command? Run the demo before and after this Task's changes and compare where each run ends.

</details>

## The workflow file changed

<details><summary>Hint 1</summary>

The CI workflow file must stay exactly as the template supplied it. Compare your copy with `main` of the template, and look for an edit you didn't mean to make, such as a reformatted line or changed line endings.

</details>
<!-- core:end -->

## In this Task

<details><summary>The <code>start</code> step times out</summary>

Is your agent server healthy? Read its log and `docker compose ps`. CI starts the stack from a fresh clone: does your service need anything that only exists on your machine?

</details>

<details><summary><code>poe demo</code> fails on the platform but passes for you</summary>

Does the demo create its own exception, and does it run on a freshly started stack? Does it exit 0 only when the run ends `finished`?

</details>
