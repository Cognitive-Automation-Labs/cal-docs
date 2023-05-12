# CA Labs Insights Overview

CA Labs Insights has been meticulously crafted to empower our clients in understanding the comprehensive value that their emerging technology ecosystem is generating. This innovative tool provides in-depth visibility into automation backlog opportunities, real-time metrics on the benefits derived from automated solutions, and crucial executive reports for the client's leadership team. Additionally, it elucidates the implications of operational costs associated with infrastructure and automation licensing. By embracing CA Labs Insights, clients can appreciate the overall value creation within their evolving technological landscape.

**Figure1: Overview of the CA Labs Insights**

## Related CA Labs Insights Applications

The client's Enhanced Automation Monitoring tool is specifically designed to be integrated with the following platforms:

- **UiPath**: Currently used to extract Information from automated queues for benefit tracking
- **Blue Prism**: Currently used to extract Information from automated queues for benefit tracking
- **MS Power Automate**: To be used in a future state to automation benefits
- **CA Labs Monitoring Web App**: Used to to track the real-time status of current solutions identifying anomalies and events.
- **ServiceNow**: Currently used to extract workflow information for SNOW tracking

## CA Labs Insights Information Diagram

The CA Labs Insights Extracts information from the CA Labs web app which acts as the automation register for the client's team. All changes to opportunities, automation projects and metrics surrounding solutions (such as stability and benefit per transaction) are stored in this system. The automation dashboard then combines this information with transactional data that is extracted from the various automation system (UiPath,MS Power Automate and SNOW) to effectively measure and track benefits in real time.

**Figure 2: Information Sources**

 **Note**: Automation Register information currently needs to be entered manually within the insights app while whilst Automated Transaction data is automatically extracted and updated from (ServiceNow, PowerAutomate & UiPath, Blue Prism) as needed.

## Objective

To enable holistic automation opportunity and benefit tracking across the client's organisation.

## Informational Views with Relevant Information

The Automation Dashboard Solution has been divided into 4 main views. These are as follows:

1. **CA Labs Executive View**: The Executive view provides an over-arching view that allows for a holistic understanding of the benefits that are being delivered by the client's team.
**Figure 3: Executive View**

2. **CA Labs Detailed View**: The Detailed view allows for detailed benefit tracking of each individual process that is currently in production providing insights as to how each solution is tracking against the benefits that have been signed off.
**Figure 4: Detailed View**

3. **CA Labs Operational View**: This view provides an operational lens of automation undertanding allowing the technical team to investigate the detailed operational metrics for each of the emerging technology solutiosn within the clients technology automation stack.
**Figure 5: Operational View**

4. **CA Labs Insights Insfrastucture & Licensing View**: This view provides insight on the current operational cost of the clients emerging technology ecosystem based on cloud expenditure as well as licensing costing.Additonal information around peak licensing and virtualised machine usage is provided to allow scheduling distribution, allowing for further cost optimisation.  
**Figure 6: Infrastructure & Licensing View**

## Summary of Calculated Columns in Automation Dashboard

- Actual Benefits = Actual Volume * Benefit per Transaction
- Net Benefit = 3 Year Return - Development Cost - 3 * Annual Operating Cost
- Required FTE (6 Months) = ((Sum (Development Effort) in backlog / 80% efficiency) / 4.3 weeks per developer per month) / 6 Months
- Success Rate = (Successful Transactions / (Successful Transactions + Business Exceptions +System Exceptions)) * 100
- Actual Benefits (Total) = Sum (Actual Benefit's Life to Date)
- Estimated Annualized Benefits (Total)  = Sum (Expected Annualized Benefit)

## CA Labs Client ETL Integrations

In general client ETL integrations will have been created for each of the source systems that CA labs insights interacts with. These ETL integration are described as below:

- **UiPath**: Data for the UiPath application is captured via the UiPath Orchestrator API. The script is built using python to capture the UiPath data via the Orchestrator API and then generates a CSV output for CA Labs Insigts to consume. 
- **Power Automate**: Data for the MS Power Automate Solutions is captured via Dataverse automation repository. For MS Power Automate Solutions the Flow id is used as the Queue identifier.
- **Blue Prism**: Data for the Blue Prism application is captured via the Blue Prism Control Room API. The script is built using python to capture the UiPath data via the control room API and then generates a CSV output for the CA Labs Insights to consume. 
- **ServiceNow**: Data for the ServiceNow application is captured via the ServiceNow API. The script is built using python to capture the ServiceNow data via the ServiceNow API and then generates a CSV output for CA Labs Insights to consume. 
