# Lab 02: Configuring Agent Pools and Understanding Pipeline Styles 

## Estimated duration: 60 Minutes

## Lab Scenario

You are a DevOps engineer at Contoso.ltd, responsible for modernizing the CI/CD workflows for an internal .NET application. To streamline deployment and gain better control over the build infrastructure, you are tasked with configuring a self-hosted Azure DevOps agent and implementing code-based pipelines using YAML. In this lab, you will define and run YAML pipelines, integrate Selenium tests, and automate the build and release processes using a self-hosted agent to support a scalable and maintainable DevOps pipeline.

## Objectives

In this lab you will complete the following exercises:

- Exercise 1: Configure the lab prerequisites
- Exercise 2: Author YAML-based Azure DevOps pipelines
- Exercise 3: Manage Azure DevOps agent pools
- Exercise 4: Implement Selenium tests by using a self-hosted Azure DevOps agent

## Lab requirement:

### Set up an Azure DevOps organization

If you don't already have an Azure DevOps organization, you signed in with your credentials, configured the organization settings, and enabled billing for CI/CD parallel jobs to prepare for pipeline execution.

1. On your lab VM open **Edge Browser** on desktop and navigate to https://go.microsoft.com/fwlink/?LinkId=307137. 

    * Email/Username: <inject key="AzureAdUserEmail"></inject>

    * Password: <inject key="AzureAdUserPassword"></inject>

1. On the **Get started with Azure DevOps**, name the project as leave the default name **(1)**, then click on **Continue (2)**.

    ![Azure DevOps](images/L2.png)

    >**Next:** If prompted for captcha on the next page, accept defaults, fill the captcha **(1)** and click on **Continue (2)**.

    ![Azure DevOps](images/dev33.png)
    
1. On the Azure DevOps page, click on **Azure DevOps (1)** located at the top left corner and then click on **Organization Settings (2)** at the bottom left corner.

    ![Azure DevOps](images/L4.png)
    
1. In the **Organization Settings** window on the left menu click on **Billing (1)** and verify the **Azure Subscription (2)** details which are visible, and then proceed to the next step.

    ![Azure DevOps](images/L5.png)

    >**Note:** If billing is not already configured, then click on **Save (3)**.

    ![Azure DevOps](images/dev35.png)

1. On the **MS Hosted CI/CD** section under **Paid parallel jobs** enter value **1** **(1)** and at the end of the page click on **Save (2)**.

    ![Azure DevOps](images/L6.png)
    
    ![Azure DevOps](images/L6i.png)
   
# Exercise 1: Configure the lab prerequisites 

In this exercise, you set up the prerequisites for the lab by creating a new Azure DevOps project named eShopOnWeb, setting its visibility to private, and leaving other settings as default for use in upcoming labs.

## Task 1: Create and configure the team project

In this task, you will create an **eShopOnWeb** Azure DevOps project to be used by several labs.

1. Click on **Azure DevOps** from the Billing page to create a new project.

    ![Azure DevOps](images/L2E1T1S1.png)
 
1. Click on **New Project**. Give your project the name  **eShopOnWeb (1)**, verify **Private (2)** under **Visibility**, and then choose **+ Create project (3)**.

      ![](images/l2-05-2.png)

## Task 2: Import eShopOnWeb Git Repository 

In this task, you will import the eShopOnWeb Git repository that will be used by several labs.

1. On the **eShopOnWeb** project. Click on **Repos (1)>Files (2) , Import a Repository**. Select **Import (3)**. On the **Import a Git Repository** window, paste the following URL https://github.com/CloudLabs-MOC/eShopOnWeb.git **(4)** and click **Import (5)**.

      ![](images/L2E1T2S1.png)
      
1. The repository is organized in the following way:
    
    - **.ado** folder contains Azure DevOps YAML pipelines
         
    - **.devcontainer** folder container setup to develop using containers (either locally in VS Code or GitHub Codespaces)
           
    - **.github** folder contains YAML GitHub workflow definitions.
         
    - **src** folder contains the .NET 6 website used in the lab scenarios.
         
      ![](images/L2E1T2S2.png)

