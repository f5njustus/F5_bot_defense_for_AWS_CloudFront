Lab 1: Building the F5 Distributed Cloud Bot Defense for Amazon CloudFront CDN
=========================================================================================

Lab 1 will focus on the deployment and configuration of the F5 Distributed Cloud Bot Defense for Amazon CloudFront CDN.
These steps will leverage the F5 Distributed Cloud console and resources delivered via Amazon CloudFront CDN. You will begin
this lab from the F5 Dsitributed Cloud main services dashboard.

|lab1-001|

Task 1: Create a new Bot Defense application for AWS CloudFront
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Go to the Dashboard page of XC console and click **Bot Defense**.

|lab1-002|

2. Click **Add Application** at the top-left of the page. If no applications exist, a prompt appears about adding a protected application.

|lab1-003|

3. Add a **Name** for the Application, and a **Description**.
4. Select a **Region** (US, EMEA, or APJC).
5. For **Connector Type**, select *AWS CloudFront*. Once *Amazon CloudFront* is selected, options appear to configure Amazon reference details.

|lab1-004|

Task 2: Add AWS Reference Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Enter your AWS 12-digit account number. F5 gives you account access to the F5 Distributed Cloud (XC) Bot Defense connector on your AWS Serverless App Repository (SAR).

|lab1-005|

2. Specify your Amazon Configuration and add your CloudFront distribution via a **Distribution ID** or a **Distribution Tag**. You can add one or more distributions. This information is needed to associate your newly created protected application to your AWS distribution(s).

|lab1-007|

Task 3: Add Protected Endpoints
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Click **Configure** to define your protected endpoints.
  
|lab1_updated8|

2. Click **Add Item**.

|lab1-009|

3. Enter a name and a description to the specific endpoint.
4. Specify the **Domain Matcher**. You can choose any domain or specify a specific host value.
5. Specify the path to the endpoint (such as /login).
6. Specify a **Query** parameter, if needed. If a **Query** value is defined, the Bot Defense service looks at the **Path** and **Query** values.
7. Choose the **HTTP Methods** for which request will be analyzed by Bot Defense. Multiple methods can be selected.

|lab1-010|

8. Select the type of client that will access this endpoint (Web Client, Mobile Client, or Web and Mobile Client).

|lab1-011|



|lab1_updated12|

9. Select the mitigation action to be taken for this endpoint:
  * **Continue** (request continues to origin). You can choose to add a header to requests going to the origin for reporting purposes. Header definition is on next screen.
  * **Redirect​**. Provide the appropriate Status Code and URI
  * **Block**. Provide the Status Code, Content Type, and Response message
  
  Note: Mobile clients only allow Continue and Block. If you select Web and Mobile Client, you can select mitigation actions for each client type.
 
 |lab1_updated13|
  
  Note2: If you select Web and Mobile Client, you will need to enter a Mobile Request Identifier Header, when enabling Mobile SDK Settings later in the configuration.


|lab1_updated14|
  
10. When done configuring the endpoint, click **Apply**.
11. Your protected endpoint is added to the table. The **Actions** column allows you to modify the endpoints. You can add additional new endpoints via the **Add Item** button.

|lab1-016|

12. To continue, click **Apply** at the bottom of the page.



Task 4: Define Continue Global Mitigation Action
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The **Header Name for Continue Mitigation Action** field is the header that is added to the request when the **Continue** mitigation action is selected and *Add A Header* was selected in the endpoint mitigation configuration screen.

|lab1_updated18|

Task 5: Define Web Client JavaScript Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Add the **Web Client JavaScript Path**.  Web client will fetch F5 Client JavaScript from his path. This path must not conflict with any other website/application paths. 

Important Note:**Web Client JavaScript Settings** is relevant only if you have web protected endpoints. If you are only protecting mobile app endpoints, do not enter insertion or exclusion paths. Skip to Mobile SDK settings. 

|lab1_updated18|

