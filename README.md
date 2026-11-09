# Readable

A communication style for AI agents, packaged as a skill: messages and reports a person can understand on the first read.

This is my own style, written down for agents, and it is opinionated on purpose. I'd rather keep it specific to what I need than water it down for everyone. If it suits you too, great.

## What it fixes

Coding agents often write messages that make sense to them but not to the reader. The skill targets these problems:

- **Unclear results.** You can't tell what was done, what was checked, and what it means.
- **Reflexive disclaimers.** "I did X, but I didn't do Y, and this doesn't prove Z..." The one caveat that matters gets buried.
- **Words that sound smart.** "Leverage", "surface", "orthogonal", "non-trivial", where a plain word would do.
- **Unexplained terms.** Jargon, function names, internal labels and abbreviations used as if the reader already knew them.
- **Missing information.** The reader has to scroll back, open files or ask to understand the message.
- **No conclusion.** A list of observations, but no "so this means...".
- **Missing objects and reasons.** "Checked ten candidates" without saying candidates for what, or a list of actions without explaining why they help achieve the goal.

It also guards against padding, repetition and oversimplifying until the facts are wrong. Familiar context stays when it helps the reader understand why the work was needed.

The skill is not about saving tokens. Messages written in this style are sometimes longer, because explanations and conclusions take words. The rule is to never cut meaning to save length, and never add words that carry no meaning.

## Example

Without the skill:

> I conducted a comprehensive investigation of the latency regression. Leveraging p99 telemetry, I observed a non-trivial uplift correlated with the PR-4821 rollout window. The pool saturation hypothesis appears plausible, although the evidence is correlational, I did not perform a controlled rollback, and the sample window is limited.

With the skill:

> The slowdown is most likely caused by last Tuesday's change that cut the database connection pool from 50 connections to 10 (PR 4821). The slowest 1% of requests went from about 200 ms to 1.4 s, starting within ten minutes of that change going live, and during slow periods all 10 connections are busy. I haven't proven it by undoing the change. Setting the pool back to 50 is a one-line change that is easy to reverse, and it would confirm the cause.

## What's in this repo

