

AWS CloudFront
Create a new Bot Defense application for AWS CloudFront​

    Go to the Dashboard page of XC console and click Bot Defense.
    Click Add Application at the top-left of the page. If no applications exist, a prompt appears about adding a protected application.
    Add a Name for the Application, and a Description.
    Select a region (US, EMEA, or APJC).
    For Connector Type, select AWS CloudFront. Once AWS CloudFront is selected, options appear to configure AWS reference details.

Add AWS Reference Information​

    Enter your AWS 12-digit account number. F5 gives you account access to the F5 Distributed Cloud (XC) Bot Defense connector on your AWS Serverless App Repository (SAR).
    Specify your AWS Configuration and add your CloudFront distribution; a Distribution ID and/or a Distribution Tag. You can add one or more distributions. This information is needed to associate your newly created protected application to your AWS distribution(s).

Add Protected Endpoints​

    Click Configure to define your protected endpoints. ​
    Click Add Item.
    Enter a name and a description to the specific endpoint.​
    Specify the Domain Matcher. You can choose any domain or specify a specific host value.​
    Specify the path to the endpoint (such as /login).​
    Specify a Query parameter, if needed. If a Query value is defined, the Bot Defense service looks at the Path and Query values.
    Choose the HTTP Methods for which request will be analyzed by Bot Defense. Multiple methods can be selected.
    Select the type of client that will access this endpoint (such as web only).​
    Select the mitigation action to be taken for this endpoint:

    Continue (request continues to origin)​. You can choose to add a header to requests going to the origin for reporting purposes. Header definition is on next screen.
    Redirect​. Provide the appropriate Status Code and URI​
    Block. Provide the Status Code, Content Type, and Response message​.

    When done configuring the endpoint, click Apply.
    Your protected endpoint is added to the table. The Actions column allows you to modify the endpoints. You can add additional new endpoints via the Add Item button.
    To continue, click Apply at the bottom of the page.

Define Continue Global Mitigation Action​