2. For the **Web Client JavaScript Insertion Settings** field, select to Specify the JS Insertion Rules or to Manually insert the JS tags (Advanced Fields needs to be turned on to view Manual Insert option).
3. **JS Location** - Choose the location where to insert the JS in the code:
  * Just After <head> tag.
  * Just After </title> tag.
  * Right Before <script> tag.
4. Use **JS Insertion Settings** to choose which pages to insert the JavaScript into. Click **Configure**.
5. Click **Add Item** to define your **JavaScript Insertion** paths.

|lab1-019|

Note: You should select paths to HTML pages that end users are likely to visit before they browse to any protected endpoint. 

6. Define a **Name** and a **Path**. 
Note: It is recommended that you use wildcards to select JS insertions paths for HTML pages (such as /index.htm and /login/\*, and other pages that end users are likely to arrive on). Global wildcards (such as /\*, and \*) are **not recommended** for Insertion Paths.

|lab1-020|

7. Click **Apply**. The **JS Insertion Path** is added to the table. Click **Add Item** to add additional JS Insertion Paths.
8. Once all JS Insertions Paths are added, click **Apply**.
9. You can choose specific web pages to exclude. In the **Exclude Paths** field, click **Add Item**. It is better to be selective with **JS Insertions** to save money rather than adding a long list of exclusions. A small cost is incurred per inclusion request for AWS lambda to check for exclusions.

.. code-block:: text

    Include examples:
      /login/*
      /catalog
    Exclude examples:
      /login/images
      /catalog/soldout/*

10. Specify a **Name**, **Domain Matcher**, and **Path** to exclude. You can choose from Prefix, Path, or Regex for **Path Match**. Click **Apply**. This adds an item to the table. You can add more excluded pages to the table.

|lab1-021|
|lab1-022|

Task 6:Define Mobile SDK Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. If you added mobile endpoints to your configuration, select Enable Mobile SDK.
2. If you selected Web and Mobile as your client type during endpoint configuration, add a mobile header to distinguish the endpoints. This is not required for Web-Only or Mobile-Only client types.
   a. At the top of the configuration, enable Advanced.
   b. In the Mobile Request Identifier field, Click Add Item.
   c. Enter a name and the corresponding value.
  
11. Click **Save & Exit** to save your protected application configuration.

Task 7: Download Config File and AWS Installer Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In the Actions column of the table, click the 3 dots (…) on your application. Download both the config file and the AWS installer.

|lab1-026|


Task 8: Advanced Fields:Trusted Client Rules (Allow List)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Trusted Client Rules adds headers and IP addresses to an Allow List. Pages with a specific IP or containing specific headers are allowed to proceed to the origin. No logging is done on pages that are on the allow list.
Multiple headers can be added to the table and saved. IP Addresses need to be added individually.  

1. In the **Trusted Client Rules** field, click **Configure**.
2. Click **Add Item**.
3. Enter a Name and specify the **Client Identifier**. Choose either *IP Address* or *HTTP Header*.
  * For *IP Prefix*, enter a string.
  * For *Header*, enter a Name and value.

Task 9: Advanced Fields:Time out and Body Sample Size Limit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  * **Timeout** - defines the max time to send the requests to the Bot Defense Engine for analysis. If the timeout is exceeded, the request will continue to the origin (this is tracked in AWS CloudWatch). By default, the field is set to 700ms based on performance efficiency.
  * **Body Sample Size** - allows for additional request body data (other than F5 telemetry) to be sent for analysis. By default, this is set to 0 MB. Max size limit is 1MB.


Task 10: AWS Console
~~~~~~~~~~~~~~~~~~~~

1. Login to AWS Console home page.
2. Select AWS Region Northern Virginia (US-EAST-1).
3. Use the search to find Serverless Application Repository and click it.
4. Click Available Applications.
5. Click Private Applications.
6. Click the f5ConnectorCloudFront tile.
  * If there are too many tiles here, you can search for f5.
  * If the F5 connector tile does not appear, validate the AWS Account number provided to F5.
7. Click Deploy to install the F5 Connector for CloudFront.

Deploying the F5 Connector creates a new Lambda Application in your AWS Account. AWS sets the name of the new Lambda Application to start with *serverlessrepo-*.

The deployment can take some time. It is complete when you see the **f5ConnectorCloudFront** of type **Lambda Function**.

You can click on the name **f5ConnectorCloudFront** to review contents of the installed Lambda Function.

Configuration of the F5 Connector in AWS is best done via the F5 CLI tool. It is recommended to use the AWS CloudShell.
1. After starting AWS CloudShell, click **Actions** and **Upload file**.
2. Upload the files you downloaded from the F5 XC Console, **config.json** and \*f5tool.
3. Run bash *f5tool -install config.json*. Installation can take up to 5 minutes.

The installation tool saves the previous configuration of each CloudFront Distribution in a file. You can use the F5 tool to restore a saved Distribution config (thus removing F5 Bot Defense).

.. note:: 
   Your F5 XC Bot Defense configuration, such as protected endpoints, is sensitive security info and is stored in AWS Secrets Manager. You should delete config.json after CLI installation.

Task 11: AWS CloudWatch
~~~~~~~~~~~~~~~~~~~~~~~

AWS CloudWatch contains logs for Lambda function deployed by **f5ConnectorCloudFront** serverless application.
The Log group name starts with */aws/lambda/us-east-1.serverlessrepo-f5ConnectorCl-f5ConnectorCloudFront-*.
The logs of lambda function can be found in the region closest to the location where the function executed.

