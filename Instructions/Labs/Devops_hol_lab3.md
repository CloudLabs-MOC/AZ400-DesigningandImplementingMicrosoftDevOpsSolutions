# Lab 3:  Enabling Continuous Integration with Azure Pipelines  

## Estimated timing: 40 Minutes

## Lab Scenario

You are a DevOps engineer at Contoso.ltd, tasked with improving the development workflow by implementing continuous integration (CI) practices. In this lab, you will define YAML-based build pipelines in Azure DevOps, configure branch policies to enforce build validation in pull requests, and manage feature branches using pull requests. You will import YAML build definitions, automate code validation, and ensure all changes are properly tested before integration, helping your team maintain high code quality and streamline delivery.

## Objectives

In this lab, you will complete the following exercises:

- Exercise 1: Include build validation as part of a Pull Request 
- Exercise 2: Configure CI Pipeline as Code with YAML


## Architecture Diagram

  ![Architecture Diagram](images/lab4-architecture-new.png)

# Exercise 1: Include build validation as part of a Pull Request 

In this exercise, you will configure branch policies on the main branch to enforce pull request validation and ensure code reviews before merging changes. You will then create and manage a pull request in Azure DevOps, merging changes from a new branch into the protected main branch.

## Task 1: Branch Policies

In this task, you will add policies to the main branch and only allow changes using Pull Requests that comply with the defined policies. You want to ensure that changes in a branch are reviewed before they are merged.

1. On your lab vm, in a browser window, click on **Azure DevOps** from the top left corner. Click on **eShopOnWeb**.

1. Go to **Repos (1)>Branches (2)** section. On the **Mine** tab of the **Branches** pane, hover the mouse pointer over the **main (3)** branch entry to reveal the **ellipsis symbol (4)** on the right side. Select **Branch Policies (5)**.

    ![](images/l3-05-01.png)

1. On the main tab of the repository settings, enable the option for **Require minimum number of reviewers (1)**. Add **1 (2)** reviewer and check the box **Allow requestors to approve their own changes (3)**(as you are the only user in your project for the lab)

    ![](images/l3-05-02.png)

1. Click on the **main (1)** tab of the repository settings, in the **Build Validation (2)** section, **click + (Add a new build policy) (3)**.

    ![](images/l3-05-03.png)

1. On the Build pipeline list, select **eshoponweb-ci-pr (4)** then click **Save (5)**.

    ![](images/dev88.png)
      
     >**Note:** If you get any error while saving the branch validation, refresh the page and try again.

## Task 2: Working with Pull Requests
 
In this task, you will use the Azure DevOps portal to create a Pull Request, using a new branch to merge a change into the protected main branch.
 
1. Navigate to the **Repos (1)->Branches (2)** section in the eShopOnWeb navigation and click **New Branch (3)**.

    ![](images/l3-05-04.png)

1. Create a new branch named **Feature01 (1)** based on the **main** branch and click **Create (2)**.

    ![](images/dev90.png)

1. Click **Feature01**.

1. Navigate to the **/eShopOnWeb/src(1)/Web(2)/Program.cs (3)** file as part of the **Feature01** branch.

    ![](images/dev91.png)
    
    ![](images/dev92.png)

1. Click on **Edit**.

    ![](images/l3-05-05.png)

1. Add the following line on the first line **(1)** and click on **Commit (2)**.
    
   ```
   // Testing my PR
   ```

    ![](images/dev94.png)
   
1. Click on **Commit** again (leave default commit message).

    ![](images/l3-05-06.png)

1. A message will pop up, proposing to create a Pull Request (as your **Feature01** branch is now ahead in changes, compared to **main**). Click on **Create a Pull Request**.

    ![](images/dev95.png)

1. In the **New pull request** tab, leave defaults and click on **Create**.
   
    ![](images/l3-05-07.png)
   
