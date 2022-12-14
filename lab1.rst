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

Before we get started, please take note of your namespace, in the event you need any further support throughout this lab. The Distributed Cloud console allows you to manage permissions via namespaces. For example, you can have a namespace for testing and another one for production. Every participant will have a unique namespace for this lab. 

|lab1_namesp|

2. Click **Add Application**, displayed on the center of the Manage Application screen.
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
  * **Redirect???**. Provide the appropriate Status Code and URI
  * **Block**. Provide the Status Code, Content Type, and Response message
  
  Note: Mobile clients only allow Continue and Block. If you select Web and Mobile Client, you can select mitigation actions for each client type.
 
 |lab1_updated13|
  
  Note2: If you select Web and Mobile Client, you will need to enter a Mobile Request Identifier Header, when enabling Mobile SDK Settings later in the configuration.


|lab1_updated14|
  
10. When done configuring the endpoint, click **Apply**.
11. Your protected endpoint is added to the table. The **Actions** column allows you to modify the endpoints. You can add additional new endpoints via the **Add Item** button.

|lab1_updated16|

12. To continue, click **Apply** at the bottom of the page.


Task 4: Define Web Client JavaScript Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Skip ahead to Add the **Web Client JavaScript Path**.  Web client will fetch F5 Client JavaScript from his path. This path must not conflict with any other website/application paths. 

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

It is recommended that you use wildcards to select JS insertions paths for HTML pages (such as /index.htm and /login/*, and other pages that end users are likely to arrive on). 

.. code-block:: text

    Recommended Insertion Path examples:
      /index.htm
      /login/*
     

|lab1-020|

7. Click **Apply**. The **JS Insertion Path** is added to the table. Click **Add Item** to add additional JS Insertion Paths.
8. Once all JS Insertions Paths are added, click **Apply**.
9. You can choose specific web pages to exclude. In the **Exclude Paths** field, click **Add Item**. 

|lab1-021|

.. code-block:: text

    Best Practice: 

    Include examples:
      /login/*
      /catalog
    Exclude examples:
      /login/images
      /catalog/soldout/*
      
It is better to be selective with **JS Insertions** to save money rather than adding a long list of exclusions. A small cost is incurred per inclusion request for AWS lambda to check for exclusions.


10. Specify a **Name**, **Domain Matcher**, and **Path** to exclude. You can choose from Prefix, Path, or Regex for **Path Match**. Click **Apply**. This adds an item to the table. You can add more excluded pages to the table.


|lab1-022|

Task 5: Save Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Before we continue, we are going to save the entire configuration. Navigate to the purple button on the bottom right hand side of the page, named **Save and Exit**. You DO NOT want to click on Cancel and Exit configuration - THIS WILL NOT SAVE YOUR CONFIGURATION. 

|lab1_save_config|

You will see that your application has been added to the table. To edit the configuration click on the '. . .'. One of the actions will be to **Manage Configuration**. 

|lab1-026|

Here you will be able to review your configuration. Next, click Edit configuration on the top right hand corner. 

|lab1_edit|

Task 6: Define Continue Global Mitigation Action
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The **Header Name for Continue Mitigation Action** field is the header that is added to the request when the **Continue** mitigation action is selected and *Add A Header* was selected in the endpoint mitigation configuration screen.

|lab1_updated18|

Task 7:Define Mobile SDK Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. If you added mobile endpoints to your configuration, select Enable Mobile SDK.

|lab1-023|

2. If you selected Web and Mobile as your client type during endpoint configuration, you will need to add a mobile header to distinguish the endpoints. This is not required for Web-Only or Mobile-Only client types. At the top of the configuration, toggle on Advanced Settings.

|lab1_adv|
   
   a. In the Mobile Request Identifier field, Click Add Item.
   
   |lab1-024|
   
   b. Enter a name and the corresponding value.
   
   |lab1_updated25|

Task 8:Trusted Client Rules (Allow List)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Trusted Client Rules add headers and IP addresses to an Allow List. Pages that have a specific IP or that contain specific headers are allowed to proceed to the origin and are not sent to the Bot Defense engine for evaluation. No logging is done on pages that are on the Allow List.

1. Trusted Client Rules is an Advanced Setting. Navigate to the top of the configuration to toggle on Advanced Settings, if you have not already done so. 
2. In the **Trusted Client Rules** field, click **Configure**.

|lab1_updated27|

2. Click **Add Item**.
3. Enter a Name and specify the **Client Identifier**. Choose either *IP Address* or *HTTP Header*.
  * For *IP Prefix*, enter a string.
  * For *Header*, enter a Name and value.
  
  Note: Multiple headers can be added to the table and saved. IP Addresses need to be added individually. 
  
|lab1_updated28|

Task 9: Save Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Your configuration is now complete! 

1. Click **Save & Exit** to save your protected application configuration.

|lab1_updated27|

Task 10: Download Config File and AWS Installer Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In the Actions column of the table, click the 3 dots (???) on your application. Download both the config file and the AWS installer.

|lab1-026|

Summary of next steps shown in the demo video: 

Next you will proceed to your AWS Console to deploy the connector and upload your config file. Once the configuration is installed on AWS, you can navigate to Monitor within the Distributed Cloud Bot Defense tile to view all traffic that the F5 XC Defense Engine has recorded, for valid and invalid requests. 

|lab1_traffic|



Task 10: DEMO
~~~~~~~~~~~~~~~~~~~~


**End of Lab:**  This concludes the Lab, feel free to review and test the configuration.

|labend|


.. |lab1-001| image:: images/lab1-001.png
   :width: 800px
.. |lab1_namesp| image:: images/lab1_namesp.png
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
.. |lab1_updated16| image:: images/lab1_updated16.png
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
.. |lab1_save_config| image:: images/lab1_save_config.png
   :width: 800px
.. |lab1_edit| image:: images/lab1_edit.png
   :width: 800px
.. |lab1-023| image:: images/lab1-023.png
   :width: 800px
.. |lab1-024| image:: images/lab1-024.png
   :width: 800px
.. |lab1_updated25| image:: images/lab1_updated25.png
   :width: 800px
.. |lab1-026| image:: images/lab1-026.png
   :width: 800px
.. |lab1_updated27| image:: images/lab1_updated27.png
   :width: 800px
.. |lab1_updated28| image:: images/lab1_updated28.png
   :width: 800px
.. |lab1_traffic| image:: images/lab1_traffic.png
   :width: 800px
.. |lab1_adv| image:: images/lab1_adv.png
   :width: 800px
.. |labend| image:: images/labend.png
   :width: 800px

