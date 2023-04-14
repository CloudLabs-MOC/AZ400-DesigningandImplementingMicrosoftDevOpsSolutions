# Lab 10: Creating a Release Dashboard
# Student lab manual

## Lab overview

In this lab, you will step through creation of a release dashboard and the use of REST API to retrieve Azure DevOps release data, which you can make this way available to your custom applications or dashboards.

The lab leverages the Azure DevOps Starter resource, which automatically creates an Azure DevOps project that builds and deploys an application into Azure. 

## Objectives

After you complete this lab, you will be able to:

- create a release dashboard
- use REST API to query release information

## Instructions

### Exercise 1: Query release information via REST API

In this exercise, you will query release information via REST API by using Postman.

#### Task 1: Generate an Azure DevOps personal access token

In this task, you will generate an Azure DevOps personal access token that will be used to authenticate from the Postman app you will install in the next task of this exercise.

1.  On the lab computer, in the web browser window displaying the Azure DevOps portal, in the upper right corner of the Azure DevOps page, click the **User settings** icon, in the dropdown menu, click **Personal access tokens**, on the **Personal Access Tokens** pane, and click **+ New Token**.
1.  On the **Create a new personal access token** pane, click the **Show all scopes** link and, specify the following settings and click **Create** (leave all others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **Creating a Release Dashboard lab** |
    | Scope | **Release** |
    | Permissions | **Read** |
    | Scope | **Build** |
    | Permissions | **Read** |

1.  On the **Success** pane, copy the value of the personal access token to Clipboard.

    > **Note**: Make sure you record the value of the token. You will not be able to retrieve it once you close this pane. 

1.  On the **Success** pane, click **Close**.

#### Task 2: Query release information via REST API by using Postman

In this task, you will query release information via REST API by using Postman.

1.  On the lab computer, start a web browser and navigate to https://www.postman.com/downloads/, click **Download the App** button, in the dropdown menu, click **Windows 64-bit**, click the downloaded file and run the installation. Once the installation completes, the Postman desktop app will start automatically. 
1.  In the **Create Postman Account** pane, provide your email address, a username and password and click **Create free account**. Complete the startup process.

    > **Note**: You will receive an email from Postman to activate your Postman account to complete the process of account provisioning.

1.  Once you signed in, within the Postman desktop app window, Select **Workspaces** and click on **My Workspace**(if there is no workspace precreated you can create one.), in the upper left corner, click **+New**, on the **Create New** pane, click **Collection**, in the **Name your collection** text box, type **Azure DevOps az400m10l02 queries**, right click the collection which you created and click on **Add request**. In the **New Request** text box enter **Get-Releases** and click on **Save**.
1.  Open another web browser window and navigate to https://docs.microsoft.com/en-us/rest/api/azure/devops/release/releases/list?view=azure-devops-rest-6.0 and review its content. 
1.  Switch back to the Postman desktop app, in the Launchpad pane in the upper right section of the app window, click the **Authorization** tab header, in the **TYPE** dropdown list, select the **Basic Auth** entry and, in the **Password** textbox, paste the value of the token you copied in the previous task (do not set the value of the **Username** text box).
1.  Switch to the web browser window displaying the content of Microsoft Docs and navigate to https://docs.microsoft.com/en-us/rest/api/azure/devops/release/deployments/list?view=azure-devops-rest-6.0 and review its content. 
1.  In the Launchpad pane in the upper right section of the app window, ensure that **GET** appears in the dropdown list, in the **Enter request URL** textbox, type the following and click **Send** (replace the value of the `<organization_name>` with the name of your Azure DevOps organization) in order to list all deployments:

    ```url
    https://vsrm.dev.azure.com/<organization_name>/Creating%20a%20Release%20Dashboard/_apis/release/deployments?api-version=6.0
    ```

1.  Review the output listed on the **Body** tab in the lower right section of the app window and verify that it includes the listing of the deployments you initiated in the previous exercise of this lab.
1.  In the Launchpad pane in the upper right section of the app window, ensure that **GET** appears in the dropdown list, in the **Enter request URL** textbox, type the following and click **Send** (replace the value of the `<organization_name>` with the name of your Azure DevOps organization) in order to list all deployments:

    ```url
    https://vsrm.dev.azure.com/<organization_name>/Creating%20a%20Release%20Dashboard/_apis/release/deployments?DeploymentStatus=failed&api-version=6.0
    ```

1.  Review the output listed on the **Body** tab in the lower right section of the app window and verify that it includes only the failed deployment you initiated in the previous exercise of this lab.
    
### Exercise 2: Remove the Azure DevOps billing

In this exercise, you will remove the Azure DevOps billing enabled in this lab to eliminate unexpected charges.

#### Task 1: Remove the Azure DevOps billing

In this task, you will remove pipeline billing to eliminate unnecessary charges.

1. On the lab computer, switch to the browser window displaying Azure DevOps organization homepage and select **Organization Settings** at bottom left corner.

1. Under **Organization Settings** select **Billing** and click on **Change billing** button to open Change billing pane.

1. In the **Change billing** pane, select **Remove billing** setting and click on Save.

#### Review

In this lab, you learned how to create and configure release dashboard and how to use REST API to retrieve Azure DevOps release data.
