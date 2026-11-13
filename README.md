# n8n skill scanner — detector test fixture

`skills/scanner-test-fixture/SKILL.md` is an **inert** test fixture for the n8n skill security scanner
(`packages/cli/src/modules/skills/utils/skill-security-rules.ts`). It is the
skill-sync equivalent of the EICAR antivirus test file: it deliberately trips
every scan rule so you can confirm the scanner flags them on import.

Nothing here is a working attack. Links point at the reserved `example.com`
domain, tokens are placeholders, and the phrases are well-known public strings.

Rules exercised: `hidden_unicode`, `new_url`, `permission_change`,
`suspicious_phrase` (instruction override, hidden action, system-prompt
request, chat role tag, jailbreak mode, send-secrets, secret-in-command,
data-carrying image link).

## Skills in this repository

Link n8n to the repository root to import every skill at once.

| Folder | Skill | What it tests |
|---|---|---|
| `skills/scanner-test-fixture/` | `scanner-test-fixture` | Scan rules and skipped files (see below) |
| `skills/meeting-notes/` | `meeting-notes` | A clean skill with only `SKILL.md` |
| `skills/release-checklist/` | `release-checklist` | A clean skill with two references |
| `skills/sql-style-guide/` | `sql-style-guide` | A clean skill with one reference |
| `skills/support/billing/refund-policy/` | `refund-policy` | A clean skill three folders deep |

A skill folder inside another skill folder belongs to the outer skill. So no
`SKILL.md` sits at the repository root.

## File handling

A skill keeps only `SKILL.md` and markdown under `references/`. Skill sync
lists every other file as skipped. The expected result for each file in
`skills/scanner-test-fixture/`:

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
| `LICENSE` | skipped | `file_skipped`, info (no extension) |
| `references/UPPERCASE.MD` | skipped | `file_skipped`, info (path rule matches `.md` and `.markdown` only) |
| `references/nul-char.md` | skipped | `file_unstorable`, info (has a NUL character) |

All scripts only print a line. n8n never stores or runs them.
