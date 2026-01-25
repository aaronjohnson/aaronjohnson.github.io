# Keyless CI: Workload Identity Federation for GitHub Actions

*Stop storing JSON keys in GitHub Secrets. There's a better way.*

## The Problem

You want GitHub Actions to deploy to Google Cloud. The old way:

1. Create a service account
2. Download a JSON key
3. Paste it into GitHub Secrets
4. Hope nobody leaks it
5. Rotate it manually (you won't)

JSON keys are long-lived credentials. They don't expire. If leaked, attackers have access until you notice and revoke.

## The Solution: Workload Identity Federation

WIF lets GitHub Actions authenticate *without* a key. GitHub proves its identity to Google Cloud using OIDC tokens. No secrets to store, rotate, or leak.

### Step 1: Create the Workload Identity Pool

```bash
gcloud iam workload-identity-pools create "github-actions-pool" \
  --project="YOUR_PROJECT_ID" \
  --location="global" \
  --display-name="GitHub Actions Pool"
```

### Step 2: Add GitHub as a Provider

```bash
gcloud iam workload-identity-pools providers create-oidc "github-provider" \
  --project="YOUR_PROJECT_ID" \
  --location="global" \
  --workload-identity-pool="github-actions-pool" \
  --display-name="GitHub Provider" \
  --attribute-mapping="google.subject=assertion.sub,attribute.actor=assertion.actor,attribute.repository=assertion.repository" \
  --issuer-uri="https://token.actions.githubusercontent.com"
```

### Step 3: Create a Service Account

```bash
gcloud iam service-accounts create "github-actions-deploy" \
  --project="YOUR_PROJECT_ID" \
  --display-name="GitHub Actions Deploy"
```

### Step 4: Allow GitHub to Impersonate It

```bash
gcloud iam service-accounts add-iam-policy-binding \
  "github-actions-deploy@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
  --project="YOUR_PROJECT_ID" \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/github-actions-pool/attribute.repository/YOUR_ORG/YOUR_REPO"
```

Replace `YOUR_ORG/YOUR_REPO` with your GitHub repository (e.g., `aaronjohnson/step_quest`).

### Step 5: Use It in GitHub Actions

```yaml
permissions:
  contents: read
  id-token: write  # Required for WIF

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: 'projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/github-actions-pool/providers/github-provider'
          service_account: 'github-actions-deploy@YOUR_PROJECT_ID.iam.gserviceaccount.com'

      # Now you're authenticated. Use gcloud, deploy, etc.
```

## Gotchas

- **`id-token: write`** — You must add this permission or it silently fails
- **Repository binding** — The service account is locked to your specific repo
- **Project number vs ID** — The provider path uses the *number*, not the project ID
- **First run takes longer** — Token exchange adds ~5 seconds

## Why Bother?

| JSON Key | WIF |
|----------|-----|
| Never expires | Tokens last 1 hour |
| One secret to leak | Nothing to leak |
| Manual rotation | Automatic |
| Works from anywhere | Only works from your repo |

The last point is the real win: even if someone steals your workflow file, they can't authenticate from their own repo.

---

*No keys. No secrets. No rotation. Just auth that works.*

**Want this as a Claude skill?** See [wif.md](https://github.com/aaronjohnson/step_quest/blob/shoes/.claude/skills/wif.md) — say "set up WIF" and Claude handles the rest.
