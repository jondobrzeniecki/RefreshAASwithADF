# Refreshing Azure Analysis Services models using only Azure Data Factory pipelines.

There are well documented approaches for using Azure Logic Apps and Azure Automation that you can then orchastrate wit Azure Data Factory. This approach however leverages the Azure Analysis Services APIs for <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-async-refresh">asynchronous refresh</a> and Azure Data Factory pipeline activies to perform tabular model refreshes natively inside Azure Data Factory.


## Prerequisites
* Create a Service Principal (SPN), see <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal" target="_blank">here</a>.
* Add SPN and Data Factory MI as an Analysis Services Administrator using SQL Server Management Studio (SSMS), see <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-addservprinc-admins#using-sql-server-management-studio" target="_blank">here</a>.
  * Use <b>Manual Entry</b> with the following format, ```app:<ApplicationID>@<TenantID>```
  * <a href="https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity#retrieve-managed-identity-using-azure-portal" target="_blank">Retrieve Data Factory Managed Identity (MI)</a>.
* Create Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#create-a-vault" target="_blank">here</a>.
* Create Azure Key Vault access policy for Data Factory MI to <b>get</b> secrets, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Steps 1 and 2 here</a>.
* Add the Azure Tenant ID, SPN Client ID and SPN Client Secret to Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault" target="_blank">here</a>.

## Deployment

<b>Step 1:</b> Retrieve Key Vault Secret Identifiers for Azure Tenant ID, SPN Client ID and SPN Client Secret, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Step 3 here</a>.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjondobrzeniecki%2FRefreshAASwithADF%2Fmaster%2Farm_template.json)
