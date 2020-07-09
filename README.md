# Refreshing Azure Analysis Services models using only Azure Data Factory pipelines.

There are well documented approaches for using Azure Logic Apps and Azure Automation that you can then orchastrate with Azure Data Factory to refresh tabular models deployed to Azure Analysis Services. The approach covered in this document leverages only the Azure Analysis Services APIs for <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-async-refresh">asynchronous refresh</a> and Azure Data Factory pipeline activies to perform tabular model refreshes natively inside Azure Data Factory.  This simplifies implementation by removing the need for additional cloud services to accomplish the same goal.

## Prerequisites
* Create a Service Principal (SPN), see <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal" target="_blank">here</a>.
* Add SPN and Data Factory MI as an Analysis Services Administrator using SQL Server Management Studio (SSMS), see <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-addservprinc-admins#using-sql-server-management-studio" target="_blank">here</a>.
  * Use <b>Manual Entry</b> with the following format, ```app:<ApplicationID>@<TenantID>```
  * <a href="https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity#retrieve-managed-identity-using-azure-portal" target="_blank">Retrieve Data Factory Managed Identity (MI)</a>.
* Create Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#create-a-vault" target="_blank">here</a>.
* Create Azure Key Vault access policy for Data Factory MI to <b>get</b> secrets, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Steps 1 and 2 here</a>.
* Add the Azure Tenant ID, SPN Client ID and SPN Client Secret to Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault" target="_blank">here</a>.

## Deployed Data Factory Objects
* Refresh AAS pipeline
* SubmitRefreshResponse dataset
* AASRefreshServer linked service

## Deployment

<b>Step 1:</b> Use deployment button below to open ARM Template deployment page in Azure Portal

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjondobrzeniecki%2FRefreshAASwithADF%2Fmaster%2Farm_template.json)

<b>Step 2:</b> Select the <b>Subscription</b>, <b>Resource Group</b> and <b>Region</b> that the data factory being deployed to resides in.

<b>Step 3:</b> Supply the <b>Factory Name</b> parameter with the name of the data factory being deployed to.

<b>Step 4:</b> Supply the <b>Aas Region</b>, <b>Aas Server</b> and <b>Aas Model</b> parameters with the details of the Azure Analysis Service Tabular model.

<b>Step 5:</b> Retrieve Key Vault <b>Secret Identifiers</b> for Azure Tenant ID, SPN Client ID and SPN Client Secret, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Step 3 here</a>.

<b>Step 6:</b> Supply the <b>Secret Identifier</b> parameters with the corresponding values from Step 5.

<b>Step 7:</b> Press <b>Review + create</b> button!

## Data Factory Pipeline Overview

The Refresh AAS pipeline uses all native activites to authenticate with Azure, submit a refresh tabular model refresh request and then monitor refresh progress.  It orchestrates the approach covered in the <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-async-refresh">Asynchronous refresh with the REST API</a> document.

The <b>Lookup</b> activity is used to overcome a change in behavior experienced with the POST refresh API that is used to submit a refresh.  The <b>Web</b> activity response does not return an Operation ID as expected that is required to monitor the refresh progress in the <b>Until</b> loop activity.  Issuing the POST refresh API request using the <b>Lookup</b> activity returns the Operation ID as expected.

![Refresh AAS Data Factory Pipeline](https://github.com/jondobrzeniecki/RefreshAASwithADF/blob/master/img/RefreshAASPipeline.jpg?raw=true)

The image below is the set of activies inside the <b>Until</b> loop activity that continously checks the refresh progress until it reaches the <i>succeeded</i> state.  The default <b>Wait</b> time is set to 5 seconds.

![Refresh AAS Until Loop Data Factory Pipeline](https://github.com/jondobrzeniecki/RefreshAASwithADF/blob/master/img/RefreshAASPipelineUntilLoop.jpg?raw=true)

## Limitations
* SPN has to be used for submitting refresh due to the use of the Lookup activity, which does not support MSI authentication. For a similar approach that leverages MSI, please refer to this <a href="https://github.com/furmangg/automating-azure-analysis-services#processazureas">example</a>.
* No error handling provided. This is a base example of submitting and concluding an asynchronous refresh.

## License
MIT
