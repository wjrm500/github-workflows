# Shared GitHub Workflows

This repository contains reusable GitHub Actions workflows that can be shared across multiple projects.

## Available Workflows

### Docker Build and Deploy

A reusable workflow for building Docker images, pushing them to Docker Hub, and deploying them to a server via SSH.

**Location**: `.github/workflows/docker-deploy.yml`

#### Usage

In your repository's workflow file (e.g., `.github/workflows/deploy.yml`):
 
```yaml
name: Deploy

on:
  push:
    branches: [master]  # or your deployment branch

jobs:
  deploy:
    uses: wjrm500/github-workflows/.github/workflows/docker-deploy.yml@master
    with:
      docker_image_tag: 'username/image:latest'
      docker_compose_project_name: 'my-project'
      docker_compose_file_path: '/path/to/docker-compose.yml'
      deployment_branch: 'master'  # optional, defaults to 'master'
    secrets: inherit  # Pass all secrets from calling repo
```

#### Required Inputs

- `docker_image_tag`: The full Docker image tag (e.g., `wjrm500/myapp:latest`)
- `docker_compose_project_name`: The project name for docker-compose
- `docker_compose_file_path`: Absolute path to the docker-compose.yml file on your server

#### Optional Inputs

- `deployment_branch`: The branch to deploy from (default: `master`)

#### Required Secrets

The calling repository must have these secrets configured:

- `DOCKERHUB_USERNAME`: Your Docker Hub username
- `DOCKERHUB_TOKEN`: Your Docker Hub access token
- `DROPLET_HOST`: Your deployment server's hostname or IP
- `DROPLET_USER`: SSH username for the deployment server
- `DROPLET_SSH_KEY`: SSH private key for authentication

**Alternative**: Configure these as organisation-level secrets in your GitHub organisation settings to avoid duplicating them across repositories.

## Adding New Workflows

1. Create a new workflow file in `.github/workflows/`
2. Use `workflow_call` as the trigger
3. Define inputs and secrets as needed
4. Document the workflow in this README

## Contributing

When modifying workflows:
1. Test changes thoroughly in a test repository
2. Update documentation in this README
3. Consider backward compatibility for existing consumers
