1.	Click the ‘Deploy to Azure’ button corresponding to the logic app that you want to deploy and follow on screen instructions to deploy the templates to your azure subscription. 
These buttons are located in repository home page. https://github.com/Microsoft/Dynamics-AX-Integration

      ![image](https://user-images.githubusercontent.com/22554479/27505678-c4568c9e-585a-11e7-818f-d13181a48854.png)

       Notes:
      * Each template requires a parameter URL to your Dynamics 365 for Operations environment. DO NOT include "https://" in the URL.
      * For OneDrive download location in the D365FO to OneDrive Template, specify the root and path (URL is not required). For example, a folder named MyFiles in the OneDrive root is /MyFiles.
      * An Export Data Project must be created in Data Management and specified when deploying the D365FO to OneDrive Template.
      * An Import Data Project is created automatically when the OneDrive to D365 Logic App is run.

      Example settings for OneDrive to D365FO Logic App:

      ![image](https://user-images.githubusercontent.com/19537546/35405880-60baed0e-01d5-11e8-8ab4-7cbaaba7a4e1.png)

      Example settings for D365FO to OneDrive Logic App:

      ![image](https://user-images.githubusercontent.com/19537546/35406020-c75fcbe2-01d5-11e8-8fd0-051fb7458246.png)

2.	Once deployed, navigate to portal.azure.com and find the deployed app under Logic App resources

      ![image](https://user-images.githubusercontent.com/22554479/27505686-eb3a6542-585a-11e7-9343-d4a931d062bc.png)
                
3.	Select the logic app you deployed and navigate to API connections

      ![image](https://user-images.githubusercontent.com/22554479/27505700-06ba106a-585b-11e7-905d-5a75f1f67253.png)

4.	Select each of the connections, click on the banner ‘This connection is not authenticated’ and follow on screen instructions to authorize the logic app to connect to these resources
 
      ![image](https://user-images.githubusercontent.com/22554479/27505703-14b292a0-585b-11e7-92f7-f13357ea8362.png)

5. Open the Logic App Designer and click Save to refresh the source container API connections.

Notes:
* The OneDrive to D365 Logic App requires a OneDrive folder from the root /LogicAppsIntegration/Demo01. It must contain the following 5 sub-folders (validate the paths in the Logic App Designer and adjust if necessary): 

     ![image](https://user-images.githubusercontent.com/19537546/35404633-a5b14c40-01d1-11e8-92b3-8b6487611434.png)

* The Package File that you import must have the Target Legal Entity ID as the prefix in the file name, followed by underscore. Example: USMF_DemoCustomers.Zip

