# Agent Skill Localizer

English | [简体中文](README.zh-CN.md)

Translate descriptions and starter prompts while keeping familiar display names for search. **Release Notes** stays **Release Notes** by default; ask to translate display names too and it can appear as **发布说明**. The skill ID stays `release-notes` either way. The localizer is a single English [SKILL.md](skills/skill-localizer/SKILL.md) with no Python dependency or separate translation service.

## Compatibility

**Running the localizer:** Codex and other hosts that load the Agent Skills format, such as Claude Code, can read these instructions. With read access, a host can inventory skills and preview translations. Applying or restoring changes also requires the file tools and permissions described in the skill.

**Displaying the translation:** The only output format supported today is OpenAI's `agents/openai.yaml`. Installing the localizer in Claude Code does not translate its `/` menu: its command names come from skill frontmatter or directory names, which this skill never edits. An agent can prepare metadata for another client without displaying it in its own UI.

## Install

**Ask Codex (recommended):**

```text
Use $skill-installer to install skills/skill-localizer from the GitHub repository oiltea94/skill-localizer.
```

**Or run one command** from your target project (requires `npx` for installation only):

```sh
npx skills add oiltea94/skill-localizer --skill skill-localizer --agent codex
```

Add `--global` for a user-level install. Both GitHub methods require the skill directory to be published. To try an unpublished checkout, run `npx skills add /absolute/path/to/skills/skill-localizer --agent codex` from a separate project. For a manual install, the only file needed is `.agents/skills/skill-localizer/SKILL.md` under your project or home directory.

For Claude Code, use `--agent claude-code` in the `npx` command instead. This selects the installation host, not the output format. The manual path above is for Codex.

## Use it

For example, in Codex:

> Use $skill-localizer to scan this project's `.agents/skills` directory. Which skills still need Brazilian Portuguese UI text? Report only.

> Translate the missing entries into Brazilian Portuguese, keeping the display names unchanged. Show me the changes first.

To opt in, say “Translate the display names too.” If you do not specify a preference, the preview offers that option once for the batch. Approving the default preview leaves names alone, including any previously translated names.

> Restore `release-notes` in the same directory. Show me which file would be restored or removed.

In Claude Code, try a read-only request: `/skill-localizer List the skills in this project's .claude/skills directory and preview Japanese display names. Do not write any files.`

You can also request a source-change review or a batch restore. The conversation language and the target language can differ. Switching the target language changes only the selected fields, not the skill ID or instructions.

## What gets written?

An inventory or preview writes nothing. After approval, the agent can create or update `interface.short_description` and `interface.default_prompt` in each target's `agents/openai.yaml`. Unless you also request display-name translation, an existing `interface.display_name` stays unchanged and a missing one stays absent. Other configuration and all managed `SKILL.md` files stay untouched.

Single-file **installation** does not mean backup-free operation. Before writing, the agent records the original bytes (or prior absence) and hashes under `.skill-localizer/v1/` in the authorized root. Keep those backups private. Restore uses the baseline from disk, even in another session; it does not regenerate the original or switch back one language. If the YAML was absent originally, only the newly created YAML is removed.

These are agent instructions, not a transaction engine. If safe parsing, backup, or verification is unavailable, the result is a preview only. Manually edited targets, ambiguous histories, and older incompatible backups are not overwritten. A batch can complete partially; check the completion report.
