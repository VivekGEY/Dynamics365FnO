1.	Click the ‘Deploy to Azure’ button corresponding to the logic app that you want to deploy and follow on screen instructions to deploy the templates to your azure subscription. 
These buttons are located in repository home page. https://github.com/Microsoft/Dynamics-AX-Integration

      ![image](https://user-images.githubusercontent.com/22554479/27505678-c4568c9e-585a-11e7-818f-d13181a48854.png)

       Notes:
      * Each template requires a parameter URL to your Dynamics 365 for Operations environment. DO NOT include "https://" in the URL.
      * For OneDrive location, specify the root and path (URL is not required). For example, a folder named MyFiles in the OneDrive root is /MyFiles.
      * An Export Data Project must be created in Data Management and specified when deploying the D365 to OneDrive Template.
      * An Import Data Project is created automatically when the OneDrive to D365 Logic App is run.

2.	Once deployed, navigate to portal.azure.com and find the deployed app under Logic App resources

      ![image](https://user-images.githubusercontent.com/22554479/27505686-eb3a6542-585a-11e7-9343-d4a931d062bc.png)
                
3.	Select the logic app you deployed and navigate to API connections

      ![image](https://user-images.githubusercontent.com/22554479/27505700-06ba106a-585b-11e7-905d-5a75f1f67253.png)

4.	Select each of the connections, click on the banner ‘This connection is not authenticated’ and follow on screen instructions to authorize the logic app to connect to these resources
 
      ![image](https://user-images.githubusercontent.com/22554479/27505703-14b292a0-585b-11e7-92f7-f13357ea8362.png)

5. Open the Logic App Designer and click Save to refresh the source container API connections.

Notes:
* The OneDrive to D365 Logic App requires a OneDrive folder containing the following 5 sub-folders named as follows: Error, Exception, Input, Processing, Success. Validate these folders in the Logic App Designer and adjust if necessary.
* The Package File that you import must have the Target Legal Entity as part of the package file name prefix, followed by underscore. Example: USMF_DemoCustomers.Zip

