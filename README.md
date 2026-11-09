# teach-loop

A Claude Code skill that teaches you a topic over many sessions, using a persistent learning loop:

1. **Current knowledge**: what you know now
2. **Lesson**: one short lesson aimed at your next gap
3. **Assignment**: you apply the concept (coding topics come with starter code and tests)
4. **Updated state**: your progress is recorded in `STATE.md`
5. Back to step 1

Progress lives in your teaching workspace (`STATE.md`, `learning-records/`, `lessons/`, `assignments/`), so each new session picks up where the last one stopped.

## Install

Clone this repo into your personal Claude Code skills folder.

macOS, Linux or Git Bash:

```bash
gh repo clone polysius/teach-loop ~/.claude/skills/teach-loop
```

Windows PowerShell:

```powershell
gh repo clone polysius/teach-loop "$HOME\.claude\skills\teach-loop"
```

If the repo is private, sign in first with `gh auth login` as polysius.

## Use

Make an empty folder for the topic, open Claude Code in it, and run:

```
/teach-loop <what you want to learn>
```

Run `/teach-loop` with no topic to resume where you left off.

## Update

```bash
git -C ~/.claude/skills/teach-loop pull
```

## Credits

Based on the [`teach` skill](https://github.com/mattpocock/skills/tree/main/skills/productivity/teach) by Matt Pocock, used under the MIT License (see [LICENSE](./LICENSE)). This version adds `STATE.md`, assignments, and the learning loop.
