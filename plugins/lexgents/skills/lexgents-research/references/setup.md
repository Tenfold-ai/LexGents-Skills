# Install and sign in

Use after the user authorizes setup or LexGents research. Requires macOS/Linux, a maintained tool installer, a supported Python runtime, shell/HTTPS access and a browser on the same machine.

## Install or update

With [uv](https://docs.astral.sh/uv/getting-started/installation/) available:

```bash
uv tool install --upgrade --prerelease disallow lexgents-cli
lexgents --version
lexgents research --help
```

Use the official `lexgents-cli` PyPI project and its `lexgents` executable. Check/update at the start of a new authorized workflow, then keep that version for the work. uv manages the private tool environment; do not build a venv or define a shell function yourself. Do not install a similarly named package, add TestPyPI as an extra index, or bypass TLS checks. Use a maintained installer with its normal official PyPI source; unexpected custom registry settings need resolution before installing.

If uv is missing, use its official installation instructions or the user's existing pipx setup. With pipx, use `pipx install lexgents-cli` for a first install and `pipx upgrade lexgents-cli` for an existing one. Do not switch managers over an existing installation or overwrite an unrelated executable. If the command is not on PATH, follow the installer's tool-directory guidance and start a new shell. Use the tool manager's installed executable, not a similarly named project script.

Compare `lexgents --version` with the current stable version on the [official PyPI project](https://pypi.org/project/lexgents-cli/). A resolver can silently select an older release compatible with the installed Python. If the versions differ or installation fails, stop and resolve the runtime/installer requirement before research; do not pin an older CLI to suppress the problem. Read installed help before applying examples; a changed command/response contract needs updated guidance rather than guessed flags.

## Browser login

```bash
lexgents auth status --json
lexgents login
```

Run login only when needed. The CLI supplies production configuration and privately stores its credentials; no AWS access, manual token, environment variables or copied JSON is required. Have the user complete browser login/MFA, using their existing Google SSO option when applicable. Never ask them to paste credentials into chat or inspect browser storage/callback codes.

Keep the login process alive and poll the same handle. Success requires exit 0, `signed_in=true` and the intended account/organization. The backend selects the user's existing default organization; `--organization-id` can check an intended existing organization, not grant membership. Resolve an account mismatch before research. `--no-browser` still requires a browser on the same computer; it is not remote-device login.

The CLI session has a fixed eight-day deadline. Authenticated commands refresh expiring JWTs automatically; refresh does not extend the session or restart the seven-day trial. After expiry/revocation, sign in again. Credentials remain owner-only local files, not an encrypted OS vault. Package upgrades keep this storage separate.

## Diagnose only when needed

Use `lexgents doctor --json` for service/account diagnosis and `lexgents billing status --json` for current trial/Credits eligibility. Research performs its own billing check before submission.

`environment_conflict` means stale credential/service overrides are present. Resolve the named configuration issue without printing secrets; do not invent a custom service target or read credential files. Operator-only `--config` and separate auth directories are documented by the CLI, outside normal external-user setup.

On sign-out, check `local_credentials_removed`, `cognito_refresh_revoked` and subsequent `signed_in=false`. Report a revocation failure; already-issued JWTs and the browser session may remain valid for their existing lifetimes.

For `invalid_credential_cache`, follow the CLI's logout-then-login guidance without reading the damaged file. Logout removes safely owned invalid local data but reports that remote refresh revocation was not completed.
