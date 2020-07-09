# RefreshAASwithADF
An approach using only Azure Data Factory pipelines for refreshing Azure Analysis Service tabular models.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjondobrzeniecki%2FRefreshAASwithADF%2Fmaster%2Farm_template.json)

## Prerequisites
* Create a Service Principal (SPN) To learn about creating a Service Principal, see <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal">Create a service principal by using Azure portal</a>.
* Add SPN as an Analysis Services Administrator using SQL Server Management Studio (SSMS), see <a href="https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-addservprinc-admins#using-sql-server-management-studio">here</a>.
