# Step 4: Fill `poe demo`

## The idea

From this Task on, the platform runs `poe demo` on every submission. A demo is a promise anyone can rerun: "the vaccine exception goes through, and here's the record". To be worth anything, it must be able to say no.

That shapes three things:

- **It starts from nothing it didn't create:** each run makes its own exception in `NEEDS_REVIEW`, the way the scenario command does. Then it works on a fresh stack and doesn't depend on an earlier run.
- **It ends with its verdict:** everything printed before the summary is progress. The summary is the record, so it comes last, after the run has ended and the exception has been read again. A reader who sees only the last lines has the whole story.
- **Its exit code is honest:** it exits 0 only when the run ended the way the success script should, and otherwise says what didn't hold and exits non-zero. A demo that exits 0 whatever happened proves nothing, and the platform only sees the exit code and the summary.

The demo and your dev command do the same work: start a run on the server and read its steps. Sharing that code keeps them from drifting apart.

## What's expected

- `poe demo` takes no arguments and runs from the repository root on a started stack.
- Its first line says what it expects: whether you started the stack, or it does.
- Its last lines are the run summary:
  - the exception id and the run id
  - each step's node and action kind
  - the final status, and the halt reason or "none"
  - the exception's state after the run
- It exits 0 when the run ended `finished`.
- Pointed once at your refusal script, it exits non-zero and says the run ended `refused`.

## When you're stuck

### The demo exits 0 even when the run was refused

<details><summary>Hint 1</summary>

Where does your demo decide its exit code?

</details>

<details><summary>Hint 2</summary>

Point the demo at your refusal script once. What exit code do you get?

</details>

<details><summary>Hint 3</summary>

Does the demo compare the final status with the one a success should have?

</details>

<details><summary>Hint 4</summary>

The approach: after the summary prints, compare the status with `finished`. If it matches, exit 0. If not, print which status came back and why that fails the demo, then exit non-zero.

</details>

### The demo works once, then fails

<details><summary>Hint 1</summary>

Does the demo create its own exception, or reuse an id from an earlier run?

</details>

<details><summary>Hint 2</summary>

Run it on a freshly started stack. What does it need that only an earlier run left behind?

</details>

<details><summary>Hint 3</summary>

Read what the scenario command runs in `pyproject.toml`. Call the same code from the demo, or run that command and read the id it prints.

</details>

### The summary gets lost in the output

<details><summary>Hint 1</summary>

Read only the last ten lines the demo prints. Do they hold the whole record?

</details>

<details><summary>Hint 2</summary>

Does the demo read the exception again after the run? Is the summary printed after that read?

</details>

<details><summary>Hint 3</summary>

Move every progress line before the run starts, or before the final read, and print the summary as the very last thing.

</details>

### The demo and the dev command disagree

<details><summary>Hint 1</summary>

Do they share the code that starts a run and reads its steps, or does each have its own copy?

</details>

<details><summary>Hint 2</summary>

Which copy did you change last?

</details>

<details><summary>Hint 3</summary>

Move the shared part into one function both commands call. The demo only adds its fixed script, a fresh exception and the verdict.

</details>
