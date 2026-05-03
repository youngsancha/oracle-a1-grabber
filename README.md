# oracle-a1-grabber

Oracle Cloud Always Free **A1.Flex** (4 OCPU / 24 GB ARM) capacity polling via GitHub Actions cron.

A1 capacity in Phoenix is heavily contested — manual retry frustrating. This workflow tries every 5 min across all 3 ADs, creates a GitHub Issue (→ email notification) the moment it succeeds, then auto-disables itself.

## Setup recap

- **Public repo** → unlimited GitHub Actions minutes
- **Cron**: `*/5 * * * *` (best-effort, actual ~5–15 min)
- **Notification**: GitHub Issue + auto email to repo owner
- **Auto-disable on success** — no infinite polling

## Secrets

| Name | Source |
|---|---|
| `OCI_USER_OCID` | OCI console → My Profile → OCID |
| `OCI_TENANCY_OCID` | OCI console → Tenancy details |
| `OCI_FINGERPRINT` | API key fingerprint |
| `OCI_REGION` | `us-phoenix-1` |
| `OCI_PRIVATE_KEY` | `.pem` file from OCI API key generation |
| `OCI_SSH_PUBLIC_KEY` | `~/.ssh/id_ed25519.pub` for VM access |

## Manual trigger

```powershell
& 'C:\Program Files\GitHub CLI\gh.exe' workflow run poll-a1.yml --repo youngsancha/oracle-a1-grabber
```

## Re-enable after manual disable

```powershell
& 'C:\Program Files\GitHub CLI\gh.exe' workflow enable poll-a1.yml --repo youngsancha/oracle-a1-grabber
```
