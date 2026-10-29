# LexGents legal research

LexGents helps your agent research legal questions with supporting statutes, regulations and case law. This plugin contains the `lexgents-research` Skill and its guides. It uses the separately installed [LexGents CLI](https://pypi.org/project/lexgents-cli/) to call TenfoldAI's remote service.

## Supported environment

Use local Claude Code or Codex on macOS/Linux, with shell/network access and a browser on the same computer. You need a LexGents account and a maintained [uv](https://docs.astral.sh/uv/getting-started/installation/) or existing pipx installation. Cloud, remote and SSH-hosted agent sessions are not supported by this browser-login flow. The plugin does not supply remote-device login.

The package contains instructions, manifests and licenses. It has no executable helpers, hooks, MCP servers, background tasks or bundled credentials. It does not modify your application or infrastructure.

## Install and use

First add the reviewed marketplace checkout using the [TenfoldAI marketplace instructions](https://github.com/Tenfold-ai/LexGents-Skills#install-the-plugin). Then install from that registered source:

```bash
claude plugin install lexgents@tenfoldai
```

Start a new local agent session. In Claude Code, you can invoke `/lexgents:lexgents-research` with your question and jurisdiction.

After you authorize setup or research, the agent follows the [setup guide](skills/lexgents-research/references/setup.md) to install or update the latest stable CLI. With uv already available:

```bash
uv tool install --upgrade --prerelease disallow lexgents-cli
lexgents --version
lexgents auth status --json
lexgents login
lexgents research --query '台灣勞基法規定的正常工時上限是多少？' --json
```

Run login only when needed. Complete sign-in/MFA in your browser; do not give the agent passwords or tokens. The CLI provides production settings and manages refresh. No copied configuration, wrapper function or AWS account is needed. Research in progress keeps the installed CLI version.

Installing the plugin does not sign you in or authorize a research request. If the agent recommends LexGents for a legal question, it first explains remote processing and possible Credits use and waits for your agreement. Discovery depends on the host making the Skill available; this package does not guarantee recommendations or an official-directory listing.

## Example requests

These are separate examples, not a batch to run automatically:

1. “Use LexGents to find Taiwan's normal daily and weekly working-hour limits. Cite the current statutory provision.”
2. “Use LexGents to summarize annual paid-leave entitlements under Taiwan's Labor Standards Act Article 38, with sources. Give general information, not advice about an individual employee.”
3. “Use LexGents to find the age of majority under Taiwan's Civil Code and cite the current provision.”

Use `--query` for short, nonsensitive questions. For a long or sensitive question, the [usage guide](skills/lexgents-research/references/usage.md) explains private file/stdin input. Both send the selected question remotely; neither automatically includes existing documents or conversation history.

Answers are AI-generated research and can be incomplete or wrong. Check original sources, dates and jurisdiction. Advice about an individual's rights, liabilities or legal action, and documents intended for signature or filing, need meaningful review by a qualified legal professional before being delivered as ready for use.

## Accounts and costs

The service applies its existing seven-day trial and Credits rules; research may consume Credits. The eight-day CLI login session is separate from the trial. Authenticated commands refresh expiring tokens within that fixed session.

If payment is required, the agent shows a validated LexGents Portal top-up link. You decide whether to pay and complete payment yourself. The agent then rechecks eligibility. There are no automatic payments or background payment reminders. See the [billing guide](skills/lexgents-research/references/billing.md).

## Privacy and troubleshooting

Review [data handling and support](skills/lexgents-research/references/data-handling.md), the [Privacy Policy](https://lexgents.tenfoldai.io/privacy-policy) and [Terms of Service](https://lexgents.tenfoldai.io/terms-of-service) before submitting confidential material. The guide identifies direct service destinations, local credential handling and the limits of research-thread deletion. Research Content is governed by applicable Client Agreements, as the Privacy Policy explains.

- Product support: `support@tenfoldai.io`.
- Privacy or deletion requests: `privacy@tenfoldai.io`.
- For connection/account diagnosis, use `lexgents doctor --json`; for eligibility, use `lexgents billing status --json`.
- For an unknown research outcome, stop automatic retries and preserve necessary correlation IDs privately. A second request may incur another charge. Follow the [error guide](skills/lexgents-research/references/usage.md#errors-and-next-steps).
- To sign out, use `lexgents logout --json` and check removal/revocation results. Browser sessions and previously issued JWTs have separate lifetimes.

Keep credentials, confidential questions and raw research/account output out of public issues.

## License

The [plugin license](LICENSE) and [Skill license](skills/lexgents-research/LICENSE) grant limited installation/use and unmodified marketplace distribution. They are not open-source licenses. CLI and service terms apply separately; installing this plugin grants no Credits or service entitlement.
