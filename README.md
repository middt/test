# Simple ASP.NET Core 9 API

A minimal ASP.NET Core 9 web API with Docker support and automated release pipeline.

## Features

- **ASP.NET Core 9** - Latest .NET framework
- **Minimal APIs** - Clean and simple endpoint definitions
- **Swagger/OpenAPI** - Interactive API documentation
- **Health Checks** - Monitoring and container health
- **Docker Support** - Multi-stage Dockerfile for optimized builds
- **Automated Releases** - GitHub Actions workflow for Docker packages

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Hello world message |
| `/health` | GET | Health check status |
| `/weatherforecast` | GET | Sample weather data |
| `/api/info` | GET | Application information |
| `/swagger` | GET | Interactive API documentation |

## Local Development

### Prerequisites
- [.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)

### Run the application
```bash
dotnet run
```

The API will be available at:
- HTTP: `http://localhost:5000`
- HTTPS: `https://localhost:5001`
- Swagger UI: `http://localhost:5000/swagger`

## Docker

### Build and run locally
```bash
# Build the image
docker build -t simple-api .

# Run the container
docker run -p 8080:8080 simple-api
```

The API will be available at `http://localhost:8080`

## Automated Docker Releases

This repository includes a GitHub Actions workflow that automatically builds and publishes Docker images when you push to `release-*` branches.

### How it works

1. **Push to release branch**: Create and push to a branch named `release-vX.Y` (e.g., `release-v1.0`, `release-v2.5`)
2. **Automatic versioning**: The workflow automatically increments patch versions
   - First release: `release-v1.0` → creates tag `v1.0.1`
   - Subsequent releases: If `v1.0.2` exists → creates `v1.0.3`
3. **Docker images published**:
   - `ghcr.io/your-org/your-repo:v1.0.3` (specific version)
   - `ghcr.io/your-org/your-repo:v1.0-latest` (latest for major.minor)

### Example usage

```bash
# Create a release branch
git checkout -b release-v1.0
git push origin release-v1.0

# This triggers the workflow which will:
# 1. Build the Docker image
# 2. Create version tag v1.0.1 (or increment if exists)
# 3. Publish to GitHub Container Registry
# 4. Create a GitHub release with changelog
```

### Supported branch patterns

- `release-v1.0` → `v1.0.1`, `v1.0.2`, etc.
- `release-v2.5` → `v2.5.1`, `v2.5.2`, etc.
- `release-v1.0-feature` → `v1.0.1`, `v1.0.2`, etc.
- Any `release-vX.Y` pattern

## Pull Request Auto-Update

The repository also includes an auto-update workflow for PRs:

1. **Label a PR** with `autoupdate`
2. **Any push to main** will automatically update the PR branch with latest changes
3. **Merge conflicts** are handled gracefully with helpful comments

## Health Monitoring

The application includes comprehensive health checks:

```bash
curl http://localhost:8080/health
```

Response:
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00Z",
  "version": "1.0.0"
}
```

## Application Information

Get detailed application info:

```bash
curl http://localhost:8080/api/info
```

Response includes:
- Application name and version
- Environment details
- Machine information
- .NET version
- Uptime statistics

## Branch Protection Setup

This repository includes code review requirements for main and release branches to ensure code quality and security.

### CODEOWNERS Configuration

The `.github/CODEOWNERS` file automatically requests code reviews:

```
# All files require review from @middt before merging to main or release-* branches
* @middt
```

### Setting Up Branch Protection Rules

To protect your main and release branches:

#### Step 1: Go to Repository Settings
1. Navigate to your repository on GitHub
2. Click **Settings** → **Branches**

#### Step 2: Protect Main Branch
1. Click **Add rule** or **Add protection rule**
2. Configure:
   - **Branch name pattern**: `main`
   - ✅ **Require a pull request before merging**
   - ✅ **Require review from CODEOWNERS**
   - Click **Create**

#### Step 3: Protect Release Branches
1. Add another protection rule
2. Configure:
   - **Branch name pattern**: `release-*`
   - ✅ **Require a pull request before merging**
   - ✅ **Require review from CODEOWNERS**
   - Click **Create**

### What This Provides

- **No direct pushes** to main or release branches
- **All changes** must go through Pull Requests
- **Automatic code review** requests for all PRs
- **Maintains code quality** and prevents unauthorized changes
- **Docker workflows** still work automatically after PR approval