1. Go to **Repos (1)>Branches (2)**, make sure the **main** branch is set as **default branch (3)**.

      ![](images/L2E1T2S3.png)

1. If not, hover on the **main** branch, then click the **ellipsis (1)** on the right of the column. Click on **Set as default branch (2)**.

      ![](images/L2E1T2S4.png)


# Exercise 2: Author YAML-based Azure DevOps pipelines

In this exercise, you will create an application lifecycle build pipeline using a YAML-based template.

## Task 1: Create an Azure DevOps YAML pipeline

In this task, you will create a template-based Azure DevOps YAML pipeline.

1. Go to **Pipelines (1)>Pipelines (2)**. Click on **Create Pipeline (3)** or the **New Pipeline** button.

    ![](images/L2E2T1S1.png)  

2. Select **Azure Repos Git (YAML)**

    ![](images/L2E2T1S2.png)

3. Select the **eShopOnWeb** repository.

    ![](images/L2E2T1S3.png)

4. Select **Existing Azure Pipelines YAML File**

    ![](images/L2E2T1S4.png)

5. Select the path **/.ado/eshoponweb-ci-pr.yml (1)** file then click on **Continue (2)**.

    ![](images/dev46.png)
 
6. On the **Review your pipeline YAML** pane, review the sample pipeline. This is a rather straightforward .NET application Build pipeline, which does the following:

   - A single Stage: Build
   - A single Job: Build
   - 4 tasks within the Build Job:
   - **DotNet Restore:** With NuGet Package Restore, you can install all your project's dependencies without having to store them in source control.      
   - **DotNet Build:** Builds a project and all of its dependencies.     
   - **DotNet Test:** .NET test driver used to execute unit tests.    
   - **DotNet Publish:** Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's **Build.ArtifactStagingDirectory**.
        
     ![](images/L2E2T1S6.png)

7. On the **Review your pipeline YAML** pane, click the down-facing caret symbol next to the **Run (1)** button, click **Save (2)**.

    ![](images/L2E2T1S7.png)

    >**Note:** We are just creating the pipeline definition for now, without running it. You will first set up an Azure DevOps agent pool and run the pipeline in a later exercise. 

# Exercise 3: Manage Azure DevOps agent pools

In this exercise, you will implement a self-hosted Azure DevOps agent.

## Task 1: Configure an Azure DevOps self-hosting agent

In this task, you will configure your lab Virtual Machine as an Azure DevOps self-hosting agent and use it to run a build pipeline.

1. In the **Azure DevOps** portal, in the upper right corner of the Azure DevOps page, click the **User settings (1)** icon, in the dropdown menu, click **Personal access tokens (2)**.

    ![Azure DevOps](images/dev49.png)
  
1. On the **Personal Access Tokens (1)** pane, and click **+ New Token (2)**.

    ![Azure DevOps](images/L2E3T1S2.png)