1. The Pull Request will show some pending requirements, based on the policies applied to the target **main** branch.

    - It shows **At least 1 user should review and approve the changes (1)**, 
    - Click **Add (2)** select **Required Reviewer (3)**

      ![](images/l3-05-08.png)    
    
1. Search for the ODL user email **<inject key="AzureAdUserEmail"></inject> (1)** and select from the list **(2)**. 

    ![](images/dev98.png)

1. Build validation, you will see that the build **eshoponweb-ci-pr** was triggered automatically
         
    ![](images/l3-05-09.png)    

1. On the top-right, click on **Approve**.

    ![](images/dev267.png)    
      
1. Wait for the validation to succeed before proceeding.

    ![](images/l3-05-10.png)

     >**Note:** It might take around 2-3 minutes.

1. Now from the **Complete (1)** dropdown you can click on **Complete (2)**. 

    ![](images/l3-05-11.png)

1. On the **Complete Pull Request** tab, select only **Complete associated work items after merging (1)** checkbox  and Click on **Complete Merge (2)**

   ![](images/l3-05-12.png)

   ![](images/l3-05-13.png)


# Exercise 2: Configure CI Pipeline as Code with YAML

In this exercise, you will configure a CI pipeline as code using YAML. You will import the YAML build definition, enable Continuous Integration for automatic builds, and test the pipeline by creating a pull request to trigger the CI process when merging changes into the protected main branch.

## Task 1: Import the YAML build definition

In this task, you will add the YAML build definition that will be used to implement Continuous Integration.

Let's start by importing the CI pipeline named **eshoponweb-ci.yml**.

1. Go to **Pipelines (1)>Pipelines (2)** and click on **New Pipeline (3)** button.

    ![](images/l3-05-14.png)

1. Select **Azure Repos Git (YAML)**.

    ![](images/l3-05-15.png)

1. Select the **eShopOnWeb** repository.

    ![](images/dev106.png)

1. Select **Existing Azure Pipelines YAML File**

    ![](images/dev107.png)

1. Select the **/.ado/eshoponweb-ci.yml (1)** file then click on **Continue (2)**

    ![](images/dev108.png)

    The CI definition consists of the following tasks:
     
    - **DotNet Restore:** With NuGet Package Restore, you can install all your project's dependencies without having to store them in source control.
       
    - **DotNet Build:** Builds a project and all of its dependencies.
       
    - **DotNet Test:** .Net test driver used to execute unit tests.
       
    - **DotNet Publish:** Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's             **Build.ArtifactStagingDirectory**.
       
    - **Publish Artifact - Website:** Publish the app artifact (created in the previous step) and make it available as a pipeline artifact.
       
    - **Publish Artifact - Bicep:** Publish the infrastructure artifact (Bicep file) and make it available as a pipeline artifact.
       
              
## Task 2: Enable Continuous Integration
   
The default build pipeline definition doesn't enable Continuous Integration.

In this task, you will enable Continuous Integration by modifying the YAML build definition to trigger on changes to the main branch and web application code, then create and complete a pull request to merge the changes.
   
1. Now, you need to replace the **trigger: none** code in `line 8` with the following code:
   
    ```
      trigger:
       branches:
        include:
        - main
      paths:
        include:
        - src/web/*
    ``` 

     ![](images/dev110.png)

      >**Note:** Be careful with copy/paste, make sure you have the same indentation shown above.
      
      >**Note:** This will automatically trigger the build pipeline if any change is made to the main branch and the web application code (the src/web folder). Since you enabled Branch Policies, you need to pass a Pull Request to update your code. 
    
1. Click the on the **Save and run (1)** dropdown and **Save (2)** button (not **Save and run**) to save the pipeline definition.

    ![](images/l3-05-16.png)
  
1. Select **Create a new branch for this commit (1)** Keep the default branch name and **Start a pull request(2)** checked. and Click on **Save(3)**

    ![](images/dev112.png)

