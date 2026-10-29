# Step 1: Define what the agent remembers and make it ask the model

## The model adapter won't build in your shell or tests

<details><summary>Hint 1</summary>

What does the worker's settings object need before it can be constructed? Read the error: which variable does it name?

</details>

<details><summary>Hint 2</summary>

Inside the stack, `compose.yaml` gives the worker those values. On your machine nothing does. Find how an existing worker unit test supplies them, and do the same in your shell and in a fixture.

</details>

<details><summary>Hint 3</summary>

Does building or calling the adapter read the provider key? If it does, which service has to be running for that read to succeed, and how long does the first read wait?

</details>

## The second call returns the first entry again

<details><summary>Hint 1</summary>

Which field of the request picks the entry? Where does its value come from when the node builds the request?

</details>

<details><summary>Hint 2</summary>

After the first call, what in the state has changed? Is that the value you pass as the position, and does it count from the base you chose in Task 1?

</details>

## One run's proposals show up in another

<details><summary>Hint 1</summary>

Where does a brand-new state get its empty list from? Is it the same list object every time?

</details>

## The node changes the state but nothing sees it

<details><summary>Hint 1</summary>

What does your node return? LangGraph applies what a node returns, not what it changes in place.

</details>

<details><summary>Hint 2</summary>

Return the new list, the new count and the new total as values. Does your node build a new list, or append to the one it was given?

</details>

## The node never receives the run configuration

<details><summary>Hint 1</summary>

How does LangGraph decide whether to pass the configuration to a node? Compare your node's parameters with the form the Graph API reference shows.

</details>
