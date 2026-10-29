# Step 2: Decide what the agent does with each answer and close the loop

## The idea

`propose` thinks; `act` decides what that thought is allowed to become. In this Task the agent has no tool and no way to reach a person, so `act` can't carry out a tool call or an approval request. What it can do is end the run honestly, with a reason a dispatcher can read. `not_enabled` means "the model asked for something this agent can't do yet". That's different from the model refusing, and the difference has to survive into the record.

Two shapes keep this easy to read and to test:

- **One handler per kind:** a small function for each of the four kinds, all with the same inputs and outputs, and a table that maps each kind to its handler. `act` only looks up and calls. With a handler for every kind, there's nothing left for a fallback branch to hide.
- **A rule that answers one question:** after `act`, is the run still going? If the status is `running`, the next node is `propose`. Otherwise the run ends. The rule doesn't care which handler ran.

The graph is then a short, readable claim about control flow: two nodes, a plain edge from `propose` to `act`, and a conditional edge back. Anyone can read it without running it.

> Covered in **3.1 Agents as State Graphs**.

## What's expected

- `build_graph()` returns a compiled graph, and asking it for its nodes shows `propose` and `act`.
- An in-process run of your success script ends `finished` with one step.
- An in-process run of your refusal script ends `refused`, with the script's reason as the halt reason.
- An in-process run of your recovery-shaped script ends `refused` with `not_enabled` after one step, even though the script has more entries.
- `poe contract` and `poe unit` still pass.

## When you're stuck

### `act` grows a chain of conditions

<details><summary>Hint 1</summary>

How many kinds are there, and does each one need different code?

</details>

<details><summary>Hint 2</summary>

What data structure maps one value to one piece of code?

</details>

<details><summary>Hint 3</summary>

If every kind has an entry in that structure, is there any case left for a fallback? What would a fallback hide if a fifth kind ever appeared?

</details>

<details><summary>Hint 4</summary>

The approach: four handlers with one shared signature, a read-only mapping from each kind to its handler, and an `act` that takes the last proposal, looks up its handler and returns what the handler returns.

</details>

### `compile()` refuses the graph

<details><summary>Hint 1</summary>

Read the error's node name. Is a node registered under one name and used by another in an edge?

</details>

<details><summary>Hint 2</summary>

Compare the names you pass when you add the nodes with the names your edges and your rule use. Are they the same strings?

</details>

<details><summary>Hint 3</summary>

Define each node name once and use that same name everywhere. Then add both nodes and both edges before you compile, and compile once.

</details>

### The rule routes somewhere unexpected

<details><summary>Hint 1</summary>

What does your rule return: a node's name, a function, or a constant?

</details>

<details><summary>Hint 2</summary>

Which values does a conditional edge expect? Check the Graph API reference: a registered name, or the end constant.

</details>

<details><summary>Hint 3</summary>

Test both directions in the shell: a state that's still `running`, and one for each ending status.

</details>

### The recovery-shaped run reads more than one entry

<details><summary>Hint 1</summary>

After the tool-call handler runs, what's the status?

</details>

<details><summary>Hint 2</summary>

What does your rule return for that status?

</details>

<details><summary>Hint 3</summary>

Does the handler change the status, or only record a reason? The rule only looks at the status.

</details>

<details><summary>Hint 4</summary>

The approach: the tool-call handler ends the run, setting the status to `refused` and the halt reason to `not_enabled`, so the rule stops the run after the first entry.

</details>

### Reading the run's result raises a `KeyError`

<details><summary>Hint 1</summary>

Print the keys of the result. Is the field you're reading among them?

</details>

<details><summary>Hint 2</summary>

Did any node write that field during this run? A field left at its default may not be in the result.

</details>

<details><summary>Hint 3</summary>

Read such fields with a default, or turn the result back into your state type before you read it.

</details>
