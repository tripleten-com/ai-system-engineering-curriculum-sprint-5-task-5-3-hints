# Step 4: Fill `poe demo`

## The demo exits 0 even when the run was refused

<details><summary>Hint 1</summary>

Where does your demo decide its exit code? Does it look at the run's final status at all?

</details>

<details><summary>Hint 2</summary>

Point the demo at your refusal script once. What exit code do you get? If it's 0, the demo can't fail.

</details>

## The demo works once, then fails

<details><summary>Hint 1</summary>

Does the demo create its own exception, or does it reuse an id from an earlier run?

</details>

<details><summary>Hint 2</summary>

Run it on a freshly started stack. What does it need that only an earlier run left behind?

</details>

## The summary gets lost in the output

<details><summary>Hint 1</summary>

What are the last lines the demo prints? A reader who sees only those should have the whole record.

</details>

<details><summary>Hint 2</summary>

Does the demo read the exception again after the run, and does it print the summary after that read?

</details>

## The demo and the dev command disagree

<details><summary>Hint 1</summary>

Do they share the code that starts a run and reads its steps, or does each have its own copy?

</details>