1. Your pipeline will take a name based on the project name. Let's **rename** it to identify the pipeline better. Click on the **ellipsis (1)** and **Rename/move (2)** option.

    ![](images/l3-05-17.png)

1. Name it **eshoponweb-ci (1)**  and click on **Save (2)**.

    ![](images/dev116.png)

1. Go to **Repos (1)>Pullrequests (2)** and click on the existing pull request **(3)**. 

    ![](images/l3-05-18.png)

1. Click on **Approve** on the top-right.

    ![](images/l3-05-19.png)

1. Once the **Required checks are succeeded (1)**, click on **Complete (2)** drop down and then select **Complete (3)**.

    ![](images/dev269.png)

1. On the **Complete Pull Request** tab, select only **Complete associated work items after merging (1)** checkbox  and Click on **Complete Merge (2)**

    ![](images/l3-05-12.png)

    ![](images/dev120.png)
  
## Task 3: Test the CI pipeline
 
 In this task, you will create a Pull Request, using a new branch to merge a change into the protected main branch and automatically trigger the CI pipeline. Navigate to the Repos section.
 
1. Navigate to the **Repos (1)->Branches (2)** section. Create a **New branch (3)**.

    ![](images/l3-05-20.png)

1. Create a branch named **Feature02 (1)** based on the **main** branch and Click on **Create (2)**.    
    
    ![](images/az-400-lab3-9.png)

1. Click the new **Feature02** branch.

1. Navigate to the **/eShopOnWeb/src (1)/Web (2)/Program.cs (3)** file.

    ![](images/l3-05-21.png)
    
    ![](images/dev123.png)

1. Click on **Edit (1)** to remove the first line // **Testing my PR (2)**.
   
    ![](images/dev124.png)

1. Click on **Commit**.    
   
    ![](images/dev125.png)

1. Click on **Commit** (leave default commit message).
   
    ![](images/l3-05-22.png)

1. A message will pop up, proposing to create a Pull Request (as your **Feature02** branch is now ahead in changes, compared to main).

1. Click on **Create a Pull Request**

    ![](images/dev127.png)

1. In the **New pull request** tab, leave defaults and click on **Create**. The Pull Request will show some pending requirements, based on the policies applied to the target **main** branch. Wait until the  build completes.

    ![](images/dev128.png)

1. Click on **Approve** from top right.

    ![](images/dev270.png)

1. Once the **Required checks are succeeded (1)**, click on **Complete (2)** drop down and then select **Complete (3)**.

    ![](images/l3-05-23.png)

     >**Note:** Please wait. The required checks might take 3-5 minutes to complete.     

1. On the **Complete Pull Request** tab, select only **Complete associated work items after merging (1)** checkbox  and Click on **Complete Merge (2)**.

     ![](images/l3-05-12.png)

1. Go back to **Pipelines (1)>Pipelines (2)**, you will notice that the build **eshoponweb-ci (3)** was triggered automatically after the code was merged. Select it.

    ![](images/l3-05-24.png)
 
1. On the **eshoponweb-ci** build, select the last run.

    ![](images/dev259.png)

1. After its successful execution, click on **Related (1) > Published (2)** to check the published artifacts:
           
    ![](images/l3-05-25.png) 
     
    - **Bicep:** the infrastructure artifact  
    - **Website:** the app artifact
     
      ![](images/l3-05-26.png)
     
## Review
  
In this lab, you learned how to define and manage build pipelines in Azure DevOps using YAML. You configured branch policies for build validation in pull requests, worked with feature branches, and set up a CI pipeline as code. The lab included importing YAML build definitions, enabling continuous integration, and testing the pipeline to automate and validate code changes efficiently.

In this lab, you have accomplished the following:

- Exercise 1: Included build validation as part of a Pull Request 
- Exercise 2: Configured CI Pipeline as Code with YAML


### You have successfully completed the lab. Click on **Next >>** to proceed with the next lab.

