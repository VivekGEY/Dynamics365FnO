
Dynamics 365 for Operation provides two primary sets of APIs, Recurring Integration APIs & Data Management Platform (DMF) package APIs, to support file based integration scenarios. These APIs allow DMF data files as well as data projects to be imported and exported from Dynamics 365 for Operations. 
Information about using Recurring Integration APIs are documented here

<https://docs.microsoft.com/en-us/dynamics365/operations/dev-itpro/data-entities/recurring-integrations>


In following articles, we would showcase a sample Azure Logic App that orchestrates the file integration flow between OneDrive and Dynamics 365 for Operations using DMF package APIs. 

Inbound flow: File originates from OneDrive and sent to Dynamics 365 for Operations

![Inbound Image](https://user-images.githubusercontent.com/22554479/27503704-c65cc106-5833-11e7-9484-08bd3aa31e3b.png)

Outbound flow: File originates from Dynamics 365 for Operations and sent to OneDrive

![Outbound Image](https://user-images.githubusercontent.com/22554479/27503676-3b4bf5aa-5833-11e7-89c7-1e3a362cc240.png)


### Inbound Flow
1.	As soon as a file is added in OneDrive folder ‘Input’, the logic app gets triggered. 
2.	The file is then moved to ‘Processing’
3.	The Dynamics 365 Operations connector is used to invoke ‘DataManagementDefinitionGroups/GetAzureUrl’ action.
	* This action is invoked to request Dynamics 365 for Operations for an writable blob url to a container within default Azure blob storage (Microsoft Subscription)
	* This returns a BlobID and BlobUrl from AX. 
	* The BlobUrl denotes the location where data packages should be uploaded
4.	The standard HTTP connector is used to upload the data package file to the BlobUrl location. 
5.	The Dynamics 365 Operations connector is used to invoke ‘DataManagementDefinitionGroups/ImportPackage’ action. 
	* This action is invoked to initiate import of package from blob storage by DMF framework
	* One could mention the data project name, whether to execute target, whether to overwrite data as well the company context under which the package should be imported
	* The sample picks up company context by the four-letter prefix in the zip file name. 
e.g. USMF_Customer_001_V101.zip Here USMF denotes the company context. 
This is done so that a generic logic app can be used to orchestrate integration in a company agnostic manner.
6.	The Dynamics 365 for Operations connector is used to invoke ‘DataManagementDefinitionGroups/GetExecutionSummaryStatus’ to get the status of execution
	* This action is invoked with the execution Id received from the previous step to monitor execution status of the data package
7.	If the data package is successful, the zip file is moved to a ‘Success’ folder, else it is moved to a ‘Error’ folder. 

### Outbound flow
1.	Based on the recurrence cadence you defined (in our sample it’s every 30 seconds), the logic app gets triggered
2.	The Dynamics 365 for Operations connector is used to invoke ‘DataManagementDefinitionGroups/ExportToPackage’ action
	* This action is invoked to request Dynamics 365 for Operations to return the export execution Id for the data project and legal entity you supplied
	* This returns execution Id of export
3.	The Dynamics 365 for Operations connections is used to invoke ‘DataManagementDefinitionGroups/GetExecutionSummaryStatus’
	* This action requests Dynamics 365 for Operations to provide export job execution status based on the execution Id you supplied
	* This returns export job execution status (Enum DMFExecutionSummaryStatus)
4.	The Dynamics 365 for Operations connections is used to invoke ‘DataManagementDefinitionGroups/GetExportedPackageUrl’
	* This action requests Dynamics 365 for Operations to provide downloadable blob Url of exported package
	* This returns blob Url of exported package
5.	The standard HTTP connector is used to download content from blob Url
	* It’s a Get with blob Url from GetExportedPackageUrl as input
	* This returns the content of exported package
6.	The standard OneDirve for Business connector is used to move exported package content to destination as a zip file
	* This action takes body of the output of HTTP connector as its input and write it as zip file to defined destination OneDrive for Business folder
