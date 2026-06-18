# Lab 5: Implementing Security and Compliance in an Azure Pipeline 

### Estimated duration: 30 Minutes

## Lab Scenario

You are a DevOps engineer for Contoso.ltd, responsible for ensuring that all code passing through the CI/CD pipeline complies with security and cost management standards. In this lab, you will integrate the Mend Bolt extension into your Azure DevOps pipeline to scan for vulnerabilities in open-source components used by your .NET application. You will configure the pipeline to run Mend Bolt during the build stage, automatically detecting security risks and generating vulnerability reports. Additionally, you will review and disable unnecessary Azure DevOps billing features to optimize operational costs. This lab equips you with the skills to implement secure, compliant, and cost-effective DevOps practices as part of Contoso’s governance and quality assurance initiatives.

## Objectives

In this lab, you will complete the following exercises:

- Exercise 1: Implement Security and Compliance in an Azure DevOps pipeline by using Mend Bolt 

# Exercise 1: Implement Security and Compliance in an Azure DevOps pipeline by using Mend Bolt 

In this exercise, you will implement security and compliance in an Azure DevOps pipeline using Mend Bolt. You will activate the Mend Bolt extension, create and trigger a build to scan for vulnerabilities in open source components, and remove Azure DevOps billing to avoid unnecessary charges.

## Task 1: Activate the Mend Bolt extension 

In this task, you will activate the Mend Bolt extension in Azure DevOps by installing it from the marketplace and setting up the free version for use in your pipeline.

1. On the Azure DevOps page, click on **Azure DevOps (1)** located in the top left corner.Then click on **Organization Settings (2)** at the bottom  left corner. 

    ![Azure DevOps](images/L4.png)

1. Navigate to **Extensions (1)** under **General** and click on **Browse marketplace (2)**.

    ![Azure DevOps](images/L5E1T1S3.png)

1. Search for **Mend Bolt (1)**, click on **Search (2)** icon and then select **Mend Bolt** from the results **(3)**.

    ![Azure DevOps](images/l4-05-74.png)

1. Select **Get it free**.

    ![Azure DevOps](images/l4-05-75.png)

1. Click on **Install**.

    ![Azure DevOps](images/l4-05-76.png)

1. Click on **Proceed to organization**.

    ![Azure DevOps](images/l4-05-78.png)

1. On the **Organization Settings**, select **Mend (1)** under Extensions. Provide your First name, Last name, Work Email, Company Name, and other details **(2)** and then click **Create Account (3)** button to start using the Free version.    

    ![Azure DevOps](images/L5E1T1S8.png)


## Task 2: Create and Trigger a build 

In this task, you will create and trigger a build in Azure DevOps by editing an existing pipeline, adding the Mend Bolt extension to scan for vulnerabilities, and running the pipeline to analyze the open source components for security risks and vulnerabilities.

1. On the **Organization Setting** page, click on **Azure DevOps** located at top left corner.

    ![Azure DevOps](images/L5E1T2S1.png)

1. Select the **eShopOnWeb_MultiStageYAML** project.

    ![Azure DevOps](images/L5E1T2S2.png)

1. Select **Pipelines (1)** under **Pipelines** section, then select the recent pipeline **(2)**.

    ![Azure DevOps](images/L5E1T2S3.png)

1. Click on **Edit**.

    ![Azure DevOps](images/L5E1T2S4.png)

1. On the **Show assistant**, Search for **Mend (1)** and select **Mend Bolt (2)** from the results.

    ![Azure DevOps](images/l4-05-83.png)

1. Under the **Project name**, enter **eShopOnWeb_MultiStageYAML (1)** and then click on **Add (2)**.

    ![Azure DevOps](images/l4-05-85.png)

1. Enter 2 Tab spaces, make sure the alignment is there as in the screenshot **(1)**, and then click on **Validate and save (2)**.

    ![Azure DevOps](images/l4-05-86.png)

1. Click on **Save**.

    ![Azure DevOps](images/l4-05-87.png)

1. Click on **Run** to run the pipeline.

    ![Azure DevOps](images/l4-05-88.png)

1. Click on **Run** again.

    ![Azure DevOps](images/l4-05-89.png)

1. Click on **build**.

    ![Azure DevOps](images/L5E1T2S11.png)

1. Once the build is completed **(1)**, click back navigation **(2)** to see the summary, which shows Test results, Build artifacts, etc., as shown below.    

    ![Azure DevOps](images/L5E1T2S12.png)

    > **Note:** Please ensure this pipeline is executed then proceed with the next steps.

1. Navigate to **Mend Bolt** tab. This shows the list of all vulnerable open source components with Vulnerability Risk, Vulnerable Libraries, and Severity Distribution.

    ![Azure DevOps](images/L5E1T2S13.png)

## Task 3: Remove the Azure DevOps billing

In this task, you will remove pipeline billing to eliminate unnecessary charges.

1. On the lab computer, switch to the browser window displaying the Azure DevOps organization homepage by clicking on **Azure DevOps** from the top left corner.

   ![Branch Policies](images/L5E1T3S1.png)

1. Select **Organization Settings** at bottom left corner.

   ![Branch Policies](images/L4.png)

1. Under **Organization Settings** select **Billing (1)** from the left pane and click on **Change billing (2)** button to open Change billing pane.

   ![Branch Policies](images/L5E1T3S3.png)

1. In the **Change billing** pane, select **Remove billing (1)** setting and click on **Save (2)**.      

   ![Branch Policies](images/l4-05-95.png)


## Summary

In this lab, you have accomplished the following:

- Exercise 1: Implemented Security and Compliance in an Azure DevOps pipeline by using Mend Bolt 

### You have successfully completed the lab.

By completing the Implementing DevOps with GitHub and Azure DevOps hands-on labs, you have gained practical experience in building end-to-end DevOps pipelines across GitHub and Azure DevOps. You learned how to implement CI/CD using GitHub Actions, configure Azure DevOps projects, and design YAML-based pipelines with both Microsoft-hosted and self-hosted agents. You also explored pull request workflows, branch policies, and continuous integration practices to enforce code quality and collaboration. Additionally, you implemented multi-stage release pipelines with deployment gates, monitored application health using Application Insights, and ensured secure and compliant delivery by integrating vulnerability scanning and cost management practices.
