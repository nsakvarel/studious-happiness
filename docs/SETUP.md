# One-time setup

The repo contains every stage that can live in code. These steps need account access and must be done by a repo/Azure admin.

## 0. Create `main` and make it the default branch
The workflows trigger on `main`, but this repo's default branch is currently `claude/add-claude-documentation-CAwJT`.
`main` now exists. Make it the default: **Settings → General → Default branch → `main`**.
Until then, `@claude` comment mentions and the weekly CodeQL scan do nothing — GitHub only runs `issue_comment`, review-comment and `schedule` triggers from workflows on the default branch.

## 1. Plan / Design integrations (stages 01–02)
- **Jira:** install *GitHub for Jira* from the Atlassian Marketplace and connect this repo. Put Jira keys (e.g. `PROJ-123`) in branch names / PR titles to link them.
- **Azure Boards:** Boards → Project settings → GitHub connections → add this repo. Use `AB#123` in commits/PRs.
- **Confluence / SharePoint / Azure Blob:** link design docs from `SPEC.md`; they are not synced into the repo.

## 2. Review (stage 04)
1. Install the Claude GitHub App: https://github.com/apps/claude
2. **Settings → Secrets and variables → Actions → New repository secret**: `ANTHROPIC_API_KEY`.
   Until it exists, the `Claude code review` workflow fails on PRs.

## 3. Security (stage 06) — free on public repos
**Settings → Code security**: enable *Dependency graph*, *Dependabot alerts*, *Dependabot security updates*, *Secret scanning* and *Push protection*.
CodeQL runs from `.github/workflows/codeql.yml` (use "Advanced" setup; do not also enable "Default" setup, or the two conflict).

## 4. Branch protection
**Settings → Rules → Rulesets → New branch ruleset** targeting `main`:
- Require a pull request. While you are the only collaborator, set required approvals to **0** — GitHub does not let you approve your own PR, so 1 approval + Code Owner review would block every merge. Raise it to 1 + Code Owners once a second maintainer joins.
- Require status checks: `Lint, typecheck, unit tests`, `Playwright e2e`, `analyze (javascript-typescript)`, `Dependency review`, `gitleaks` (add `Claude review` once `ANTHROPIC_API_KEY` is set)
- Block force pushes

## 5. Azure App Service deploy (stage 07)
```bash
# Fill in your values
RG=rg-studious-happiness; APP=<globally-unique-app-name>; LOC=westeurope
REPO=nsakvarel/studious-happiness

az group create -n $RG -l $LOC
az appservice plan create -g $RG -n plan-$APP --is-linux --sku B1
az webapp create -g $RG -p plan-$APP -n $APP --runtime "NODE:22-lts"
az webapp config set -g $RG -n $APP --startup-file "npm start"
az webapp update -g $RG -n $APP --https-only true

# OIDC identity for GitHub Actions (no stored secret)
APP_ID=$(az ad app create --display-name gh-$APP --query appId -o tsv)
az ad sp create --id $APP_ID
az ad app federated-credential create --id $APP_ID --parameters "{
  \"name\": \"github-production\",
  \"issuer\": \"https://token.actions.githubusercontent.com\",
  \"subject\": \"repo:$REPO:environment:production\",
  \"audiences\": [\"api://AzureADTokenExchange\"]}"
az role assignment create --assignee $APP_ID --role "Website Contributor" \
  --scope $(az webapp show -g $RG -n $APP --query id -o tsv)

echo "AZURE_CLIENT_ID=$APP_ID"
echo "AZURE_TENANT_ID=$(az account show --query tenantId -o tsv)"
echo "AZURE_SUBSCRIPTION_ID=$(az account show --query id -o tsv)"
```
Then in GitHub:
- **Settings → Environments → New environment `production`** (optionally add yourself as required reviewer).
- Add secrets `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`.
- Add repository **variable** `AZURE_WEBAPP_NAME=$APP`. The deploy workflow is skipped until this variable exists.

References: [Deploy to App Service with GitHub Actions](https://learn.microsoft.com/azure/app-service/deploy-github-actions), [Azure login with OIDC](https://learn.microsoft.com/azure/developer/github/connect-from-azure-openid-connect).

## 6. Maintain (stage 08)
Dependabot (`.github/dependabot.yml`) opens weekly update PRs on Monday (Tbilisi time). Add Azure Monitor / Application Insights on the web app for runtime monitoring.
