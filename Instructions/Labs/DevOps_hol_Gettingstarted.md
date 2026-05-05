# Implementing DevOps with GitHub and Azure DevOps

### Overall Estimated Duration: 4 Hours

## Introduction - Overview of GitHub and Collaborative Development

In this hands-on lab, you will learn how to implement end-to-end DevOps practices using Azure DevOps and GitHub. You will begin by deploying the eShopOnWeb application using GitHub Actions, creating a service principal, and securing credentials with GitHub secrets. Next, you'll configure self-hosted agents and use YAML pipelines to enable code-based CI/CD workflows with support for branching, versioning, and pull requests. You will define build pipelines, enforce branch policies for continuous integration, and manage releases using gates that monitor app health and alerts. Finally, you'll integrate the Mend Bolt extension to secure your pipelines by detecting open-source vulnerabilities, ensuring secure and compliant deployments.


## Objectives

By the end of this lab, you will be able to:

- **Implementing GitHub Actions for CI/CD**: During this hands-on session, participants will learn how to implement a GitHub Action workflow to deploy an Azure web app using DevOps. They will configure GitHub Actions for continuous integration and deployment, connect to Azure services, and automate the deployment process for a streamlined DevOps pipeline.

- **Configuring Agent Pools and Understanding Pipeline Styles**: During this hands-on session, participants will learn how to configure self-hosted Azure DevOps agents, build and release pipelines using YAML, and integrate Selenium testing for automated validation—enabling end-to-end CI/CD automation with version-controlled pipeline definitions.

- **Enabling Continuous Integration with Azure Pipelines**: During this hands-on session, participants will define and manage YAML-based build pipelines in Azure DevOps, configure branch policies, and implement continuous integration to automate and validate code changes through pull requests.

- **Controlling Deployments using Release Gates**: During this hands-on session, participants will configure deployment gates in Azure Pipelines to control and automate application releases across environments, ensuring deployments proceed only when predefined health and compliance checks are met.

- **Implementing Security and Compliance in an Azure Pipeline**: During this hands-on session, participants will implement security and compliance in an Azure DevOps pipeline by integrating the Mend Bolt extension to scan for vulnerabilities, analyze security risks during builds, and manage costs by disabling unnecessary billing.
  
## Prerequisites

- Basic knowledge of Azure DevOps and GitHub repositories.

- Familiarity with CI/CD concepts and pipeline workflows.

- Access to an active Azure subscription and Azure DevOps organization.

- Understanding of YAML syntax and Git-based version control.

- Basic experience working with Azure Web Apps and Application Insights.

## Getting Started with the Lab
 
## Accessing Your Lab Environment
 
Once you are ready to dive in, your virtual machine and **Guide** will be at your fingertips within your web browser.

   ![](images/gdev1.png)

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕: 100%** icon next to the lab environment's timer.

   ![Manage Your Virtual Machine](images/dpg2.png)

## Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.
 
   ![Explore Lab Resources](images/gdev2.png)
 
## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the top right corner.
 
 ![Use the Split Window Feature](images/gdev3.png)
 
## Managing Your Virtual Machine
 
Feel free to **Start, Stop, or Restart (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!
 
 ![Manage Your Virtual Machine](images/gdev4.png)

## Lab Validation

1. After completing the task, hit the **Validate** button under the Validation tab integrated into your lab guide. You can proceed to the next task if you receive a success message. If not, carefully read the error message and retry the step, following the instructions in the lab guide.

   ![Inline Validation](images/gdev5.png)

1. If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.


## Let's Get Started with Azure Portal

1. On your virtual machine, click on the **Azure Portal** icon as shown below:

   ![Launch Azure Portal](images/afg6.png)

1. You will see the **Sign in to the Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
       ![Enter Your Username](images/afg7.png)
 
1. Next, provide your password:
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>
 
       ![Enter Your Password](images/l1-05-g1.png)

1. If you see the pop-up **Stay Signed in?**, click **No**.       

   ![Enter Your Password](images/l1-05-g2.png)


This hands-on lab will guide you through implementing end-to-end DevOps practices using Azure DevOps and GitHub. You will deploy an Azure web app with GitHub Actions, set up CI/CD automation with YAML pipelines, and configure release gates based on app health. Additionally, you will integrate the Mend Bolt extension to detect open-source vulnerabilities, ensuring security and compliance in your pipeline.

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance anytime. We offer dedicated support channels tailored specifically for learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: cloudlabs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

Click **Next** from the lower right corner to move on to the next page.

![Launch Azure Portal](images/dev266.png)

## Happy Learning!!
