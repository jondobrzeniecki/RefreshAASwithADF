# RefreshAASwithADF
An approach using only Azure Data Factory pipelines for refreshing Azure Analysis Service tabular models.


## Prerequisites
*1. Create a Service Principal (SPN), see <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal" target="_blank">here</a>.
*2. Retrieve Data Factory Managed Identity (MI), see <a href="https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity#retrieve-managed-identity-using-azure-portal" target="_blank">here</a>. 
*3. Add SPN and Data Factory MI as an Analysis Services Administrator using SQL Server Management Studio (SSMS), see <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-addservprinc-admins#using-sql-server-management-studio" target="_blank">here</a>.
* Create Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#create-a-vault" target="_blank">here</a>.
* Create Azure Key Vault access policy for Data Factory MI to <b>get</b> secrets, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Steps 1 and 2 here</a>.
* Add the Azure Tenant ID, SPN Client ID and SPN Client Secret to Azure Key Vault, see <a href="https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault" target="_blank">here</a>.
* Retrieve Key Vault Secret Identifiers for Azure Tenant ID, SPN Client ID and SPN Client Secret, see <a href="https://docs.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities#steps" target="_blank">Step 3 here</a>.

## Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjondobrzeniecki%2FRefreshAASwithADF%2Fmaster%2Farm_template.json)
