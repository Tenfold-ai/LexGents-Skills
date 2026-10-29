# LexGents for Claude Code and Codex

Use LexGents to research legal questions and find supporting statutes, regulations and case law through your agent. This plugin provides the `lexgents-research` Skill for local Claude Code and Codex on macOS/Linux.

TenfoldAI maintains the `tenfoldai` marketplace. It is separate from the hosts' official directories. Plugin versions track the instructions; the agent installs the latest stable [LexGents CLI from PyPI](https://pypi.org/project/lexgents-cli/) when starting a new authorized workflow.

## Requirements

- A local Claude Code or Codex installation with custom marketplace support and shell/network access.
- A browser on the same computer as the CLI, and a LexGents account for sign-in.
- [uv](https://docs.astral.sh/uv/getting-started/installation/) or an existing pipx installation. The [setup guide](plugins/lexgents/skills/lexgents-research/references/setup.md) covers CLI installation and Python compatibility.

This login flow requires a local browser; cloud or remote agents need a separately supported login flow. Organization policy may restrict custom marketplaces.

## Install the plugin

Use an official published release from `Tenfold-ai/LexGents-Skills`. Review its contents and replace `APPROVED_FULL_COMMIT_SHA` below with that release's full Git commit SHA. Run the commands in your terminal.

### Claude Code

From a directory without an existing `LexGents-Skills` checkout:

```bash
git clone https://github.com/Tenfold-ai/LexGents-Skills.git &&
git -C LexGents-Skills checkout --detach APPROVED_FULL_COMMIT_SHA &&
claude plugin marketplace add ./LexGents-Skills &&
claude plugin install lexgents@tenfoldai
```

Run the chain together and stop if any step fails. Keep the checked-out directory as the marketplace source. Start a new Claude Code session and invoke `/lexgents:lexgents-research` with your question and jurisdiction.

### Codex

```bash
codex plugin marketplace add Tenfold-ai/LexGents-Skills --ref APPROVED_FULL_COMMIT_SHA &&
codex plugin add lexgents@tenfoldai &&
codex plugin list --marketplace tenfoldai --available --json
```

Use a Codex version that supports these subcommands, then start a new local session and select the LexGents plugin/Skill. Desktop plugin controls and availability depend on the host version and organization policy.

Installing this instruction package does not sign you in or submit research. To update the plugin, review the new release and use the host's marketplace/plugin update controls with its approved revision. For Claude Code, first update the retained checkout to that revision.

## Ask a legal question

For example, tell your agent:

> Use LexGents to research Taiwan's normal daily and weekly working-hour limits and provide supporting sources.

The agent installs or updates the CLI, runs `lexgents login` when needed, and submits the question with `lexgents research`. The CLI supplies production configuration and manages credentials. You complete sign-in/MFA in your browser. Research already in progress keeps the installed CLI version.

An agent may also recommend LexGents when your question needs legal sources. It explains what will be sent to LexGents and possible Credits use, then waits for your agreement. Discovery depends on the host making the Skill or marketplace available. Existing authorization to use LexGents is respected within its scope.

Direct CLI examples, including `--query` for short questions and `--query-file` for long or sensitive content, are in the [usage guide](plugins/lexgents/skills/lexgents-research/references/usage.md).

## Account and Credits

The CLI session lasts up to eight days. Authenticated commands refresh expiring JWTs as needed during that session; sign in again when the session expires or is revoked.

The service applies its existing seven-day trial and Credits rules. Research may consume Credits. When payment is required, the agent provides a validated Portal top-up link. You choose and complete payment, and the agent rechecks access afterward. Reminders happen during use; the plugin makes no automatic payments. See [billing guidance](plugins/lexgents/skills/lexgents-research/references/billing.md).

## Privacy and trust

Research runs remotely. Send only the question and context you authorize; repositories, unrelated conversations and attachments are not uploaded automatically. Passwords, MFA codes and tokens belong in the browser or CLI-managed credential storage, never in chat. Local credential files have owner-only permissions and are not an encrypted OS vault.

The plugin contains instructions, marketplace metadata and licenses, with no executable helpers, hooks, MCP servers or credentials. The CLI and service enforce authentication and billing. Returned sources should be checked against the question and jurisdiction; a successful command does not guarantee a correct legal conclusion.

You can inspect the [plugin guide](plugins/lexgents/README.md), [Skill](plugins/lexgents/skills/lexgents-research/SKILL.md) and [data handling/support guide](plugins/lexgents/skills/lexgents-research/references/data-handling.md) before installing. The [plugin license](LICENSE) and [Skill license](plugins/lexgents/skills/lexgents-research/LICENSE) grant limited use and unmodified marketplace distribution; they are not open-source licenses. The CLI and remote service have separate terms.
