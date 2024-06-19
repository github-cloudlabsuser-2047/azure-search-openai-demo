# Azure OpenAI and Azure AI Search ChatGPT-like App (Python)

This repository contains a solution for creating a ChatGPT-like application using Azure OpenAI and Azure AI Search. The backend of the solution is written in Python. There are also JavaScript, .NET, and Java samples available based on this solution. To learn more about developing AI applications using Azure AI Services, visit the [Azure AI Services documentation](https://aka.ms/azai).

## Table of Contents

- [Features](#features)
- [Azure Account Requirements](#azure-account-requirements)
- [Azure Deployment](#azure-deployment)
  - [Cost Estimation](#cost-estimation)
  - [Project Setup](#project-setup)
    - [GitHub Codespaces](#github-codespaces)
    - [VS Code Dev Containers](#vs-code-dev-containers)
    - [Local Environment](#local-environment)
  - [Deploying](#deploying)
  - [Deploying Again](#deploying-again)
- [Sharing Environments](#sharing-environments)
- [Using the App](#using-the-app)
- [Running Locally](#running-locally)
- [Monitoring with Application Insights](#monitoring-with-application-insights)
- [Customizing the UI and Data](#customizing-the-ui-and-data)
- [Productionizing](#productionizing)
- [Clean Up](#clean-up)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=599293758&machine=standardLinux32gb&devcontainer_path=.devcontainer%2Fdevcontainer.json&location=WestUs2)
[![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/azure-search-openai-demo)

This repository provides a sample application that demonstrates how to create ChatGPT-like experiences using the Retrieval Augmented Generation pattern. It utilizes Azure OpenAI Service to access a GPT model (gpt-35-turbo) and Azure AI Search for data indexing and retrieval.

The sample includes pre-built data related to a fictitious company called Contoso Electronics. The application allows employees to ask questions about benefits, internal policies, job descriptions, and roles.

## Features

- Chat (multi-turn) and Q&A (single turn) interfaces
- Citations and thought process for each answer
- UI settings to customize behavior and experiment with options
- Integration with Azure AI Search for document indexing and retrieval, supporting various document formats and integrated vectorization
- Optional usage of GPT-4 with vision for image-heavy documents
- Optional speech input/output for accessibility
- Optional user login and data access via Microsoft Entra
- Performance tracing and monitoring with Application Insights

## Azure Account Requirements

To deploy and run this example, you'll need the following:

- Azure account: If you're new to Azure, you can [get an Azure account for free](https://azure.microsoft.com/free/cognitive-search/) and receive free Azure credits to get started.
- Azure subscription with access enabled for the Azure OpenAI service: You can request access using [this form](https://aka.ms/oaiapply). If your access request to Azure OpenAI service doesn't match the [acceptance criteria](https://learn.microsoft.com/legal/cognitive-services/openai/limited-access?context=%2Fazure%2Fcognitive-services%2Fopenai%2Fcontext%2Fcontext), you can use the [OpenAI public API](https://platform.openai.com/docs/api-reference/introduction) instead. Learn how to switch to an OpenAI instance in the [documentation](docs/deploy_existing.md#openaicom-openai).
- Azure account permissions: Your Azure account must have `Microsoft.Authorization/roleAssignments/write` permissions, such as [Role Based Access Control Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview), [User Access Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator), or [Owner](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner). If you don't have subscription-level permissions, you must be granted [RBAC](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview) for an existing resource group and [deploy to that existing group](docs/deploy_existing.md#resource-group). Your Azure account also needs `Microsoft.Resources/deployments/write` permissions on the subscription level.

## Azure Deployment

### Cost Estimation

The pricing for this solution varies based on region and usage. You can use the [Azure pricing calculator](https://azure.com/e/d18187516e9e421e925b3b311eec8aae) to estimate the costs for the following resources:

- Azure App Service: Basic Tier with 1 CPU core, 1.75 GB RAM. Pricing per hour. [Pricing](https://azure.microsoft.com/pricing/details/app-service/linux/)
- Azure OpenAI: Standard tier, GPT and Ada models. Pricing per 1K tokens used, with at least 1K tokens used per question. [Pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)
- Azure AI Document Intelligence: SO (Standard) tier using pre-built layout. Pricing per document page, with sample documents having a total of 261 pages. [Pricing](https://azure.microsoft.com/pricing/details/form-recognizer/)
- Azure AI Search: Standard tier, 1 replica, free level of semantic search. Pricing per hour. [Pricing](https://azure.microsoft.com/pricing/details/search/)
- Azure Blob Storage: Standard tier with ZRS (Zone-redundant storage). Pricing per storage and read operations. [Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/)
- Azure Monitor: Pay-as-you-go tier. Costs based on data ingested. [Pricing](https://azure.microsoft.com/pricing/details/monitor/)

To reduce costs, you can switch to free SKUs for various services, but note that these SKUs have limitations. Refer to the [guide on deploying with minimal costs](docs/deploy_lowcost.md) for more details.

### Project Setup

There are multiple options for setting up this project. The easiest way to get started is by using GitHub Codespaces, which sets up all the necessary tools for you. Alternatively, you can set up the project locally.

#### GitHub Codespaces

You can run this repository virtually using GitHub Codespaces, which opens a web-based VS Code environment in your browser. Click the following badge to open the environment:

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=599293758&machine=standardLinux32gb&devcontainer_path=.devcontainer%2Fdevcontainer.json&location=WestUs2)

Once the Codespace is open, you can access the terminal window.

#### VS Code Dev Containers

Another option is to use VS Code Dev Containers, which opens the project in your local VS Code using the Dev Containers extension. Follow these steps:

1. Start Docker Desktop (install it if not already installed).
2. Open the project by clicking the following badge:

[![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/azure-search-openai-demo)

Once the project files appear in the VS Code window, you can access the terminal window.

#### Local Environment

To set up the project in your local environment, follow these steps:

1. Install the required tools:
   - [Azure Developer CLI](https://aka.ms/azure-dev/install)
   - [Python 3.9, 3.10, or 3.11](https://www.python.org/downloads/)
     - Ensure that Python and the pip package manager are in the path in Windows for the setup scripts to work.
     - Make sure you can run `python --version` from the console. On Ubuntu, you might need to run `sudo apt install python-is-python3` to link `python` to `python3`.
   - [Node.js 18+](https://nodejs.org/download/)
   - [Git](https://git-scm.com/downloads)
   - [Powershell 7+ (pwsh)](https://github.com/powershell/powershell) - For Windows users only.
     - Ensure that you can run `pwsh.exe` from a PowerShell terminal. If this fails, you may need to upgrade PowerShell.
2. Create a new folder and navigate to it in the terminal.
3. Run the following command to download the project code:

   ```shell
   azd init -t azure-search-openai-demo
   ```

   Note that this command initializes a git repository, so you do not need to clone this repository.

### Deploying

To provision Azure resources and deploy the application code, follow these steps:

1. Log in to your Azure account:

   ```shell
   azd auth login
   ```

2. Create a new azd environment:

   ```shell
   azd env new
   ```

   Enter a name for the resource group. This will create a new folder in the `.azure` directory and set it as the active environment for any `azd` calls.
3. (Optional) Customize the deployment by setting environment variables to use existing resources, enable optional features, or deploy to free tiers.
4. Run `azd up` to provision Azure resources and deploy the sample to those resources. This command will also build the search index based on the files in the `./data` folder.
   - Note: The resources created by this command will incur immediate costs, primarily from the AI Search resource. These resources may accrue costs even if you interrupt the command before it is fully executed. To avoid unnecessary spending, you can run `azd down` or manually delete the resources.
   - You will be prompted to select two locations: one for the majority of resources and one for the OpenAI resource. The location list is based on the [OpenAI model availability table](https://learn.microsoft.com/azure/cognitive-services/openai/concepts/models#model-summary-table-and-region-availability) and may become outdated as availability changes.
5. After the application is successfully deployed, a URL will be printed to the console. Click the URL to interact with the application in your browser.

### Deploying Again

If you have only made changes to the backend/frontend code in the `app` folder, you can redeploy the application without re-provisioning the Azure resources. Run the following command:

```shell
azd deploy
```

If you have made changes to the infrastructure files (`infra` folder or `azure.yaml`), you will need to re-provision the Azure resources. Run the following command:

```shell
azd up
```

## Sharing Environments

To give someone else access to a fully deployed environment, follow these steps:

1. Install the [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli).
2. Run `azd init -t azure-search-openai-demo` or clone this repository.
3. Run `azd env refresh -e {environment name}`. You will need the azd environment name, subscription ID, and location to run this command. You can find those values in your `.azure/{env name}/.env` file. This command populates the `.env` file of the recipient's azd environment with the necessary settings to run the application locally.
4. Set the environment variable `AZURE_PRINCIPAL_ID` in the `.env` file or in the active shell to the Azure ID of the recipient. They can obtain their Azure ID by running `az ad signed-in-user show`.
5. Run `./scripts/roles.ps1` or `.scripts/roles.sh` to assign all the necessary roles to the recipient. If the recipient does not have the necessary permission to create roles in the subscription, you may need to run this script on their behalf. After running the script, the recipient should be able to run the application locally.

## Running Locally

You can run the application locally after successfully running the `azd up` command. If you haven't done so, follow the steps in the [Azure Deployment](#azure-deployment) section above.

1. Run `azd auth login`.
2. Navigate to the `app` directory.
3. Run `./start.ps1` or `./start.sh`, or use the "VS Code Task: Start App" command to start the project locally.

For more tips on local development, refer to the [local development guide](docs/localdev.md).

## Using the App

To use the application:

- In Azure: Navigate to the Azure WebApp deployed by azd. The URL is printed out when azd completes (as "Endpoint"), or you can find it in the Azure portal.
- Running locally: Navigate to 127.0.0.1:50505.

Once in the web app:

- Try different topics in the chat or Q&A context. For chat, try follow-up questions, clarifications, asking to simplify or elaborate on an answer, etc.
- Explore citations and sources.
- Click on "settings" to try different options and tweak prompts.

## Monitoring with Application Insights

The deployed application uses Application Insights for request tracing and error logging by default.

To view performance data, go to the Application Insights resource in your resource group, click on the "Investigate -> Performance" blade, and navigate to any HTTP request to see timing data. Use the "Drill into Samples" button to see end-to-end traces of all the API calls made for any chat request.

To view exceptions and server errors, go to the "Investigate -> Failures" blade and use the filtering tools to locate a specific exception. Python stack traces can be found on the right-hand side.

You can also see chart summaries on a dashboard by running the following command:

```shell
azd monitor
```

## Customizing the UI and Data

After successfully deploying the application, you can customize it to suit your needs. You can change the text, tweak the prompts, and replace the data. Refer to the [app customization guide](docs/customization.md) and the [data ingestion guide](docs/data_ingestion.md) for more details.

## Productionizing

This sample serves as a starting point for your own production application. Before deploying to production, it is recommended to review the security and performance aspects. Read through the [productionizing guide](docs/productionizing.md) for more details.

## Clean Up

To clean up all the resources created by this sample:

1. Run `azd down`.
2. When prompted if you are sure you want to continue, enter `y`.
3. When prompted if you want to permanently delete the resources, enter `y`.

The resource group and all associated resources will be deleted.

## Troubleshooting

Here are some common failure scenarios and their solutions:

1. The subscription (`AZURE_SUBSCRIPTION_ID`) does not have access to the Azure OpenAI service. Ensure that `AZURE_SUBSCRIPTION_ID` matches the ID specified in the [OpenAI access request process](https://aka.ms/oai/access).

2. You are attempting to create resources in regions that are not enabled for Azure OpenAI or where the model you are trying to use is not enabled. Refer to the [model availability table](https://aka.ms/oai/models) for the list of available models and regions.

3. You have exceeded a quota, such as the number of resources per region. Review the [quotas and limits](https://aka.ms/oai/quotas) documentation for more information.

4. You are encountering conflicts with "same resource name not allowed". This is likely because you have run the sample multiple times and deleted the resources without purging them. Azure retains resources for 48 hours unless you purge them from soft delete. Refer to the [purging resources](https://learn.microsoft.com/azure/cognitive-services/manage-resources?tabs=azure-portal#purge-a-deleted-resource) documentation for more information.

5. You encounter a `CERTIFICATE_VERIFY_FAILED` error when running the `prepdocs.py` script. This is typically due to incorrect SSL certificate setup on your machine. Try the suggestions provided in this [StackOverflow answer](https://stackoverflow.com/questions/35569042/ssl-certificate-verify-failed-with-python3/43855394#43855394).

6. After running `azd up` and visiting the website, you see a '404 Not Found' error in the browser. Wait for 10 minutes and try again, as the application may still be starting up. If the issue persists, try running `azd deploy` and wait again. If you continue to encounter errors with the deployed app, refer to the [guide on debugging App Service deployments](docs/appservice.md). If the logs do not help resolve the error, please file an issue.

## Resources

- [Additional documentation for this app](docs/README.md)
- [Revolutionize your Enterprise Data with ChatGPT: Next-gen Apps w/ Azure OpenAI and AI Search](https://aka.ms/entgptsearchblog)
- [Azure AI Search documentation](https://learn.microsoft.com/azure/search/search-what-is-azure-search)
- [Azure OpenAI Service documentation](https://learn.microsoft.com/azure/cognitive-services/openai/overview)
- [Comparing Azure OpenAI and OpenAI](https://learn.microsoft.com/azure/cognitive-services/openai/overview#comparing-azure-openai-and-openai/)
- [Access Control in Generative AI applications with Azure Cognitive Search](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/access-control-in-generative-ai-applications-with-azure/ba-p/3956408)
- [Video overview of OpenAI apps on Azure](https://www.youtube.com/watch?v=j8i-OM5kwiY)
- [AI Chat App Hack series](https://www.youtube.com/playlist?list=PL5lwDBUC0ag6_dGZst5m3G72ewfwXLcXV)

### Getting Help

This repository is supported by the maintainers, not by Microsoft Support. For help with deploying this sample, please post in [GitHub Issues](/issues). If you are a Microsoft employee, you can also post in [our Teams channel](https://aka.ms/azai-python-help).
1. You're getting "same resource name not allowed" conflicts. That's likely because you've run the sample multiple times and deleted the resources you've been creating each time, but are forgetting to purge them. Azure keeps resources for 48 hours unless you purge from soft delete. See [this article on purging resources](https://learn.microsoft.com/azure/cognitive-services/manage-resources?tabs=azure-portal#purge-a-deleted-resource).

1. You see `CERTIFICATE_VERIFY_FAILED` when the `prepdocs.py` script runs. That's typically due to incorrect SSL certificates setup on your machine. Try the suggestions in this [StackOverflow answer](https://stackoverflow.com/questions/35569042/ssl-certificate-verify-failed-with-python3/43855394#43855394).

1. After running `azd up` and visiting the website, you see a '404 Not Found' in the browser. Wait 10 minutes and try again, as it might be still starting up. Then try running `azd deploy` and wait again. If you still encounter errors with the deployed app, consult the [guide on debugging App Service deployments](docs/appservice.md). Please file an issue if the logs don't help you resolve the error.

## Resources

- [Additional documentation for this app](docs/README.md)
- [📖 Revolutionize your Enterprise Data with ChatGPT: Next-gen Apps w/ Azure OpenAI and AI Search](https://aka.ms/entgptsearchblog)
- [📖 Azure AI Search](https://learn.microsoft.com/azure/search/search-what-is-azure-search)
- [📖 Azure OpenAI Service](https://learn.microsoft.com/azure/cognitive-services/openai/overview)
- [📖 Comparing Azure OpenAI and OpenAI](https://learn.microsoft.com/azure/cognitive-services/openai/overview#comparing-azure-openai-and-openai/)
- [📖 Access Control in Generative AI applications with Azure Cognitive Search](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/access-control-in-generative-ai-applications-with-azure/ba-p/3956408)
- [📺 Quickly build and deploy OpenAI apps on Azure, infused with your own data](https://www.youtube.com/watch?v=j8i-OM5kwiY)
- [📺 AI Chat App Hack series](https://www.youtube.com/playlist?list=PL5lwDBUC0ag6_dGZst5m3G72ewfwXLcXV)

### Getting help

This is a sample built to demonstrate the capabilities of modern Generative AI apps and how they can be built in Azure.
For help with deploying this sample, please post in [GitHub Issues](/issues). If you're a Microsoft employee, you can also post in [our Teams channel](https://aka.ms/azai-python-help).

This repository is supported by the maintainers, _not_ by Microsoft Support,
so please use the support mechanisms described above, and we will do our best to help you out.

### Note

>Note: The PDF documents used in this demo contain information generated using a language model (Azure OpenAI Service). The information contained in these documents is only for demonstration purposes and does not reflect the opinions or beliefs of Microsoft. Microsoft makes no representations or warranties of any kind, express or implied, about the completeness, accuracy, reliability, suitability or availability with respect to the information contained in this document. All rights reserved to Microsoft.
