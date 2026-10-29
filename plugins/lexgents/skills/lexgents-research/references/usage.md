# Research commands and results

Use after authorization and login. Consult `lexgents research --help` for the installed version. Choose one input style and submit the user's question once; these are alternatives, not a sequence of research calls.

## Ask directly

For a short, nonsensitive question, use `--query` directly:

```bash
lexgents research --query '台灣勞基法規定的正常工時上限是多少？' --json
```

Pass the question as one literal argument, using an argument array when available or proper shell quoting. `--json` selects the output format; it does not require file input. Capture the exit code as well as the output.

Questions are limited to 2,000 characters in every input mode. Default jurisdiction/language are Taiwan/Traditional Chinese. Each call is independent, so include the relevant authorized facts deliberately; the CLI does not attach chat history or unrelated files.

## Long or sensitive questions

Command-line arguments can appear in shell history or process listings. For long or sensitive content, write the current authorized question to a private UTF-8 `question.txt`, then run:

```bash
lexgents research --query-file question.txt --json
```

`--query-file -` can read stdin. This is a way to pass the current question, not document ingestion or permission to scan existing files. Keep question and result files private. File input retains the same length limit and remote-processing authorization requirements; it does not keep the question local. Review [data handling](data-handling.md) before sending confidential material.

## Structured input

For explicit jurisdiction/language/tool selection, save a private `request.json` with a direct object, not an `input` wrapper. This is an illustrative request:

```json
{
  "query": "Under Taiwan Labor Standards Act Article 30 paragraph 1, what are the normal daily and weekly working-hour limits? Cite the official provision.",
  "jurisdiction": [{"country": "tw"}],
  "language": "en",
  "tool_list": ["legal_retrieval_agent"],
  "max_tool_call_num": 1
}
```

```bash
lexgents research --input-file request.json --json
```

Do not combine input-file with query-file or input override flags. US input requires a state, but accepted syntax does not promise jurisdiction coverage. A single-tool limit above is illustrative, not a universal research policy. Use the installed help for supported options and avoid changing cost-related settings beyond the user's scope.

## Read the result

Success requires process exit 0, `schema_version="1"`, `status=completed` and a nonempty `result.answer`. Read returned source arrays such as `legal_sources`, `judicial_sources`, `interpretation_sources` and `web_sources`; preserve producer fields and source identities. Sources may be absent, null or empty. Missing sources are not proof the relevant law does not exist, and successful execution does not establish legal correctness.

Assess the answer against the user's jurisdiction/facts and verify original sources when required. Label citations according to actual verification. Research output's `billing` is a preflight snapshot, not a post-run wallet balance. `thread_cleanup=deleted` means deletion was accepted, not that all remote logs/backups were erased.

## Errors and next steps

Always inspect `error.code` and `error.outcome_unknown`, not only the exit code. Unknown outcome takes priority: remote research may already have run and used Credits. Poll the same process if it is still running; otherwise stop automatic retries and preserve necessary correlation IDs privately for investigation. These IDs are not idempotency keys. Stopping a local process does not establish remote cancellation.

| Outcome | Next action |
|---|---|
| Missing/expired/rejected login | `lexgents login`, then continue only after a known no-submission outcome |
| `callback_unavailable` during login | Follow [callback troubleshooting](setup.md#browser-login); preserve other local services while resolving the registered-port conflict |
| `environment_conflict` or target/account mismatch | Resolve the configuration/account mismatch before sending data; do not substitute another endpoint or token |
| Invalid input/configuration | Follow installed help and correct the issue; retry only when no submission occurred |
| `payment_required` / exit 9 | Follow [billing](billing.md); no automatic payment or repeat of an unknown request |
| `billing_unavailable` / exit 10 | Eligibility is unknown; explain and check later, not a zero-balance/payment claim |
| Rate limit | Observe the service's guidance; no retry loop or unknown-outcome resubmission |
| API error, timeout, disconnect, interruption or invalid response | Inspect the structured outcome; no blind retry |
| `input_required` | Check installed help for continuation support; do not invent a resume command |
| Success with cleanup warning | Return the useful result and limitation; do not repeat research to clean up |

Keep raw output, confidential facts and account identifiers out of public issues. Provide the useful answer, citations and material limits in the user's language.
