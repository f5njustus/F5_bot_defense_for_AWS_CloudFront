Introduction: Accessing F5 Distributed Cloud Console
====================================================

Welcome to this F5 Distributed Cloud Lab. The following tasks will guide you through the initial 
access requirements for this multi-part lab.  Lab attendees should have received an invitation 
(which requests you update your password for access) email to the lab environment. Please check 
the email address used for course registration and its associated spam folders to see if the
invitation email has been received.  If you have not received an email, please contact a member
of the lab team.
 
The F5 Distributed Cloud Console, where a majority of all lab tasks will be conducted, is a SaaS
based control-plane for services which provides a GUI and API for managing network, security, and
compute services. The F5 Distributed Cloud Console can manage "sites" in existing on-premises,
private data centers and sites within AWS, Azure, and GCP public cloud environments.

Task 1: Lab Environment
~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
The image below represents an overview of the Distributed Cloud Bot Defense connector integration 
methods, where we will be using our cloud edge connector on Amazon CloudFront CDN.                           

|intro_arch| 

Today's lab will focus on how to confiure in the distibuted Cloud Console, followed by a short demo 
demonstrating how to deploy the connector and Config files in your AWS Console. 
                                                                                              
* **F5 Distributed Cloud Console**                                                           
* **Amazon CloudFront/ AWS Console**                                             

 |intro_overview|                                                                                   
+----------------------------------------------------------------------------------------------+

Task 2: F5 Distributed Cloud Console Login
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following will guide you through the initial Lab environment access within the F5 Distributed
Cloud Console.  You should have received an email with an invitation to access a F5 Distributed
Cloud Tenant. The email will come from **no-reply@volterramails.io**.

The name of the F5 Distributed Cloud tenant that we will be using for this lab is **f5-xc-lab-sec**
Additionally, the following are key configuration elements for this lab and will be used
throughout the lab tasks that follow.

* F5 Distributed Cloud Console: https://f5-xc-lab-sec.console.ves.volterra.io/
* Delegated Domain: **lab-sec.f5demos.com**

After following the invitation email's instructions to **Update Password**, proceed to the first
step below to access the F5 Distributed Cloud Lab Tenant. 

+----------------------------------------------------------------------------------------------+
| 1. Please log into the F5 Distributed Cloud Lab Tenant with your user ID (email) & password. |
|                                                                                              |
|    https://f5-xc-lab-sec.console.ves.volterra.io/                                            |
|                                                                                              |
| 2. When you first login, accept the Lab tenant EULA. Click the check box and then click      |
|                                                                                              |
|    **Accept and Agree**.                                                                     |
|                                                                                              |
| 3. Select all work domain roles and click **Next** to see various configuration options.     |
|                                                                                              |
|    Roles can be changed any time later if desired.                                           |
|                                                                                              |
| 4. Click the **Advanced** skill level to expose more menu options and then click **Get**     |
|                                                                                              |
|    **Started** to begin. You can change this setting after logging in as well.               |
|                                                                                              |
| 5. Several **Guidance ToolTips** will appear, you can safely close these as they appear.     |
+----------------------------------------------------------------------------------------------+
| |intro002|                                                                                   |
|                                                                                              |
| |intro003|                                                                                   |
|                                                                                              |
| |intro004|                                                                                   |
|                                                                                              |
| |intro005|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **Beginning of Lab:**  You are now ready to begin the lab, Enjoy! Ask questions as needed.   |
+----------------------------------------------------------------------------------------------+
| |labbgn|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. |intro_arch| image:: images/intro_arch.png
   :width: 800px
.. |intro_overview| image:: images/intro_overview.png
   :width: 800px
.. |intro002| image:: images/intro-002.png
   :width: 800px
.. |intro003| image:: images/intro-003.png
   :width: 800px
.. |intro004| image:: images/intro-004.png
   :width: 800px
.. |intro005| image:: images/intro-005.png
   :width: 800px
.. |labbgn| image:: images/labbgn.png
   :width: 800px
