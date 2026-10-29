# Step 5: Prove it with tests and write the evidence note

## Tests that build the adapter fail with a settings error

<details><summary>Hint 1</summary>

Did you set the worker's three variables for the tests the same way you did in the shell? Find how an existing worker unit test does it with a fixture.

</details>

## The "always stop" break doesn't fail any test

<details><summary>Hint 1</summary>

Which direction of the rule do your tests check? Would a rule that always stops pass every one of them?

</details>

<details><summary>Hint 2</summary>

Build a state whose status is still running and ask the rule where to go.

</details>

## A test of an ending passes for the wrong reason

<details><summary>Hint 1</summary>

Does the test assert the status and the halt reason, or only the status?

</details>

<details><summary>Hint 2</summary>

Make the tool-call handler end the run `finished`. Which of your tests fails? If none does, which assertion is missing?

</details>

## A test breaks every time the state grows

<details><summary>Hint 1</summary>

Does the test compare the whole returned mapping? Which fields does the requirement actually name?

</details>

## The recovery-shaped run's step count is wrong in your test

<details><summary>Hint 1</summary>

Where does your test read the step count: from the run's result, or from the script's length?

</details>