The Header Name for Continue Mitigation Action field is the header that is added to the request when the Continue mitigation action is selected and Add A Header was selected in the endpoint mitigation configuration screen.
Define Web Client JavaScript Settings​

    Add the Web Client JavaScript Path. You should select paths to HTML pages that end users are likely to visit before they browse to any protected endpoint. Web Client JavaScript Settings is relevant only if you have web protected endpoints.
    For the Web Client JavaScript Insertion Settings field, select to Specify the JS Insertion Rules or to Manually insert the JS tags (Advanced Fields needs to be turned on to view this option. Follow instruction in Advanced Features Settings).

    JS Location - Choose the location where to insert the JS in the code:
        Just After <head> tag​.
        Just After </title> tag​.
        Right Before <script> tag.​

    Use JS Insertion Settings to choose which pages to insert the JavaScript into. Click Configure.​
    Click Add Item to define your JavaScript Insertion paths.
    Define a Name and a Path.​ It is recommended that you use wildcards to select JS insertions paths for HTML pages (such as /index.htm and /login/*, and other pages that end users are likely to arrive on).​ Global wildcards (such as /*, and *) are not recommended for Insertion Paths.
    Click Apply. The JS Insertion Path is added to the table. Click Add Item to add additional JS Insertion Paths. ​
    Once all JS Insertions Paths are added, click Apply.
    You can choose specific web pages to exclude. In the Exclude Paths field, click Add Item. It is better to be selective with JS Insertions to save money rather than adding a long list of exclusions. A small cost is incurred per inclusion request for AWS lambda to check for exclusions.

    Include examples​:
        /login/*
        /catalog
    Exclude examples​:
        /login/images​
        /catalog/soldout/*

    Specify a Name, Domain Matcher, and Path to exclude. You can choose from Prefix, Path, or Regex for Path Match. Click Apply. This adds an item to the table. You can add more excluded pages to the table.
    Click Save & Exit to save your protected application configuration.

Download Config File and AWS Installer Tool​

    In the Actions column of the table, click the 3 dots (…) on your application. Download both the config file and the AWS installer.

Advanced Fields​
Manual JS Insertion​

If you require Manual JavaScript Insertion, add the following tags to one of the recommended locations:

    Immediately After <Head>
    Immediately After </title>
    Before <script> (first script tag on the page).​

Matcher Config JavaScript:

<script type='text/javascript' src='INJECTION_PATH?matcher'></script>​

I/O Hook JavaScript​:

<script type='text/javascript' src='INJECTION_PATH?cache'></script>​

Replace INJECTION PATH with the value you specified for Web Client JavaScript Path.​
Trusted Client Rules (Allow List)​

Trusted Client Rules adds headers and IP addresses to an Allow List. Pages with a specific IP or containing specific headers are allowed to proceed to the origin. No logging is done on pages that are on the allow list.

Multiple headers can be added to the table and saved. IP Addresses need to be added individually.

    In the Trusted Client Rules field, click Configure. ​
    Click Add Item.
    Enter a Name and specify the Client Identifier. Choose either IP Address or HTTP Header.

    For IP Prefix, enter a string​.
    For Header, enter a Name and value. ​

Time out and Body Sample Size Limit​

    Timeout – defines the max time to send the requests to the Bot Defense Engine for analysis. If the timeout is exceeded, the request will continue to the origin (this is tracked in AWS CloudWatch). By default, the field is set to 700ms based on performance efficiency.
    Body Sample Size - allows for additional request body data (other than F5 telemetry) to be sent for analysis. By default, this is set to 0 MB. Max size limit is 1MB.

View Traffic​

After your configuration has been added, navigate to Monitor. You can view all traffic that the F5 XC Defense Engine has recorded, for valid and invalid requests.​

This tool can help analyze thousands or millions of requests.
AWS Console

    Login to AWS Console home page.​
    Select AWS Region Northern Virginia (US-EAST-1).
    Use the search to find Serverless Application Repository and click it.
    Click Available Applications.
    Click Private Applications.
    Click the f5ConnectorCloudFront tile.

    If there are too many tiles here, you can search for f5.
    If the F5 connector tile does not appear, validate the AWS Account number provided to F5.

    Click Deploy to install the F5 Connector for CloudFront.

Deploying the F5 Connector creates a new Lambda Application in your AWS Account.​ AWS sets the name of the new Lambda Application to start with serverlessrepo-.​

The deployment can take some time. It is complete when you see the f5ConnectorCloudFront of type Lambda Function.​

You can click on the name f5ConnectorCloudFront to review contents of the installed Lambda Function.​

Configuration of the F5 Connector in AWS is best done via the F5 CLI tool. It is recommended to use the AWS CloudShell.

    After starting AWS CloudShell, click Actions and Upload file.
    Upload the files you downloaded from the F5 XC Console, config.json and *f5tool.
    Run bash f5tool –install config.json. Installation can take up to 5 minutes.

The installation tool saves the previous configuration of each CloudFront Distribution in a file. You can use the F5 tool to restore a saved Distribution config (thus removing F5 Bot Defense).​

Note: Your F5 XC Bot Defense configuration, such as protected endpoints, is sensitive security info and is stored in AWS Secrets Manager. You should delete config.json after CLI installation.
AWS CloudWatch

AWS CloudWatch contains logs for Lambda function deployed by f5ConnectorCloudFront serverless application.​

​The Log group name starts with /aws/lambda/us-east-1.serverlessrepo-f5ConnectorCl-f5ConnectorCloudFront-.​

The logs of lambda function can be found in the region closest to the location where the function executed.​

For troubleshooting, look for error messages contained in the links under Log steams.
References

    Firewall or Proxy Reference for Distributed Cloud
    System Overview
    Load Balancing and Proxy
