# teach-loop

A Claude Code skill that teaches you a topic over many sessions, using a persistent learning loop:

1. **Current knowledge**: what you know now
2. **Lesson**: one short lesson aimed at your next gap
3. **Assignment**: you apply the concept (coding topics come with starter code and tests)
4. **Updated state**: your progress is recorded in `STATE.md`
5. Back to step 1

Progress lives in your teaching workspace (`STATE.md`, `learning-records/`, `lessons/`, `assignments/`), so each new session picks up where the last one stopped.

## Install

With Node.js installed, run:

```bash
npx skills@latest add polysius/teach-loop -g -a claude-code
```

`-g` installs it for your user, so `/teach-loop` works in every project; leave it off to install into the current project only. Leave off `-a claude-code` to choose other agents.

## Use

Make an empty folder for the topic, open Claude Code in it, and run:

```
/teach-loop <what you want to learn>
```

Run `/teach-loop` with no topic to resume where you left off.

## Update

```bash
npx skills@latest update -g
```

## Credits

Based on the [`teach` skill](https://github.com/mattpocock/skills/tree/main/skills/productivity/teach) by Matt Pocock, used under the MIT License (see [LICENSE](./LICENSE)). This version adds `STATE.md`, assignments, and the learning loop.
