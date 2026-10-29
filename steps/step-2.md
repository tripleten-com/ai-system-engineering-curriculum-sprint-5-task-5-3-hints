# Step 2: Decide what the agent does with each answer and close the loop

## `act` grows a chain of conditions

<details><summary>Hint 1</summary>

How many kinds are there, and does each one need different code? What data structure maps one value to one piece of code?

</details>

<details><summary>Hint 2</summary>

If every kind has an entry, is there any case left for a fallback? What would a fallback hide if a fifth kind ever appeared?

</details>

## `compile()` refuses the graph

<details><summary>Hint 1</summary>

Read the error's node name. Is a node registered under one name and referred to by another in an edge?

</details>

<details><summary>Hint 2</summary>

Compare the names you pass when you add the nodes with the names your edges and your rule use. Are they the same strings?

</details>

## Reading the run's result raises a `KeyError`

<details><summary>Hint 1</summary>

Which fields does the result contain? Print its keys. Did any node write the field you're reading?

</details>

<details><summary>Hint 2</summary>

A field left at its default may be missing from what the run returns. Read it with a default, or turn the result back into your state type first.

</details>

## The recovery-shaped run reads more than one entry

<details><summary>Hint 1</summary>

After the tool-call handler runs, what is the status? What does your rule return for that status?

</details>

<details><summary>Hint 2</summary>

Does the handler change the status at all, or does it only record a reason? The rule only looks at the status.

</details>

## The rule routes somewhere unexpected

<details><summary>Hint 1</summary>

What does the rule return: a name, a function or a constant? Which of those does a conditional edge expect?

</details>
