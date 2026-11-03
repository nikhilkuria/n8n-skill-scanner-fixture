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
