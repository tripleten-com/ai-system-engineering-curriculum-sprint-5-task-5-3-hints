# Step 3: Put the agent in the stack and run a script through it

## The idea

This Step leans on three container ideas the lesson takes for granted.

- **Image or mount:** an image is a frozen copy of files, built once. A container started from it sees exactly those files, whatever you change on your machine afterwards. A mount is a window: a folder from your machine appears inside the container, live. Your agent server gets its code one way or the other, and that decides what a code change needs: a rebuild, or a restart. Data the image doesn't carry, such as your scripts, needs a mount either way.
- **Where a server listens:** inside a container, `localhost` means the container itself. A server bound only to it can't be reached by a port mapping from your machine, or by a health check run from outside its process. Binding to all interfaces makes it reachable.
- **A YAML merge:** `compose.yaml` keeps the worker's settings in a named block, an anchor. Another service can merge that whole block in with one line instead of copying the values. Then a change to the block reaches every service that merges it, and two copies can never drift apart.

With those three in mind, giving the agent server "the same reach as the worker" becomes concrete: the same merged settings, the same dependencies, its code from an image or a mount, and a port and health check that actually reach the server.

> Covered in **3.2 Running a Graph Behind a Server**, and in **Compose: Running the Stack Together** and **Configuration, Secrets, and Health Checks**.

## What's expected

- `langgraph.json` at the repository root maps a graph id to your graph-building function and lists the project itself as a dependency.
- `docker compose --profile observability --profile localstack config` for your service shows these values, all resolved:
  - its image or build
  - the shared environment
  - what it depends on
  - its mounts
  - its health check
- `poe start` waits for your service, and `docker compose ps` shows it healthy.
- Your dev command prints these, in order:
  - a run id
  - one line per step with the node and the action kind
  - the final status, and the halt reason when there is one
- The three scripts end through the server as they did in-process, and the exception still reads `NEEDS_REVIEW` afterwards.

## When you're stuck

### `poe start` waits forever on the agent server

<details><summary>Hint 1</summary>

Read your service's log. What's the last thing it printed before `poe start` gave up?

</details>

<details><summary>Hint 2</summary>

Look for three usual causes:
- the server can't find `langgraph.json` from its working directory
- an import of your code fails inside the container
- the health check asks an address the server isn't listening on

</details>

<details><summary>Hint 3</summary>

Inside the container, which address is the server bound to? Can a request from the health check, or from your machine through the port mapping, reach that address?

</details>

<details><summary>Hint 4</summary>

The approach: start the server bound to all interfaces with no browser, point the health check at the server's own health route, and map the port to your machine. Then rebuild and start again.

</details>

### The server can't load your graph

<details><summary>Hint 1</summary>

Read the service's log from the start. Which module path and function name did the server try to load, and what error came back?

</details>

<details><summary>Hint 2</summary>

Open the configuration file reference the lesson links. Which form does a graph entry take, and which key puts the project itself on the server's import path?

</details>

<details><summary>Hint 3</summary>

Check the path in your file against where your graph-building function actually lives, as seen from the container's working directory, not from your machine.

</details>

<details><summary>Hint 4</summary>

The approach: one graph entry that names the module's path and the function that builds and compiles the graph, and the project itself listed as the dependency. Restart the service and read the log again.

</details>

### Your dev command can't show the steps

<details><summary>Hint 1</summary>

Open the server's API documentation at its own URL once it's up. Which calls create a thread, start a run on it, wait for it, and read its final state?

</details>

<details><summary>Hint 2</summary>

Where does each step's node and proposal come from: the run's final state, the stream of updates, or the thread's saved history?

</details>

<details><summary>Hint 3</summary>

Which id do you print first, and where does the server return it?

</details>

<details><summary>Hint 4</summary>

The approach: create a thread, start a run with the exception id as input and your script in the configuration, then either stream the node updates or read the thread's history when it finishes. Print the run id, one line per step with the node and the action kind, then the final status and the halt reason.

</details>

### The image is missing the agent's packages

<details><summary>Hint 1</summary>

Which dependency groups does the worker's image install? Read the install line in `infra/containers/worker.Dockerfile`.

</details>

<details><summary>Hint 2</summary>

Which group in `pyproject.toml` holds the LangGraph packages?

</details>

<details><summary>Hint 3</summary>

The approach: a Dockerfile of your own beside the worker's, built the same way, but installing the worker group and the agent group. Give the service a build entry like the worker's, so `poe start` builds it.

</details>

### The emulator can't find your script inside the server

<details><summary>Hint 1</summary>

Where are your scripts on your machine, and where would they be inside the container?

</details>

<details><summary>Hint 2</summary>

Read `.dockerignore`. Does the image carry that folder at all?

</details>

<details><summary>Hint 3</summary>

Print your service's resolved configuration. What does it mount, and is the scripts path the adapter uses valid inside the container?

</details>

<details><summary>Hint 4</summary>

The approach: mount the scripts folder into the container read-only, at the path the emulator's default points to, or pass that path to the adapter in its settings.

</details>

### The server logs a `BlockingError`

<details><summary>Hint 1</summary>

Which call in your node blocks? Read the stack trace: is it your script read, or the key read inside the worker's settings?

</details>

<details><summary>Hint 2</summary>

The lesson gives two ways out. Which one works for blocking code you didn't write and can't move to a thread?

</details>

<details><summary>Hint 3</summary>

Choose one, apply it, restart the service, and write the choice down for your evidence note.

</details>

### A run uses your old code

<details><summary>Hint 1</summary>

When did the server load your graph?

</details>

<details><summary>Hint 2</summary>

Does your service get the code from its image or from a mount? Either way, did it restart after your last change?

</details>

<details><summary>Hint 3</summary>

Rebuild or restart your service after every change to the agent's code, then run again.

</details>

### Other things you may see

<details><summary>Hint 1</summary>

**The server warns that its API version is End of Life.** That's expected with the pinned packages, and it doesn't stop a run.

</details>

<details><summary>Hint 2</summary>

**<code>poe ready</code> passes, but your service isn't up.** `poe ready` checks the platform's services only. Your service's health comes from its own health check, which `poe start` waits on.

</details>

<details><summary>Hint 3</summary>

**<code>poe compose-config</code> prints nothing.** That's its passing result. Print your one service's resolved configuration to see its values.

</details>

<details><summary>Hint 4</summary>

**The CLI's help text crashes on Windows.** Read the error. If it's an encoding error while printing, set your terminal to UTF-8 for that one command.

</details>