For troubleshooting, look for error messages contained in the links under Log steams.

Task 10: View Traffic
~~~~~~~~~~~~~~~~~~~~~

After your configuration has been added, navigate to **Monitor**. You can view all traffic that the F5 XC Defense Engine has recorded, for valid and invalid requests.
This tool can help analyze thousands or millions of requests.

**End of Lab:**  This concludes the Lab, feel free to review and test the configuration.

|labend|

.. |lab1-001| image:: images/lab1-001.png
   :width: 800px
.. |lab1-002| image:: images/lab1-002.png
   :width: 800px
.. |lab1-003| image:: images/lab1-003.png
   :width: 800px
.. |lab1-004| image:: images/lab1-004.png
   :width: 800px
.. |lab1-005| image:: images/lab1-005.png
   :width: 800px
.. |lab1-006| image:: images/lab1-006.png
   :width: 800px
.. |lab1-007| image:: images/lab1-007.png
   :width: 800px
.. |lab1_updated8| image:: images/lab1_updated8.png
   :width: 800px
.. |lab1-009| image:: images/lab1-009.png
   :width: 800px
.. |lab1-010| image:: images/lab1-010.png
   :width: 800px
.. |lab1-011| image:: images/lab1-011.png
   :width: 800px
.. |lab1_updated12| image:: images/lab1_updated12.png
   :width: 800px
.. |lab1_updated13| image:: images/lab1_updated13.png
   :width: 800px
.. |lab1_updated14| image:: images/lab1_updated14.png
   :width: 800px
.. |lab1-015| image:: images/lab1-015.png
   :width: 800px
.. |lab1-016| image:: images/lab1-016.png
   :width: 800px
.. |lab1-017| image:: images/lab1-017.png
   :width: 800px
.. |lab1_updated18| image:: images/lab1_updated18.png
   :width: 800px
.. |lab1-019| image:: images/lab1-019.png
   :width: 800px
.. |lab1-020| image:: images/lab1-020.png
   :width: 800px
.. |lab1-021| image:: images/lab1-021.png
   :width: 800px
.. |lab1-022| image:: images/lab1-022.png
   :width: 800px
.. |lab1-023| image:: images/lab1-023.png
   :width: 800px
.. |lab1-024| image:: images/lab1-024.png
   :width: 800px
.. |lab1-025| image:: images/lab1-025.png
   :width: 800px
.. |lab1-026| image:: images/lab1-026.png
   :width: 800px
.. |labend| image:: images/labend.png
   :width: 800px