1. On the **Create a new personal access token** pane, specify the following settings and click **Create (3)** (leave all others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **eShopOnWeb (1)** |
    | Scope | **Full access (2)** |
   
     ![Azure DevOps](images/l2-05-10.png)

1. On the **Success** pane, copy the value of the personal access token to the Clipboard.

    > **Note:** Make sure you **copy** and paste the token in a notepad. You will not be able to retrieve it once you close this pane. 

    ![Azure DevOps](images/dev53.png)

1. On the **Success** pane, click **Close**.

1. On the **Personal Access Token** pane of the Azure DevOps portal, click the **Azure DevOps** symbol in the upper left corner.

    ![Azure DevOps](images/L2E3T1S6.png)

1. Then click the **Organization settings** label in the lower left corner.

    ![Azure DevOps](images/L2E3T1S7.png)

1. To the left side of the **Overview** pane, in the vertical menu, in the **Pipelines (1)** section, click **Agent pools (2)**. On the **Agent pools** pane, in the upper right corner, click **Add pool (3)**. 

    ![Azure DevOps](images/L2E3T1S8.png)

1. On the **Add agent pool** pane, in the **Pool type** dropdown list,
    
    - Select **Self-hosted (1)**
    - In the **Name** text box, type **devops-pool (2)**
    - Under **Pipeline permissions** select the checkboxes **(3)**
    - Then click **Create (4)**

      ![Azure DevOps](images/dev60.png)    
    
1. Back on the **Agent pools** pane, click the entry representing the newly created **devops-pool**. 

    ![Azure DevOps](images/L2E3T1S10.png)

1. On the **Jobs** tab of the **devops-pool** pane,  click the **New agent** button.

    ![Azure DevOps](images/L2E3T1S11.png)

1. On the **Get the agent** pane, ensure that the **Windows (1)** and **x64 (2)** tabs are selected, and click **Download (3)** to download the zip archive containing the agent binaries to download it into the local **Downloads** folder within your user profile.

    ![Azure DevOps](images/dev62.png)

     > **Note:** If you receive an error message at this point indicating that the current system settings prevent you from downloading the file, in the Browser window, in the upper right corner, click the gearwheel symbol designating the **Settings** menu header, in the dropdown menu, select **Internet Options**, in the **Internet Options** dialog box, click **Advanced**, on the **Advanced** tab, click **Reset**, in the **Reset Browser Settings** dialog box, click **Reset** again, click **Close**, and try the download again.

1. On the LabVM, right click on **Start (1)**, then select **Windows Powershell (Admin) (2)**.

    ![Azure DevOps](images/dev63.png)

1. Run the following lines to create the **C:\\agent** directory and extract the content of the downloaded archive into it.

    ```powershell
    cd \
    mkdir agent ; cd agent
    $TARGET = Get-ChildItem "$Home\Downloads\vsts-agent-win-x64-*.zip"
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory($TARGET, "$PWD")
    ```

     >**Note:** If you encounter an error indicating that the item already exists, please ignore it and proceed with the next steps.

      ![Azure DevOps](images/dev64.png)      

1. In the same **Administrator: Windows PowerShell** console, run the following to configure the agent:

    ```powershell
    .\config.cmd
    ```

     ![Azure DevOps](images/l2-05-15.png)     

1. When prompted, specify the values of the following settings:

    | Setting | Value |
    | ------- | ----- |
    | Enter server URL | Enter https://dev.azure.com/odluser<inject key="DeploymentID" enableCopy="false"/>/ |
    | Enter authentication type (press enter for PAT) | **Hit Enter** |
    | Enter personal access token | The access token you recorded earlier in this task |
    | Enter agent pool (press enter for default) | enter **devops-pool** |
    | Enter agent name (press enter for VM-<inject key="DeploymentID" enableCopy="false"/>) | **Hit Enter** |
    | Enter work folder (press enter for _work) | **Hit Enter** |
    | **(Only if shown)** Enter Perform an unzip for tasks for each step. (Press Enter for N) | **WARNING:** only press **Enter** if the message is shown|
    | Enter run agent as a service? (Y/N) (press enter for N) | **Y** |
    | enter enable SERVICE_SID_TYPE_UNRESTRICTED (Y/N) (press enter for N) | **Y** |
    | Enter User account to use for the service (press enter for NT AUTHORITY\NETWORK SERVICE) | **Hit Enter** |
    | Enter whether to prevent service starting immediately after configuration is finished? (Y/N) (press enter for N) | **Hit Enter** |

    ![Azure DevOps](images/dev273.png)    

     > **Note:** You can run a self-hosted agent as either a service or an interactive process. You might want to start with the interactive mode, since this simplifies verifying agent functionality. For production use, you should consider either running the agent as a service or as an interactive process with auto-logon enabled, since both persist their running state and ensure that the agent starts automatically if the operating system is restarted.

     > **Note:** Verify that the agent is reporting the **Listening for Jobs** status.

1. Switch to the browser window displaying the Azure DevOps portal and close the **Get the agent** pane.

1. Back on the **Agents (1)** tab of the **devops-pool** pane, note that the newly configured agent is listed with the **Online (2)** status. Click on the **Azure DevOps (3)** label from the top left corner.

    ![Azure DevOps](images/L2E3T1S18.png)

1. In the browser window displaying the list of projects, click the tile representing your **eShopOnWeb** project.

    ![Azure DevOps](images/L2E3T1S19.png)
 
1. On the **eShopOnWeb** pane, in the vertical navigational pane on the left side, in the **Pipelines (1)** section, click **Pipelines (2)**. On the **Recent** tab of the **Pipelines** pane, select **eShopOnWeb (3)**.

    ![Azure DevOps](images/L2E3T1S20.png)

1. On the **eShopOnWeb** pane, select **Edit**.

    ![Azure DevOps](images/L2E3T1S21.png)

1. On the **eShopOnWeb** edit pane, in the existing YAML-based pipeline, replace line **13** which says `vmImage: ubuntu-latest` designating the target agent pool with the following content, designating the newly created self-hosted agent pool:

    ```yaml
    name: devops-pool
    demands:
    - agent.name -equals Agentname
    ```

    > **Note:** Replace `Agentname` with **VM-<inject key="DeploymentID" enableCopy="false"/>**

    ![Azure DevOps](images/dev72.png)    
 
    ![Azure DevOps](images/L2E3T1S22i.png)
    
    > **WARNING:** Be careful with copy/paste, make sure you have the same indentation shown above. 
 
1. On the **eShopOnWeb** edit pane, in the upper right corner of the pane, click **Validate + Save**.

    ![Azure DevOps](images/L2E3T1S23.png)

1. On the **Validate and save** pane, click **Save** again. This will automatically trigger the build based on this pipeline. 

    ![Azure DevOps](images/dev75.png)

1. Click on **Run**.    

    ![Azure DevOps](images/L2E3T1S25.png)

1. Click on **Run** again to run the pipeline.

    ![Azure DevOps](images/l2-05-20.png)

1. Click on **View** to provide the permission.

    ![Azure DevOps](images/L2E3T1S27.png)

1. Click on **Permit**.

    ![Azure DevOps](images/l2-05-22.png)

1. Click on **Permit** on **Permit access**.

    ![Azure DevOps](images/dev81.png)

1. Click on **Build**.  

    ![Azure DevOps](images/L2E3T1S30.png)

1. Wait until the build  succeeds.

    ![Azure DevOps](images/L2E3T1S31.png)

     >**Note:** It might take around 5 minutes to build. Please ensure this pipeline is executed then proceed with the next steps.

1. Your pipeline will take a name based on the project name. Let's **rename** it to identify the pipeline better.

1. Go to **Pipelines>Pipelines (1)** and click on the recently created pipeline. Click on the **ellipsis (2)** and **Rename/move (3)** option.
   
    ![Azure DevOps](images/L2E3T1S33.png)

1. Name it **eshoponweb-ci-pr (1)** and click on **Save (2)**.

    ![Azure DevOps](images/dev84.png) 

# Exercise 4: Implement Selenium tests by using a self-hosted Azure DevOps agent  

In this exercise, you will implement automated Selenium tests using a self-hosted Azure DevOps agent. You will deploy a Selenium project to Azure DevOps, configure the agent on a virtual machine, and trigger a build and release pipeline to execute the tests automatically.


## Task 1: Deploy the Selenium project to Azure DevOps

In this task, you will open and configure the ADOGenerator Selenium project in Visual Studio, build the solution, and use the demo generator to create a new Azure DevOps project using the Selenium template.

1. Before deploying the project, we need to enable the **creation of classic build pipeline and release**. 

1. Click on **Azure DevOps** from the top left to navigate to the Azure DevOps page, from the bottom left, click on **Organization settings**, go to the **Pipelines (1)** section, and click **Settings (2)**. **Turn off** the **Disable creation of classic build pipelines** and **Disable creation of classic release pipelines (3)**.

    ![Azure DevOps](images/L2E4T1S2.png)

1. On the Lab VM, search for **Visual Studio (1)** and then select **Visual studio 2022 (2)**.

    ![Azure DevOps](images/dev277.png)

    > **Note:** If a Sign in to Visual Studio window is open, click on **Skip and add accounts later** and **Start Visual Studio**.

1. On the Get started page, select **Open a project or solution**.

    ![Azure DevOps](images/dev278.png)

1. Navigate to `C:\Users\azureuser\Documents\AzDevOpsDemoGenerator\src` **(1)**, select **ADOGenerator.sln (2)** solution file and then click on **Open (3)**.

    ![Azure DevOps](images/dev279.png)

1. Enable the checkbox for **.NET desktop development (1)** and then click on **Install (2)** to install the workload.

    ![Azure DevOps](images/l2-05-27.png)

     >**Note:** Wait until all the package installation completes, it might take around 5-10 minutes.

1. Once the Installation completes, **close** the installation page.    

    ![Azure DevOps](images/l2-05-28.png)

1. From the search bar, search for **Visual studio 2022 (1)** and then open the **ADOGenerator.sln (2)** Project.    

    ![Azure DevOps](images/ddev3.png)

1. If the projects are not loaded, In the **Solution explorer**, right click on the **ADOGenerator project (1)** and select **Reload project with dependencies (2)**. Do the same for **API** as well to make the project compatible.

    ![Azure DevOps](images/dev300.png)

    >**Note:** If the **GitHub Copilot Chat** pane is open, select the **Close (X)** button to close it and view the projects.

    ![Azure DevOps](images/lab2-05-01.png)

1. Right-click on the **ADOGenerator project** in the Solution Explorer **(1)**. and select **Set as Startup Project (2)**.    

    ![Azure DevOps](images/dev301.png)

1. Right-click on the solution in the Solution Explorer **(1)** and select **Build (2)**.

    ![Azure DevOps](images/dev302.png)

1. Once the build is completed **(1)**, click on the **Start (2)** button.

    ![Azure DevOps](images/l2-05-29.png)

1. Enter `1` to create a new project using the demo generator.

    ![Azure DevOps](images/dev305.png)

1. Enter `14` to choose the Selenium template.

    ![Azure DevOps](images/dev306.png)

1. Enter `2` **(1)** to choose **PAT** as authentication method.

    - Enter your Azure DevOps organization name as **odluser<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Enter the PAT token that you have copied in the previous exercise **(3)**
    - Enter the new project name as **Selenium (4)** and you can see that the project was created successfully

      ![Azure DevOps](images/dev307.png)    

1. Navigate back to the **Azure DevOps**, you can see the newly created **Selenium project**.

    ![Azure DevOps](images/L2E4T1S16.png)

## Task 2: Configure agent on the VM

In this task, you will configure a self-hosted Azure DevOps agent on the SeleniumVM and update the release pipeline to use the Default agent pool for all deployment phases.

1. Navigate to the Azure portal, then go to **SeleniumVM** inside the **Selenium** RG.

    ![Azure DevOps](images/l2-05-30.png)

1. Click on **Connect (1)** drop down and then select **Connect (2)**.

    ![Azure DevOps](images/dev343.png) 

1. Click on **Download RDP file**.

    ![Azure DevOps](images/l2-05-31.png) 

1. Select **Keep**.

1. From the downloads, click on **Open file**.

    ![Azure DevOps](images/dev345.png) 

1. Select **Connect**.

1. Click on **More choices**.

1. Click on **Use a different User account**.

1. Log in to the VM using RDP with the following credentials and then click on **OK (3)**.

    - Username: **vmadmin (1)**
    - Password: **P2ssw0rd@123 (2)**

      ![Azure DevOps](images/dev346.png)    

1. Click on **Yes** to connect to the VM.     

1. In the SeleniumVM open **Chrome** from the desktop.

    >**Note:** Click on **Don't sign in** followed by **Skip** and then **Got it**.

1. Sign in to your Azure DevOps organization using the link https://go.microsoft.com/fwlink/?LinkId=307137.

1. If prompted, please sign in with the given credentials.

    * Email/Username: <inject key="AzureAdUserEmail"></inject>

    * Password: <inject key="AzureAdUserPassword"></inject>

1. Choose **Azure DevOps (1)**, **Organization settings (2)**.

    ![Azure DevOps](images/L4.png)

1. Choose **Agent pools (1)** and then select the **Default (1)** pool.

    ![Azure DevOps](images/L2E4T2S15.png)

1. Select the **Agents (1)** tab and choose **New agent (2)**.

    ![Azure DevOps](images/l2-05-33.png)

1. On the **Get the agent** pane, ensure that the **Windows (1)** and **x64 (2)** tabs are selected, and click **Download (3)** to download the zip archive containing the agent binaries to download it into the local **Downloads** folder within your user profile.

    ![Azure DevOps](images/dev62.png)

1. Once the download is complete, click on **Folder** icon.

    ![Azure DevOps](images/ddev4.png)

1. Right-click on the downloaded file and then click on **Extract all**.

1. Change the destination path to **C:\AzAgent (1)** then click on **Extract (2)**.

    ![Azure DevOps](images/dev310.png)

1. On the SeleniumVM, right click on **Start (1)**, then select **Command Prompt(Admin) (2)**.

    ![Azure DevOps](images/ddev5.png) 
    
1. Run the command `cd C:\AzAgent` Change the path to C:\AzAgent.

1. Type `.\config.cmd` and hit **Enter**.

1. When prompted, specify the values of the following settings:

    | Setting | Value |
    | ------- | ----- |
    | Enter server URL | Enter https://dev.azure.com/odluser<inject key="DeploymentID" enableCopy="false"/>/ (Copy completely)|
    | Enter authentication type (press enter for PAT) | **Hit Enter** |
    | Enter personal access token | The access token you recorded earlier in this task |
    | Enter agent pool (press enter for default) | **Hit Enter** |
    | Enter agent name (press enter for SeleniumVM) | **Hit Enter** |
    | Enter work folder (press enter for _work) | **Hit Enter** |
    | **(Only if shown)** Enter Perform an unzip for tasks for each step. (Press Enter for N) | **WARNING:** only press **Enter** if the message is shown|
    | Enter run agent as service? (Y/N) (press enter for N) | **Y** |
    | enter enable SERVICE_SID_TYPE_UNRESTRICTED (Y/N) (press enter for N) | **Y** |
    | Enter User account to use for the service (press enter for NT AUTHORITY\NETWORK SERVICE) | **Hit Enter** |
    | Enter whether to prevent service starting immediately after configuration is finished? (Y/N) (press enter for N) | **Hit Enter** |

    ![Azure DevOps](images/l2-05-34.png)    

1. Back on the **Agents (1)** tab of the **Default** pane, note that the newly configured agent is listed with the **Online (2)** status. Click on the **Azure DevOps (3)** label from the top left corner.

    ![Azure DevOps](images/L2E4T2S25.png)

1. Select the **Selenium** project.

1. Go to **Releases (2)** under the **Pipelines (1)** tab. Select **Selenium release (3)** definition and click on **Edit (4)**.

    ![Azure DevOps](images/L2E4T2S27.png)

1. Click on the **Tasks (1)** drop-down and  Open the **Dev (2)** environment to see the three deployment phases.

    ![Azure DevOps](images/L2E4T2S28.png)

    ![Azure DevOps](images/dev318.png)    

1. Click on **IIS Deployment phase (1)** and select the **Default (2)** Agent pool.

    ![Azure DevOps](images/dev320.png)

1. Repeat the above step for **SQL Deployment phase**.

    ![Azure DevOps](images/dev321.png)

1. Click on **Selenium tests execution** phase and set Agent pool to Default then **Save** the changes.

    ![Azure DevOps](images/dev322.png)

## Task 3: Exercise 3: Trigger Build and Release

In this task, we will trigger the Build to compile Selenium C# scripts along with the Web application. The resulting binaries are copied to the Azure VM, and finally, the Selenium scripts are executed as part of the automated Release.

1. Navigate to **Pipelines (1)** under Pipelines. Select **Selenium (2)** build pipeline.

    ![Azure DevOps](images/L2E4T3S1.png) 

1. In the **Selenium** pipeline page, select **Edit** to modify the pipeline configuration.

    ![Azure DevOps](images/L2E4T3S2.png) 

1. Select the **Pipeline (1)** settings pane, set the **Agent Specification (2)** to **windows-2022**.

    ![Azure DevOps](images/lab2-05-02.png)

1. Select **Use NuGet (1)**, set the **Version of NuGet.exe to install (2)** to **6.x**.

    ![Azure DevOps](images/lab2-05-03.png)

1. In **Phase 1**, select **Add (+) (1)**, search for **Command line (2)**, select **Command line (3)**, and then choose **Add (4)**.

    ![Azure DevOps](images/lab2-05-05.png)

1. In **Phase 1**, drag **Command Line Script** above **Build solution** and select it.

    ![Azure DevOps](images/lab2-05-06.png)

1. In the **Command Line Script** task, enter the provided command in the **Script (1)** field, then select **Save & queue (2)** and choose **Save (3)**.

    ```cmd
    choco install netfx-4.5.1-devpack -y && choco install netfx-4.6.1-devpack -y
    ```

    ![Azure DevOps](images/lab2-05-07.png)

1. In the **Save build pipeline** pane, select **Save**.

    ![Azure DevOps](images/lab2-05-08.png)

1. Navigate to **Pipelines (1)** under Pipelines. Select **Selenium (2)** build pipeline.

    ![Azure DevOps](images/L2E4T3S1.png)

1. Click **Run pipeline**.    

    ![Azure DevOps](images/L2E4T3S10.png) 

1. Click on **Run** again.

1. Click on **Phase 1**.

1. This build will publish the test artifacts to Azure DevOps, which will be used in the release.

1. Wait until the build succeeds.

    ![Azure DevOps](images/L2E4T3S14.png)

     >**Note:** It might take around 5 minutes to complete. Please ensure this pipeline is executed then proceed with the next steps.

1. Once the build is successful, the release will be triggered. Navigate back to the **Releases (1)**  tab to see the deployment in progress. Click on the **Release-1 (2)**

    ![Azure DevOps](images/L2E4T3S15.png)

    > **Note:** If any notification banner appears (such as release summary updates), close it by selecting the **X** icon.

1. When the Selenium test execution phase starts, you can see the Selenium test execution jobs. Wait until the jobs are completed successfully.

    ![Azure DevOps](images/l2-05-43.png)
    
    - If the **IIS deployment** fails, please follow the steps below to fix that.
    - In SeleniumVM search bar, search for **Services (1)** and then select **Services (2)**

      ![Azure DevOps](images/dev329.png)    

    - Right click on the **Azure Pipeline agent (1)** and the click on **Stop (2)**

      ![Azure DevOps](images/dev330.png)  

    - Right click on the **Azure Pipeline agent (1)** and the click on **Properties (2)**

        ![Azure DevOps](images/l2-05-42.png)

    - Navigate to **Log On (1)** tab, then select **Local System account (2)** and then click on **OK (3)**

      ![Azure DevOps](images/dev331.png)         

    - Click on **Start** to start the services

      ![Azure DevOps](images/dev332.png)           

    - Then navigate back to DevOps releases, click on **Redeploy** and then **Deploy**.

      ![Azure DevOps](images/lab2-05-09.png)

1. Wait for the Selenium test execution jobs completion. It might take around 10-15 minutes.       

1. In this lab, we are executing **four UI** test scenarios configured to run on Chrome and Firefox browsers.

1. Navigate to **Chrome** from the desktop, then paste the following URL: http://localhost:82/. You should be able to see the PartsUnlimited website.

    ![Azure DevOps](images/dev328.png)

1. Navigate to **Firefox** from the desktop, then paste the following URL: http://localhost:82/. 

    ![Azure DevOps](images/dev342.png)

1. Once the release succeeds, click on the **Tests** tab to analyze the test results.

    ![Azure DevOps](images/l2-05-45.png)

## Summary

In this exercise, you have accomplished the following:

- Exercise 1: Configured the lab prerequisites
- Exercise 2: Authored YAML-based Azure DevOps pipelines
- Exercise 3: Managed Azure DevOps agent pools
- Exercise 4: Implemented Selenium tests by using a self-hosted Azure DevOps agent

### You have successfully completed the lab. Click on **Next >>** to proceed with the next lab.

![Launch Azure Portal](images/dev266.png)

