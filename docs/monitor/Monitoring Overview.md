# CA Labs Monitoring User Overview 

## Introduction 

CA Labs monitoring provides enhanced monitoring capabilities necessary for swift warning and incident responses to emerging technology solutions deployed within the client's organization. Enhanced Monitoring includes several monitoring modules that track unwanted anomalies such as process terminations and missed schedules. The primary benefit of the Enhanced Monitoring solution is that support staff no longer need to constantly review the state of solutions since they are notified in real time when run-time errors and warning`s arise in real time. This results in improved response times for incidents whilst reducing the manual support effort required for this. 

## Related Monitoring Tool Applications 

CA Labs Monitoring tool is specifically designed to be integrated with the following platforms: 

- **UiPath:** Currently used to track anomalies as described in the events above. 
- **Power Automate:** To be used in a future state to track anomalies similar to the events described above 
- **Blue Prism:** Currently used to track anomalies and run time errors of the various solutions 
- **ServiceNow:** Use to create incident tickets 

## Objective 

To enable real-time monitoring of automations through alert notifications. 

## Types of Notifications 

The Enhanced Automation Monitoring Tool has been set up to provide 2 levels of notifications, namely warnings and incidents.  

### Incidents 

Incident categorisations are deemed the highest priority and require timely intervention to ensure ACU`s production environment maintains high levels of responsiveness to issues when they arise. As such an incident event results in an MS Teams channel notification, as well as the generation of a SNOW ticket which requires action. Incident tickets will contain the following information: 

- Release name: The given name of the faulted process automation 
- HostMachineName: The name of the host machine on which the current machine is running 
- StartTime: The time at which the given process started  
- EndTime: The time at which the process was deemed to be faulted  
- State: Type of faulted state 
- Info: Detailed information captured from the logs which describe the termination reason 

#### Predominant Incident Types 

- **Bot Terminations:** Triggered every 5 minutes when a process has terminated. 

Example of an automatically generated incident notification 

### Warnings 

Warnings are deemed a lower priority and may not yield immediate production impact. As a result, only MS Teams channel notifications are created for this event. Information contained in warning notifications will vary depending on the nature of the warning. Some example information of warning notifications are described below: 

- ReleaseName: The given name of the faulted process automation 
- Description: Some processes have specific information that's described here (e.g. Max log size of XXX logs reached with log size of XXX records) 
- CreationTime: The time at which the given process job was triggered to start 
- StartTime: The actual time at which the process job was started after the CreationTime 
- Endtime: The time at which the process job ended 
- HostMachineName: The name of the host machine on which the current machine is running 
- State: Type of state the process job is in (e.g. Pending, Faulted, Running, Completed) 
- Time Waiting: The amount of time the process job has been in “pending” state (i.e. time between CreationTime and StartTime) 

Example of an automatically generated warning ticket 

#### Predominant Warnings Types 

- **Missed Process Schedules:** Triggered once when a process has been in Pending state for longer then 5 minutes (i.e. defined by time between CreationTime and StartTime) 
- **Infinite Loops:** Triggered every 60 minutes when a process has ran for longer than the 95th percentile runtime when compared to all historical process

run times for that particular process. Another component of this process is a function called Identify Runtime which is triggered every 35 minutes to get the average and 95th percentile runtimes for all processes. 
- **Processes Stuck in Pending:** Triggered every 60 minutes if a process has been stuck in Pending state for longer than 60 minutes 
- **Large Session Logs:** Triggered every 60 minutes when a process job ends and the session logs of that job are larger than expected (e.g. 5 megabytes) 
- **Low Completion Rates:** Triggered every 5 minutes when a process jobs ends and if the transaction queue completion rate of that job is below the expected threshold (e.g. 50%)  

## How to work with the Monitoring Tool 

### How to Add Team Members to notification channels 

As an Incident or Warning channel owner, you can add or remove members, and edit channel settings. Each person that you add must first be a member of the prescribed team. (Automation Business / Robotic)  

To add members of your team to a private channel: 

1. Next to the private channel name, select More options  >  Add members. 
2. Use the Members and Settings tabs to add or remove members and assign roles. Your private channel can have multiple owners and up to 250 members. 
3. When you’re ready, select Done.  

### Creating and Resolving ServiceNow Incident Tickets 

For each Automation Incident, a SNOW ticket will be created. This can then then be actioned using typical SNOW procedure as shown below: 

#### Role requirements 

Role required: itil, list_updater, sn_incident_write, or admin (for resolution) and itil_admin or admin (for closure) 

#### Procedure 

1. Navigate to Incident > Open 
2. Open the incident that you want to resolve and close. 
3. In the Resolution Information section, fill in the fields as follows: 
4. Click Resolve. The incident is in a resolved state. 
5. Click Close Incident. The incident is closed. 

## Monitoring Tool Architecture and Infrastructure 

The Monitoring Tool solution is hosted within the ACU Azure Environment and sits behind the ACU Private Network. To better understand how the solution works, review the architectural diagrams below. If you have any other questions or concerns, please reach out to support@cognitiveautomationlabs.com and we will be happy to assist. 

### Solution Architecture 

The solution is designed to pull data from the applications it monitors, identifies if any anomalies have occurred as described in the code logic, send notifications for warnings and incidents, and create service management tickets for any incidents.  

Monitoring Tool Solution Architecture 

### Network Architecture 

The core networking components of the solution include the ACU On-Prem Network and Azure Backbone. Developers use the Global Protect VPN to access the UiPath Orchestrator for development purposes.  

Once the solution is deployed to the TST or PRD Resource Groups, solution Container Instances will then connect to the UiPath Orchestrators over the ACU private network by using Azure Virtual Networks that have been configured with the appropriate access. Container Images for each solution are stored in the Container Registry that relates to each environment and any secret values for the solutions are stored in the Azure Key Vault of each environment.  

Monitoring Tool Network Architecture 

## DevOps Pipelines 

### Overview 

The core components of the Monitoring Tool DevOps pipelines include GitHub, local DEV laptop machines, and the various Azure Resource Group for each environment (DEV, TST, PRD).  

GitHub is used to store the code of the Monitoring Tool solution and to manage CICD pipelines for the various Azure Resource Group environments via GitHub Actions. Azure Service Principals are used to grant

GitHub Actions with access to deploy resources via the CICD pipelines. If you would like access to the code or to know more about the CICD pipelines, reach out to support@cognitiveautomationlabs.com. 

Developers that wish to make changes or run the code locally first need to have Docker installed and access to the ACU private network via Global Protect VPN. See the solution readme file for more information on this topic. 

### Azure Resource Deployment Pipeline 

This pipeline will be triggered whenever changes occur to files that relate specifically to Azure resources (e.g. ARM templates). The pipeline deploys all the related Azure Resources and ensures they are configured with the appropriate settings. This pipeline is a pre-requisite for the Monitoring Tool Build Pipeline and is only reran if Azure infrastructure related items change. 

### Monitoring Tool Build Pipeline 

This pipeline will be triggered whenever changes occur to files that relate specifically to the Monitoring Tool solution (e.g. python files, docker files). The pipeline first runs docker compose to configure and build the container image, and it then deploys that image to the Azure Container Registry and Container Instance resource. This pipeline requires all related Azure Resources to already be deployed via the Azure Resource Deployment Pipeline and after this pipeline runs the Monitoring Tool should be up and running.  

### Branch Management 

Branches are managed via three different levels, which include the following: 

- **feature/feature_name:** When a developer wishes to make changes, they create a feature branch based off the main branch and assign an appropriate name (feature_name). This is the working branch for the developer and they will create pull request into main when they are ready to merge with main branch. 
- **main:** This branch is used as the primary branch and it also triggers the deployment to the TST Resource Group. When the changes to files in this branch are approved, the CICD pipelines will be triggered and changes will be deployed to the TST Resource Group. 
- **release:** This branch is used as the Production branch. When the changes to files in this branch are approved, the CICD pipelines will be triggered and changes will be deployed to the PRD Resource Group. 

DevOps Pipelines Architecture 


