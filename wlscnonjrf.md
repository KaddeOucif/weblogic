# WebLogic Cloud - non JRF

## with provided cloud environment



This Hands on Lab will go through the process of creating a non JRF type of WebLogic Cloud Instance - using Oracle Cloud Marketplace - and the steps of deploying a simple application. A provided (lab) cloud environment will be used.



### Step 1. Create WLS Cloud Stack

- Go to https://console.eu-frankfurt-1.oraclecloud.com/ and login to the lab cloud environment
- Choose **oractdemeabdmnative** as Cloud Tenant:

![](images/wlscnonjrfwithenv/image010.png)



- Choose Single Sign On:

![](images/wlscnonjrfwithenv/image020.png)



- Login with *User Name* and *Password*; Use the credentials from the Self Registration

![](images/wlscnonjrfwithenv/image030.png)



- After logging in, go to Hamburger Menu, *Solutions and Platform* -> *Marketplace*

![](images/wlscnonjrfwithenv/image040.png)



- You can choose to browser-search for *WebLogic Cloud*, or you can apply the filters:
  - **Type**: *Stack*
  - **Publisher**: *Oracle*
  - **Category**: *Application Development*

![](images/wlscnonjrfwithenv/image050.png)



- Choose *WebLogic Cloud Enterprise Edition BYOL*; This brings you to the Stack Overview page:

![](images/wlscnonjrfwithenv/image060.png)



- Choose *CTDOKE* compartment:

![](images/wlscnonjrfwithenv/image070.png)



- And accept the terms for launching the Stack:

![](images/wlscnonjrfwithenv/image080.png)



- Fill in Stack information:
  - **Name**: *WLSCNN* - where NN is your unique suffix given by the instructor
  - **Description**: Any meaningful description, maybe type in your name/initials
- Click **Next**

![](images/wlscnonjrfwithenv/image090.png)



- Start to fill in details:

  - **Resource Name Prefix**: *WLSCNN* - where **NN** your unique suffix
  - **WebLogic Server Shape**: *VM.Standard2.1*
  - **SSH Public Key**: fill in pre-generated key:

  > ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAjB69WYDCNddsCkPf0qxQAQAjRDYRTZtkCB676RseTeNU17hb4QzZkRlE6vBggN+23bn2IzcshZw3Hj0RSZZiluYAUGXHV1s2o2n4WiGfhEjj1yE7xNEIzrT3RjB1Pk8cBzXwN35aqrnEF+0UmHAWMdOl64W/BAvn14dgHNKQFCoTkanY28znazq8Y65XchoHIJG

![](images/wlscnonjrfwithenv/image100.png)



- Continue setting up:

  - **WebLogic Server Availability Domain**: choose one of the three ADs
  - **WebLogic Server Node count**: *2* (we will create a WebLogic cluster with two node)
  - **Admin user name**: *weblogic*
  - **Admin password**: here we have pre-encrypted the *Weblogic1#* password using a Virtual Vault key; you can copy & paste the pre-encrypted password:

  > IdDFz1qKLvhTAAP0OHpg3uGl1wpknYGNmQ0cmsJbVfVgPXzoyaqCKvm5nOAebXnpAAAAAA==

![](images/wlscnonjrfwithenv/image110.png)



- Don't change WebLogic Server Advanced configuration, choose the same *CTDOKE* Compartment and *Use existing VCN* for Virtual Cloud Network Strategy: 

![](images/wlscnonjrfwithenv/image120.png)



- We have pre-configured part of the networking resources; choose for:
  - **Existing network**: *WLSCB2-WLSCloudVCN*
  - **Subnet Strategy**: *Use Existing Subnet*
  - **Subnet Type**: *Use Public Subnet*
  - **Subnet Span**: *Regional Subnet*



![](images/wlscnonjrfwithenv/image130.png)



- End network configuration with:
  - E**xisting Subnet for WebLogic Server**: *WLSCB2-wls-subnet*
  - Tick to **Provision Load Balancer**
  - **Existing Subnet For Load Balancer**: *WLSCB2-lb-subnet-1*
  - **Load Balancer Shape**: *400Mbps*
  - Tick to **Prepare Load Balancer for Https**



![](images/wlscnonjrfwithenv/image140.png)



- Leave Identity Cloud Service Integration as default (no integration) and *No Database* for **Database Strategy**

![](images/wlscnonjrfwithenv/image150.png)



- Lastly, configure Key Management Service; previously we have provided a pre-encrypted admin password for WebLogic admin user; for the Stack scripts being able to decrypt it on the fly (without storing anywhere the password in plain text), it needs the Vault Key Id used for encryption and the Key Management Service Cryptographic Endpoint:

  - **Key Management Service Key Id**: use existing:

  > ocid1.key.oc1.eu-frankfurt-1.bfoz5szoaafak.abtheljsw5qx4fxv3kzeqx6ih6obdtwi7434qbgeom5h3vzmyejtgakgswha

  - **Key Management Service Cryptographic Endpoint**: https://bfoz5szoaafak-crypto.kms.eu-frankfurt-1.oraclecloud.com

- Click **Next**

![](images/wlscnonjrfwithenv/image160.png)



- Review the Stack configuration and Click **Create**:

![](images/wlscnonjrfwithenv/image170.png)



- A Stack Job is being created and our WLS Server is being provisioned:

![](images/wlscnonjrfwithenv/image180.png)



- While all resources being created we can check the Job Logs; it helps fixing potentially configuration errors if the provisioning fails:

![](images/wlscnonjrfwithenv/image190.png)



- After a while, the Job should end with success:

![](images/wlscnonjrfwithenv/image200.png)



