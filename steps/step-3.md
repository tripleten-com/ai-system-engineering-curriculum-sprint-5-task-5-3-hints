# Step 3: Put the agent in the stack and run a script through it

## `poe start` waits forever on the agent server

<details><summary>Hint 1</summary>

What does the service's log say? Run `docker compose --profile observability --profile localstack logs` for your service and read the last lines before it stopped.

</details>

<details><summary>Hint 2</summary>

Look for three usual causes in the log: the server can't find `langgraph.json` from its working directory, an import of your code fails inside the container, or the healthcheck asks for an address the server isn't listening on.

</details>

<details><summary>Hint 3</summary>

From inside the container, which address is the server bound to? Can a request from outside the container, or from the healthcheck, reach that address?

</details>

## The image is missing the agent's packages

<details><summary>Hint 1</summary>

Which dependency groups does the worker's image install? Is the group that holds the LangGraph packages among them?

</details>

<details><summary>Hint 2</summary>

Read `infra/containers/worker.Dockerfile` and find the line that installs dependencies. What would an image need to install for the agent server?

</details>

## The server logs a `BlockingError`

<details><summary>Hint 1</summary>

Which call in your node blocks? Read the stack trace in the log: is it your script read, or the key read in the worker's settings?

</details>

<details><summary>Hint 2</summary>

The lesson gives two routes. Which one covers a blocking call in code you didn't write?

</details>

## The emulator can't find your script inside the server

<details><summary>Hint 1</summary>

Where is the script on your machine, and where is it inside the container? Does the image carry that folder at all? Read `.dockerignore`.

</details>

<details><summary>Hint 2</summary>

Print your service's resolved configuration and check what it mounts and which path the adapter is given. Is that path valid inside the container?

</details>

## `poe ready` passes, but your service isn't up

<details><summary>Hint 1</summary>

Which services does `poe ready` check? Read what it prints. Is the agent server among them?

</details>

<details><summary>Hint 2</summary>

Which check waits for your service, then? Look at what `poe start` waits for and what `docker compose ps` shows for your service.

</details>

## A run uses your old code

<details><summary>Hint 1</summary>

When did the server load your graph? Did you restart or rebuild your service after your last change?

</details>

## Other things you may see

<details><summary>The server warns that its API version is End of Life</summary>

The lesson says this warning is expected with the pinned packages. It doesn't stop a run.

</details>

<details><summary>`poe compose-config` prints nothing</summary>

That's its passing result. To see your service with every value filled in, print that one service's resolved configuration with `docker compose config`, as the lesson shows.

</details>

<details><summary>The CLI's help text crashes on Windows</summary>

Read the error: is it an encoding error while printing? Ask how your terminal's encoding can be set to UTF-8 for that one command.

</details>
