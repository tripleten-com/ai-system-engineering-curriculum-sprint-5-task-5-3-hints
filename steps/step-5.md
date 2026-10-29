# Step 5: Prove it with tests and write the evidence note

## The idea

The server runs prove one script on one day. Tests prove the rules behind them, every time anyone runs the suite. Your graph is a Python object, so the tests call it directly, with no server and no stack. The emulator answers from your scripts, so you need no stand-in model, only the worker's three settings so the adapter can be built.

Five groups cover what you built:

- **The state's starting values.**
- **Each ending.**
- **What `propose` records.**
- **The rule in both directions.**
- **One in-process run per script.**

Two of them are easy to get half right:

- **The rule:** a rule that always stops passes every test that only checks the stopping side. You also need a state that's still running and a check that the rule sends it back to `propose`.
- **The endings:** a test that checks only the status passes when a handler ends the run for the wrong reason. Check the reason too.

Assert only the fields a requirement names. A handler returns what changed, and a test that compares the whole mapping breaks the day the state gains a field the test never cared about. As before, break each group once on purpose and watch a test fail before you trust it.

> Covered in **CI Gates and Release Verification**.

## What's expected

- Your test file sits where `poe student-tests` collects it, and your Task 1 and Task 2 tests still pass beside it.
- The tests set the worker's three variables in a fixture and need no server.
- Each ending test asserts both the status and the halt reason.
- The rule is tested with a running state and with each ending status.
- The in-process run of the recovery-shaped script reports one step.
- You broke each group once and saw a test fail:
  - a rule that always stops
  - a handler for tool calls that ends the run `finished`
  - a `propose` that drops the tokens
  - a node registered under another name
- `poe verify` and `poe demo` pass after your last edit.

## When you're stuck

### Tests that build the adapter fail with a settings error

<details><summary>Move 1</summary>

Which setting does the error name?

</details>

<details><summary>Move 2</summary>

Did you set the worker's three variables for the tests, or only in your shell?

</details>

<details><summary>Move 3</summary>

Find how `tests/unit/worker/test_bootstrap.py` sets them with a fixture, and use the same kind of fixture in your test file.

</details>

### The "always stop" break doesn't fail any test

<details><summary>Move 1</summary>

Which direction of the rule do your tests check?

</details>

<details><summary>Move 2</summary>

Would a rule that always stops pass every one of them?

</details>

<details><summary>Move 3</summary>

Add a test that builds a state that's still running and checks the rule sends it to `propose`.

</details>

### A test of an ending passes for the wrong reason

<details><summary>Move 1</summary>

Does the test assert the status and the halt reason, or only the status?

</details>

<details><summary>Move 2</summary>

Make the tool-call handler end the run `finished`. Which of your tests fails?

</details>

<details><summary>Move 3</summary>

Assert both fields for every ending, and send at least one of them through `act`, so the lookup is covered too.

</details>

### A test breaks every time the state grows

<details><summary>Move 1</summary>

Does the test compare the whole returned mapping?

</details>

<details><summary>Move 2</summary>

Which fields does the requirement for that test actually name?

</details>

<details><summary>Move 3</summary>

Assert only those fields, each on its own line, so a new field elsewhere doesn't break a test that never cared about it.

</details>

### The recovery-shaped run's step count is wrong in your test

<details><summary>Move 1</summary>

Where does your test read the step count: from the run's result, or from the script's length?

</details>

<details><summary>Move 2</summary>

When does the run stop: when the script runs out, or when the rule says so?

</details>

<details><summary>Move 3</summary>

Read the step count from what the run returns, and expect one step for the recovery-shaped script.

</details>