- [`skills/readable/SKILL.md`](skills/readable/SKILL.md): the full guide. It has 12 rules with explanations, a worked example and a pre-send check the agent runs on its draft. The folder follows the open [Agent Skills](https://agentskills.io) format, so Claude Code, Codex and other agents that support the format can load it.
- [`always-on.md`](always-on.md): a short version of the rules (about 20 lines) that you can add to your agent's global instructions file. That file is loaded in every session, so the rules apply even when the agent doesn't load the skill.

## Install

There are two ways to install the skill. Either one works with all three ways of using it, described in the next section.

- **Option 1, the skills CLI**, is the quickest. It installs only the skill.
- **Option 2, cloning the repo**, also gives you a local copy of `always-on.md`, so in Claude Code the always-on rules can update together with the repo. Choose it if you plan to use mode (a) in Claude Code, or if you want to edit the skill.

### Option 1: the skills CLI

[skills](https://github.com/vercel-labs/skills) is Vercel's command-line tool for installing agent skills, and it powers the [skills.sh](https://skills.sh/alexeymalafeev/readable/readable) catalog. It needs Node.js 22.20 or later.

```bash
npx skills add AlexeyMalafeev/readable -g -a claude-code -a codex
```

- `-g` installs the skill for all your projects. Without it, the skill goes into the current project only.
- `-a` picks the agents. Keep only the ones you use, or leave out `-a` to choose from a list.

The command puts the skill in `~/.agents/skills/readable`, the folder Codex reads, and links `~/.claude/skills/readable` to it for Claude Code.

### Option 2: clone the repo and link the skill

The commands assume you clone the repo to `~/readable`; if you put it somewhere else, change the paths to match.

1. Clone the repo:

   ```bash
   git clone https://github.com/AlexeyMalafeev/readable.git ~/readable
   ```

2. Link the skill folder into your agent's skills folder. A link (rather than a copy) means a `git pull` updates the skill.

   Claude Code:

   ```bash
   mkdir -p ~/.claude/skills
   ln -s ~/readable/skills/readable ~/.claude/skills/readable
   ```

   Codex:

   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/readable/skills/readable ~/.agents/skills/readable
   ```

## Choose how to use it

There are three ways to use the skill, depending on how much you want it to change your agent's behavior.

| Way | Skill installed | Global instructions changed | Who decides when the skill is used |
| :-- | :-- | :-- | :-- |
| (a) Eager | Yes | Yes | It's always on: the short rules apply in every session |
| (b) Conservative | Yes | No | The agent, when it judges a task needs it, or you, by calling it |
| (c) Full control | Yes | No | Only you, by calling it |

### (a) Eager: always on, everywhere

Use this if you want every agent message in every project written this way. Install the skill, then add the short rules to your global instructions.

Claude Code, if you cloned the repo: add an import line to `~/.claude/CLAUDE.md`. Claude Code loads the imported file at the start of every session, so a `git pull` updates the rules too.

```bash
printf '\n@~/readable/always-on.md\n' >> ~/.claude/CLAUDE.md
```

Claude Code, if you installed with the skills CLI, or you'd rather review changes before they reach your instructions: append a copy of the rules instead.

```bash
{ echo; curl -fsSL https://raw.githubusercontent.com/AlexeyMalafeev/readable/main/always-on.md; } >> ~/.claude/CLAUDE.md
```

Codex: Codex can't import files, so append a copy of the rules to `~/.codex/AGENTS.md`. If you cloned the repo:

```bash
mkdir -p ~/.codex
{ echo; cat ~/readable/always-on.md; } >> ~/.codex/AGENTS.md
```

If you installed with the skills CLI:

```bash
mkdir -p ~/.codex
{ echo; curl -fsSL https://raw.githubusercontent.com/AlexeyMalafeev/readable/main/always-on.md; } >> ~/.codex/AGENTS.md
```

Codex reads only one global instructions file. If you have a `~/.codex/AGENTS.override.md`, add the rules there instead. If you've set `CODEX_HOME`, use that folder instead of `~/.codex`.

Run each command only once, because running it again adds a second copy. The copied block starts with a `readable:start` comment and ends with a `readable:end` comment, so you can find it later to update or remove it.

### (b) Conservative: installed, but your global instructions stay as they are

Use this if you don't want to change your global instructions. After installing, there are two ways the skill gets used:

- The agent loads it on its own when a task matches the skill's description, for example when writing a report or a summary. How often that happens is up to the agent, and it may skip the skill for short replies.
- You call it yourself: type `/readable` in Claude Code or `$readable` in Codex. Once loaded, the skill usually keeps applying for the rest of the session. If the agent slips back into its old style in a long session, call it again.

To make it always on in just one project, add the contents of `always-on.md` to that project's `CLAUDE.md` or `AGENTS.md` instead of your global file.

### (c) Full control: only when you call it

Use this if you want the skill only for specific tasks, such as writing an experiment report, and never on the agent's own initiative. Install the skill, then turn off automatic loading. Each agent does this differently.

Claude Code: add this to `~/.claude/settings.json`, merging it with any settings already there. This hides the skill from Claude but keeps `/readable` working, and it doesn't change the skill's files.

```json
{
  "skillOverrides": {
    "readable": "user-invocable-only"
  }
}
```

To do this for a single project instead, run `/skills`, highlight `readable`, press Space until it shows `user-only`, then press Esc to save.

Codex: create an `agents/openai.yaml` file inside the skill folder that turns off automatic loading. `$readable` still works. These commands work with both install options; with option 2, `~/.agents/skills/readable` is a link to the cloned repo.

```bash
mkdir -p ~/.agents/skills/readable/agents
cat > ~/.agents/skills/readable/agents/openai.yaml <<'EOF'
policy:
  allow_implicit_invocation: false
EOF
```

If you installed with the skills CLI, an update replaces the whole skill folder and deletes this file, so run these commands again after each update.

Then call the skill when you need it, for example: `/readable write up the results of today's experiment` in Claude Code, or `$readable write up the results of today's experiment` in Codex.

## What it covers

When the skill is active, it applies to everything written for a person to read: chat replies, reports, status updates, summaries, handoff notes and PR descriptions. Formats that have their own project rules, such as commit messages and changelogs, keep following those rules. Your explicit instructions about length or format always win over the skill.

## Updating

If you installed with the skills CLI:

```bash
npx skills update -g readable
```

If you cloned the repo:

```bash
git -C ~/readable pull
```

Either way, the skill itself updates right away. In mode (a), the Claude Code import picks up the new rules in your next session. Copies of the rules don't update by themselves: if `always-on.md` changed, replace the block between the `readable:start` and `readable:end` comments in your `CLAUDE.md` or `AGENTS.md`.

## Removing

1. Mode (c): delete the `readable` entry from `skillOverrides` in `~/.claude/settings.json`. For Codex, delete `~/.agents/skills/readable/agents/openai.yaml`.
2. Mode (a): delete the `@~/readable/always-on.md` line from `~/.claude/CLAUDE.md`, or the block between the `readable:start` and `readable:end` comments from your `CLAUDE.md` or `AGENTS.md`.
3. Remove the skill. If you installed with the skills CLI, run `npx skills remove --global readable`, which removes it from both agents. If you cloned the repo, run `rm ~/.claude/skills/readable` for Claude Code and `rm ~/.agents/skills/readable` for Codex. That removes only the links; delete `~/readable` too if you no longer need it.

## Other agents

The skills CLI can install the skill for many other agents too: see its [list of supported agents](https://github.com/vercel-labs/skills/blob/main/docs/agents.md) for the names to pass to `-a`. Any agent that supports the [Agent Skills](https://agentskills.io) format can also use the `skills/readable` folder directly; link it into that agent's skills folder. The short rules in `always-on.md` work in any agent's global instructions file.

## License

MIT. See [LICENSE](LICENSE).