- We can check the *Outputs* section of Job Resources and we see two important value:
  - Sample Application URL (we can can try it at this moment, but the out of the box sample application won't load as we need to finish the SSL configuration of the Load Balance)
  - WebLogic Server Administration Console

![](images/wlscnonjrfwithenv/image210.png)



- Let's check the WLS admin console of the newly created WebLogic Server; as we have have chosen a Public Subnet for the WLS network, both Compute instances create have associated public IPs
- Instead of *http://< public ip >:7001/console*, open *https://< public ip >:7002/console*; we'll prevent sending the WebLogic admin user password plain text, insecurely:

![](images/wlscnonjrfwithenv/image220.png)



- Login with **weblogic**/**Weblogic1#**; we can see that our domain has one admin server and two managed servers:

![](images/wlscnonjrfwithenv/image230.png)



- We can check the Compute Instances to see what has been provisioned; From general hamburger menu choose *Core Infrastructure* -> *Compute* -> *Instances*:

![](images/wlscnonjrfwithenv/image240.png)



- We can see two instances having our prefix mentioned during Stack configuration; one of them has the admin server and a managed server running and the other the second managed server:

![](images/wlscnonjrfwithenv/image250.png)



- Congratulations! Your WLS domain is up&running! 



### Step 2. Configure SSL Load Balancer

- We need to do some additional configurations to the created Load Balancer in order to support SSL traffic; From the main menu, go to *Core Infrastructure* -> *Networking* -> *Load Balancers*:

![](images/wlscnonjrfwithenv/image260.png)



- Identify the Load Balancer that has your unique *NN* prefix:

![](images/wlscnonjrfwithenv/image270.png)



- Click the Load Balancer Link and go to Resources sub menu; choose *Hostnames*:

![](images/wlscnonjrfwithenv/image280.png)



- Let's create a new hostname:

![](images/wlscnonjrfwithenv/image290.png)



- Fill in:
  - **Name**: *wlschostname*
  - **Hostname**: *weblogiccloud*

![](images/wlscnonjrfwithenv/image300.png)



- Shortly, the hostname will be created:

![](images/wlscnonjrfwithenv/image310.png)



Now, choose *Certificates* from *Resources* sub menu:

![](images/wlscnonjrfwithenv/image320.png)



- Here, we'll add a Load Balancer certificate that it's required for our SSL listener; configure as following:
  - **Certificate Name**: *weblogiccloud-cert*
  - **SSL Certificate File**: upload provided [weblogiccloud_cert.pem](resources/weblogiccloud_cert.pem) file
  - Tick **Specify CA Certificate**:

![](images/wlscnonjrfwithenv/image330.png)



- As we use a demo self signed certificate, upload the same weblogiccloud_cert.pem file for *CA Certificate File*;
- For **Private Key File**, upload the [weblogiccloud_dec_key.pem](resources/weblogiccloud_dec_key.pem) file
- Enter *Weblogic1#* for **Private Key Passphrase**
- Click **Add Certificate**:

![](images/wlscnonjrfwithenv/image340.png)



- Shortly, the SSL certificate is available:

![](images/wlscnonjrfwithenv/image350.png)



- Finally, we need to configure the SSL listener; From the *Resources* sub menu, go to *Listeners* and edit existing listener:

![](images/wlscnonjrfwithenv/image360.png)



- Configure as following and Save Changes:
  - **Name**: choose existing *wlschostname* hostname
  - Tick **USE SSL** checkbox
  - **Certificate Name**: choose *weblogiccloud-cert*
  - leave default backend set

![](images/wlscnonjrfwithenv/image370.png)



- Shortly, the listener should get updated:

![](images/wlscnonjrfwithenv/image380.png)



- We can check now if the out of the box deployed application is loading; From the Stack Job Outputs, open the Sample Application URL; now it's loading, but we have to bypass the browser warning as we're using a Self Signed Certificate;
- Click **Advanced**, **Proceed to ...** to continue:



![](images/wlscnonjrfwithenv/image390.png)

- The out of the box deployed sample application is being served through a secured SSL Load Balancer Listener:

![](images/wlscnonjrfwithenv/image400.png)



### Step 3. Deploy custom sample application

- Let's go back to the WebLogic Server admin console to deploy our sample web application:

![](images/wlscnonjrfwithenv/image410.png)



- Go to *Domain Structure* menu, *Deployments*; **Lock & Edit** to switch to edit mode; Click **Install**:

![](images/wlscnonjrfwithenv/image420.png)



Follow **Upload your files** link and upload provided [SampleWebApp.war](resources/SampleWebApp.war) web archive file:

![](images/wlscnonjrfwithenv/image430.png)



- Click **Next**, **Next**, leave *Install de deployment as an application* default option; click **Next**:

![](images/wlscnonjrfwithenv/image440.png)



- Choose deploying the application on WebLogic Cluster; click **Next**:

![](images/wlscnonjrfwithenv/image450.png)



- Leave default setting and click **Next**:

![](images/wlscnonjrfwithenv/image460.png)



- Choose *No, I will review the configuration later* and click **Finish**

![](images/wlscnonjrfwithenv/image470.png)



- **Activate Changes**:

![](images/wlscnonjrfwithenv/image480.png)



- The application is now in *Prepared* state; switch to *Control* tab:

![](images/wlscnonjrfwithenv/image490.png)



- Select the *SampleWebApp* web application and click **Start** -> **Serving all requests**; Click **Next** in the following screen:

![](images/wlscnonjrfwithenv/image500.png)



- The *SampleWebApp* web application is in the *Active* State now:

![](images/wlscnonjrfwithenv/image510.png)



- Now, test this new application at *https://< public lb ip >/SampleWebApp/*

 ![](images/wlscnonjrfwithenv/image520.png)

- Click on the link to test this sample application:

![](images/wlscnonjrfwithenv/image530.png)



- This is just another sample application, but you can deploy any other application; Congratulations!