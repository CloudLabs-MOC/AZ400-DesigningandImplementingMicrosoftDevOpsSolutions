# Lab 01: Implementing GitHub Actions for CI/CD

## Estimated duration: 40 Minutes

## Lab Scenario

You are a DevOps engineer for Contoso.ltd tasked with deploying a dynamic .NET web app to Azure using GitHub Actions and Infrastructure as Code. In this lab, you will import the eShopOnWeb app into your GitHub repository, configure Azure access using a service principal, and automate deployment via a GitHub Actions workflow backed by Bicep templates.

## Objectives

In this lab you will complete the following exercises:

- Import the eShopOnWeb app into your GitHub repository
- Configure GitHub repository settings and Azure service principal
- Update and run a GitHub Actions workflow to deploy the web app to Azure

## Lab requirements

- If you don't already have a GitHub account that you can use for this lab, follow the instructions available at [Signing up for a new GitHub account](https://docs.github.com/get-started/signing-up-for-github/signing-up-for-a-new-github-account).


## Prepare a GitHub account

1. If you already have a GitHub account that you can use for this lab, proceed with Exercise 1, follow the instructions to create an account.

1. Navigate to the https://github.com/ **(1)** and then Click on **Sign up (2)** in the top right corner.

   ![Github](images/dev263.png)
   
1. Provide your **Email address (1)**, **Password (2)**, **Username (3)** and click on **Continue (4)**.

   ![Github](images/dev264.png)

1. If prompted, complete the visual puzzle.

1. Confirm your details, verify your account, and click **'Create Account'**. The process will take approximately 2 minutes to complete.

# Exercise 1: Import eShopOnWeb to your GitHub Repository

In this exercise, you will import the existing [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb) repository code to your own GitHub private repo.

The repository is organized in the following way:
   - **.ado** folder contains Azure DevOps YAML pipelines
   - **.devcontainer** folder container setup to develop using containers (either locally in VS Code or GitHub Codespaces)
   - **.azure** folder contains Bicep & ARM infrastructure as code templates used in some lab scenarios.
   - **.github** folder contains YAML GitHub workflow definitions.
   - **src** folder contains the .NET 6 website used in the lab scenarios.

## Task 1: Create a public repository in GitHub and import eShopOnWeb

In this task, you will create an empty public GitHub repository and import the existing [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb) repository.

