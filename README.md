# n8n skill scanner — detector test fixture

`SKILL.md` is an **inert** test fixture for the n8n skill security scanner
(`packages/cli/src/modules/skills/utils/skill-security-rules.ts`). It is the
skill-sync equivalent of the EICAR antivirus test file: it deliberately trips
every scan rule so you can confirm the scanner flags them on import.

Nothing here is a working attack. Links point at the reserved `example.com`
domain, tokens are placeholders, and the phrases are well-known public strings.

Rules exercised: `hidden_unicode`, `new_url`, `permission_change`,
`suspicious_phrase` (instruction override, hidden action, system-prompt
request, chat role tag, jailbreak mode, send-secrets, secret-in-command,
data-carrying image link).

## File handling

A skill keeps only `SKILL.md` and markdown under `references/`. Skill sync
lists every other file as skipped. The expected result for each file:

| File | Result | Finding |
|---|---|---|
| `SKILL.md` | kept | — |
| `references/usage.md` | kept | — |
| `references/advanced/tips.md` | kept | — |
| `scripts/setup.sh` | skipped | `file_skipped`, needs review (script) |
| `scripts/fetch_data.py` | skipped | `file_skipped`, needs review (script) |
| `scripts/run.ps1` | skipped | `file_skipped`, needs review (script) |
| `lib/helper.ts` | skipped | `file_skipped`, needs review (script) |
| `assets/config.json` | skipped | `file_skipped`, info |
| `assets/logo.svg` | skipped | `file_skipped`, info |
| `templates/report.txt` | skipped | `file_skipped`, info |
| `docs/notes.md` | skipped | `file_skipped`, info (markdown outside `references/`) |
| `README.md` | skipped | `file_skipped`, info |
| `LICENSE` | skipped | `file_skipped`, info (no extension) |
| `references/UPPERCASE.MD` | skipped | `file_skipped`, info (path rule matches `.md` and `.markdown` only) |
| `references/nul-char.md` | skipped | `file_unstorable`, info (has a NUL character) |

All scripts only print a line. n8n never stores or runs them.
