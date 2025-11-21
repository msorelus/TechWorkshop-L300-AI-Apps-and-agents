# GitHub Actions Workflows

## Deploy to Azure Container Registry

The `deploy-to-acr.yml` workflow builds and pushes the Docker image to Azure Container Registry.

### Required GitHub Secrets

You need to configure the following secrets in your GitHub repository:

1. **ACR_NAME** - Your Azure Container Registry name (e.g., `myregistry`)
2. **ACR_USERNAME** - ACR username (from Access Keys in Azure Portal)
3. **ACR_PASSWORD** - ACR password (from Access Keys in Azure Portal)
4. **ENV** - The complete contents of your `.env` file

### Setting Up Secrets

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add each of the secrets listed above

#### How to get ACR credentials:

```bash
# Login to Azure
az login

# Get ACR credentials
az acr credential show --name <your-acr-name>

# Or enable admin user if not already enabled
az acr update --name <your-acr-name> --admin-enabled true
az acr credential show --name <your-acr-name>
```

#### Setting the ENV secret:

Copy the entire contents of your `src/.env` file and paste it as the value for the `ENV` secret.

### Workflow Triggers

- **Automatic**: Pushes to the `main` branch that modify files in `src/`
- **Manual**: Via the "Actions" tab using "Run workflow"

### Image Tags

The workflow creates the following tags:
- `latest` - Latest build from main branch
- `main-<git-sha>` - Specific commit SHA
- `main` - Branch name tag

### Security Notes

⚠️ **Important**: 
- Never commit `.env` files to the repository
- The `.env` file is created during the Docker build from the GitHub secret
- The `.env` file is automatically cleaned up after the build
- Your `.gitignore` is configured to prevent accidental commits of `.env` files
