# Step 1: Define what the agent remembers and make it ask the model

## The idea

An agent run is a loop: ask the model what to do, act on the answer, decide whether to go round again. LangGraph builds that loop from three pieces:

- **State:** the run's memory. Between one step and the next, nothing survives except what's in the state. For the vaccine exception, that's which exception this is, the proposals so far, how many steps were taken, the token total, and how the run ended.
- **Nodes:** plain functions that read the state and return only the fields they changed. LangGraph merges what a node returns into the state. A node that edits the state in place and returns nothing has changed nothing.
- **Edges:** the rules that say which node runs next.

This Step builds the state and the first node, `propose`. Its whole job is one question to the model: build a request from what the state knows, ask through the port, record the answer.

Two design ideas carry through the rest of the Project:

- **Choices live in one place:** which model adapter answers is decided by one function that reuses the worker's own settings, never inside the node. Then the same node runs against the emulator now and against a real model later.
- **Closed lists for anything a rule depends on:** the run's status is a closed list like `ActionKind`, so a typo can't reach the rule you'll write next.

> Covered in **3.1 Agents as State Graphs**.

## What's expected

- In the shell, a state built from an exception id alone has: an empty proposals list, zero steps, zero tokens, the status `running`, and no halt reason.
- Setting the status to a word outside your list is refused.
- With the worker's three variables set, the function that builds the model adapter returns one without touching the node.
- Calling `propose` on a fresh state returns three changed fields: a list holding one proposal, a step count of 1, and the proposal's token count.
- Calling it again on that new state returns the script's second entry, not the first.

## When you're stuck

### The model adapter won't build in your shell or tests

<details><summary>Move 1</summary>

Read the error. Which setting does it say is missing?

</details>

<details><summary>Move 2</summary>

Inside the stack, `compose.yaml` gives the worker those values. On your machine nothing does. Find how `tests/unit/worker/test_bootstrap.py` supplies them.

</details>

<details><summary>Move 3</summary>

Set the same three variables in your shell before you build the adapter, and in a fixture for your tests.

</details>

<details><summary>Move 4</summary>

Does building or calling the adapter read the provider key? From your machine, that read can't reach LocalStack at its Compose address at all. Make sure the in-process path you use for `propose()` never triggers the key read.

</details>

### One run's proposals show up in another

<details><summary>Move 1</summary>

Build two fresh states and add a proposal to the first one's list. Does the second one change too?

</details>

<details><summary>Move 2</summary>

Where does a fresh state get its empty list? Is it the same list object every time?

</details>

<details><summary>Move 3</summary>

Give the list field a default that makes a new list for every state, the way your validation library lets a field build its default.

</details>

### A misspelled status gets through

<details><summary>Move 1</summary>

What type is your status field? Can you set it to any string?

</details>

<details><summary>Move 2</summary>

How did you make `ActionKind` a closed list in Task 1? Use the same construct for the status.

</details>

<details><summary>Move 3</summary>

Does your model check values when a field is assigned, or only when it's built? Try both in the shell.

</details>

### You can't decide where the script name lives

<details><summary>Move 1</summary>

Where could the script name live: in the state, or in the run configuration? List what each place means for a run started through the server and for one started in your shell.

</details>

<details><summary>Move 2</summary>

For each place, ask what it costs: what does the caller have to send, what does a resumed or replayed run see, and what does the record of the run show?

</details>

<details><summary>Move 3</summary>

Pick one place, keep it the same for every run, and write down why. It goes into your evidence note.

</details>

### The second call returns the first entry again

<details><summary>Move 1</summary>

Which field of the request picks the entry? Where does its value come from when the node builds the request?

</details>

<details><summary>Move 2</summary>

After the first call, what has changed in the state? Is that what you pass as the position?

</details>

<details><summary>Move 3</summary>

Does the position count from the base you chose in Task 1?

</details>

<details><summary>Move 4</summary>

The approach: the position in the request comes from the number of proposals the run already holds, adjusted to the base you chose in Task 1. A fresh run asks for the first entry, and each later call for the next one.

</details>

### The node changes the state but nothing sees it

<details><summary>Move 1</summary>

What does your node return?

</details>

<details><summary>Move 2</summary>

LangGraph applies what a node returns, not what it changes in place. Does your node append to the list it was given?

</details>

<details><summary>Move 3</summary>

The approach: return a mapping with three entries: a new list made of the old proposals plus the new one, the step count plus one, and the token total plus the proposal's count.

</details>

### The node never receives the run configuration

<details><summary>Move 1</summary>

How does LangGraph decide whether to pass the configuration to a node? Look at the parameters in the Graph API reference the lesson links.

</details>

<details><summary>Move 2</summary>

Compare your node's parameter names and types with that form. A different name means LangGraph doesn't pass it.

</details>

<details><summary>Move 3</summary>

Is your node a coroutine? It awaits the port's method, so it must be declared async, and you call it in the shell through `asyncio.run(...)`.

</details>
