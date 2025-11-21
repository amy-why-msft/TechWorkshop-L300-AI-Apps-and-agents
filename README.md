# Project

This lab teaches you how to design and build AI applications and agents using Azure AI Foundry. You will learn how to create AI-powered applications that can interact with users, process natural language, and perform tasks based on user guidance. You will also learn how to monitor, troubleshoot, and perform red teaming activities against agents.

## Deployment

### Azure Container Registry Deployment

The application in the `src/` folder can be automatically deployed to Azure Container Registry (ACR) using GitHub Actions.

#### Prerequisites

Configure the following GitHub secrets in your repository:
- `AZURE_REGISTRY_LOGIN_SERVER` - Your ACR login server (e.g., `myregistry.azurecr.io`)
- `AZURE_REGISTRY_USERNAME` - ACR username for authentication
- `AZURE_REGISTRY_PASSWORD` - ACR password for authentication
- `ENV` - Complete contents of your `.env` file (see `src/env_sample.txt` for required variables)

#### Workflow

The deployment workflow (`.github/workflows/deploy-acr.yml`) triggers automatically on pushes to the `main` branch and:
1. Builds a Docker image from the `src/` directory
2. Securely injects environment variables from the `ENV` secret using Docker BuildKit secrets
3. Tags the image with both the git SHA and `latest`
4. Pushes the image to Azure Container Registry

#### Security Features

- **No secrets in image layers**: Environment variables are injected using BuildKit secrets, ensuring they are not stored in any Docker image layer
- **No secrets in logs**: The `ENV` secret content is never exposed in GitHub Actions logs
- **Protected `.env` files**: The `.gitignore` file prevents `.env` files from being committed to the repository

#### Local Development

For local development, create a `.env` file in the `src/` directory based on `src/env_sample.txt`. This file will be automatically excluded from git commits.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