1. From the Lab VM, start a web browser, right-click on [GitHub website](https://github.com/), and paste it into the browser tab.

1. Click on **Sign in**. 
    ![Create Repository](images/dev231.png)

1. Provide your **Github Username/email address (1)** and **Password (2)** then click on **Sign in (3)**.

    ![Create Repository](images/l1-05-01.png)

1. Then you will receive a device verification code to your email, enter that **code (1)** and then click on **Verify (2)**. 

    ![Create Repository](images/dev262.png)

1. Click on **New** to create a new repository.

    ![Create Repository](images/l1-05-02.png)
 
1. On the **Create a new repository** page, click on the **Import a repository** link (below the page title).

    ![](images/l1-05-03.png)

     >**NOTE:** You can also open the import website directly at <https://github.com/new/import>

1. On the **Import your project to GitHub** page, enter the following details and then click on **Begin Import (5)** and wait for your repository to be ready (this may take a few minutes).
    
    | Field | Value |
    | --- | --- |
    | The URL for your source repository| https://github.com/CloudLabs-MOC/eShopOnWeb **(1)** |
    | Owner | Leave your default username **(2)** |
    | Repository Name | **eShopOnWeb (3)** |
    | Privacy | **Public** **(4)** | 

    ![](images/dev265.png)

1. Wait for the import to complete.

    ![](images/dev3.png)

1. On the repository page, go to **Settings (1)**, click on **Actions (2)> General (3)** and choose the option **Allow all actions and reusable workflows (4)**. Click on **Save (5)**.

    ![Enable GitHub Actions](images/l1-05-04.png)

# Exercise 2: Set up your GitHub Repository and Azure access

In this exercise, you will create an Azure Service Principal to authorize GitHub to access your Azure subscription from GitHub Actions. You will also set up the GitHub workflow that will build, test, and deploy your website to Azure. 

## Task 1: Create an Azure Service Principal and save it as a GitHub secret

In this task, you will create the Azure Service Principal used by GitHub to deploy the desired resources. As an alternative, you could also use [OpenID connect in Azure](https://docs.github.com/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-azure) as a secretless authentication mechanism.

1. On your lab VM, in an edge browser window, open the  [Azure Portal](https://portal.azure.com).

1. In the portal, search for **Resource Groups (1)** and select **Resource Groups (2)**.

    ![](images/dev5.png)

1. Click on **+ Create** to create a new Resource Group for the exercise.

    ![](images/l1-05-05.png)

1. On the **Create a resource group** tab, 
     
    - Subscription: Leave it at the default setting  **(1)**
    - Resource Group name: **rg-devOps-eshopeonweb-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Region: Leave the default one **(3)**
    - Click on **Review + Create (4)**

      ![](images/dev232.png)

1. Then click on **Create**.

    ![](images/dev233.png)

1. In the Azure Portal, open the **Cloud Shell** (next to the search bar).

    ![](images/l1-05-06.png)

1. Select **Bash** mode.

    ![](images/l1-05-07.png)

1. On the **Getting started**, 

    - Select **No storage account required (1)**
    - Leave the default subscription **(2)**
    - Select **Apply**

      ![](images/dev11.png)

1. Once the Terminal starts, execute the following command **(1)**, replacing **SUBSCRIPTION-ID** and **RESOURCE-GROUP** with your identifiers (both can be found on the **Overview** page of the Resource Group):

   ```
   az ad sp create-for-rbac --name GH-Action-eshoponweb --role contributor --scopes /subscriptions/SUBSCRIPTION-ID/resourceGroups/RESOURCE-GROUP --sdk-auth
   ```

    >**NOTE:** To get the **SUBSCRIPTION-ID** and **RESOURCE-GROUP**, In the Azure portal navigate to **rg-devOps-eshopeonweb-<inject key="DeploymentID" enableCopy="false"/>** Resource group then copy the **Resource group name (1)** and **SUBSCRIPTION-ID (2)**.

     ![](images/l1-05-08.png)
    
    >**NOTE:** Make sure this is typed or pasted as a single line!
    
    >**NOTE:** This command will create a Service Principal with Contributor access to the Resource Group created before. This way, we make sure GitHub Actions will only have the permissions needed to interact with this Resource Group (not the rest of the subscription).

1. The command will output a JSON object, you will later keep it as a GitHub secret for the workflow, **Copy the JSON (2)**. The JSON contains the identifiers used to authenticate against Azure in the name of an Azure Entra application identity (service principal).

    ```JSON
    {
        "clientId": "<GUID>",
        "clientSecret": "<GUID>",
        "subscriptionId": "<GUID>",
        "tenantId": "<GUID>",
        (...)
    }
    ```

    ![Import ADO org to Sonarcloud](images/l1-05-09.png)

1. You also need to run the following command to register the resource provider for the **Azure App Service** you will deploy later:

   ```bash
   az provider register --namespace Microsoft.Web
   ```

    ![Import ADO org to Sonarcloud](images/l1-05-10.png)   

1. Navigate back to your **eShopOnWeb** GitHub repository opened in the browser.

1. On the repository page, go to **Settings (1)**, click on **Secrets and variables (2)> Actions (3)**. Click on **New repository secret (4)** under Repository secrets.

      ![](images/l1-05-11.png)

1. On the **Action Secret/New secret** tab, add the following:
    
    - Name : **AZURE_CREDENTIALS (1)**
    - Secret: **paste the previously copied  JSON object (2)** (GitHub can keep multiple secrets under the same name, used by  [azure/login](https://github.com/Azure/login) action )
    - Click on **Add secret  (3)**. Now GitHub Actions will be able to reference the service principal, using the repository secret.

      ![Import ADO org to Sonarcloud](images/l1-05-12.png)

## Task 2: Modify and execute the GitHub workflow

In this task, you will modify the given GitHub workflow and execute it to deploy the solution in your subscription.

1. Navigate to your **eShopOnWeb** GitHub repository.

1. On the repository page, go to **Code**.

    ![](images/l1-05-13.png)

1. Open the following file: **eShopOnWeb/.github (1)/workflows (2)/eshoponweb-cicd.yml (3)**. This workflow defines the CI/CD process for the given .NET 6 website code.

    ![](images/l1-05-14.png)

1. Select the **Edit** (pencil icon). 

    ![](images/l1-05-15.png)

1. Uncomment the **on** section (delete "#" symbol). The workflow triggers with every push to the main branch and also offers manual triggering ("workflow_dispatch").

    ![](images/l1-05-16.png)

1. In the **env** section, make the following changes:
    - **RESOURCE-GROUP:** Replace `RESOURCE-GROUP` variable with **rg-devOps-eshopeonweb-<inject key="DeploymentID" enableCopy="false"/>** 
    - **Location:** **westus**
    - **SUBSCRIPTION-ID:** Replace **YOUR-SUBS-ID** in **SUBSCRIPTION-ID**. You can find your subscription ID from the Overview page of **rg-devOps-eshopeonweb-<inject key="DeploymentID" enableCopy="false"/>** Resource group in Azure portal. 
    - **WEBAPP-NAME:** Enter **eshoponweb-webapp-<inject key="DeploymentID" enableCopy="false"/>**. It will be used to create a globally unique website using Azure App Service.

      ![](images/l1-05-17.png)

1. Scroll down, within the **publish (1)** section. Replace the app name value **${{ env.WEBAPP-NAME }}** with **eshoponweb-webapp-<inject key="DeploymentID" enableCopy="false"/> (2)** and then click on **Commit changes (3)**.

    ![Succesfull workflow](images/l1-05-18.png)

     >**NOTE:** Read the workflow carefully, comments are provided to help understand.

1. **Commit changes** again, leaving defaults (changing the main branch). The workflow will get automatically executed.

    ![](images/l1-05-19.png)

## Task 3: Review GitHub Workflow execution
 
In this task, you will review the GitHub workflow execution.

1. On the **eShopOnWeb** repository page, go to **Actions**.

    ![GitHub workflow in progress](images/l1-05-20.png)

1. You will see the workflow setup on top before executing. Click on **Update eshoponweb-cicd.yml**, which is associated with **eShopOnWeb Build and Test**.

    ![GitHub workflow in progress](images/l1-05-21.png)

    >**NOTE:** If it shows you the **Workflows aren’t being run on this repository**, select **Enable Actions on this repository**.

   > And then on the select workflow that is **eShopOnWeb Build and Test (1)** page, select **Run workflow (2)** drop-down, and select **Run workflow (3)**.

   ![GitHub workflow in progress](images/runworkflow.png)

1. From the **Summary**, you can see the two workflow jobs, the status, and Artifacts retained from the execution. You can click on each job to review logs.

    ![GitHub workflow in progress](images/l1-05-22.png)

     >**NOTE:** The workflow might take around 10 minutes to complete. Please wait until it is _Succeeded_, as illustrated above.

1. Navigate back to the [Azure Portal](https://portal.azure.com/).

1. Open the resource group **rg-devOps-eshopeonweb-<inject key="DeploymentID" enableCopy="false"/>** that was created earlier in Exercise 2, Task 1. **Refresh** the resource group. You will notice that the GitHub Action, using a Bicep template, has deployed an **Azure App Service Plan**  and an **App Service**.

    ![](images/l1-05-23.png)

1. Select **eshoponweb-webapp-<inject key="DeploymentID" enableCopy="false"/>** App service.

    ![](images/l1-05-24.png)

1. You can view the published website by opening the App Service and clicking on **Browse**.

    ![Browse WebApp](images/dev240.png)
    ![Browse WebApp](images/dev29.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
 
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="bbf0a520-41e6-41d4-97ab-23cda9045900" />   
    
## Summary

In this lab, you implemented a GitHub Action workflow that deploys a dynamic Azure web app by using Azure DevOps.

In this lab, you have accomplished the following:

- Exercise 1: Imported eShopOnWeb to your GitHub Repository
- Exercise 2: Set up your GitHub Repository and Azure access

### You have successfully completed the lab. Click on **Next >>** to proceed with the next lab.

![Launch Azure Portal](images/dev266.png)
