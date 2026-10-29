# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Did you meet the Task's requirements?

- Is the status a closed list, so a misspelled status can't reach the rule?
- Does a tool call or an approval request end the run `refused` with `not_enabled`, with nothing called and nothing written?
- Does the recovery-shaped run stop at its first entry and never read the rest?
- Does your service merge the worker's environment source instead of copying values?
- Does `poe demo` create its own exception, print the summary last, and exit non-zero when the run doesn't end `finished`?

## Did you explain your design choices?

- Does your note say where the script name lives, and why there?
- Does it say whether the state carries a run id of its own?
- Does it say how the server gets its code and reaches your scripts, and how the demo decides its exit code?

## Can your tests fail when the code is wrong?

- Do you test the rule in both directions?
- Did you break each group once and see a test fail, including a node registered under the wrong name?
- Did you run the demo once with the refusal script and see it fail?

## Did you state the limits of what your checks show?

- Does your note say that every run here was answered by the emulator, and what that can't tell you about a real model?
