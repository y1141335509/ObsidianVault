ref: 
* Integrate Restlet API of Netsuite in Postman - [link](https://theonetechnologies.com/blog/post/how-to-integrate-restlet-api-of-netsuite-in-postman#stage-1-pre-requisites)
* Oracle Netsuite - [Importing and setting up a Postman Environment](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_1545058149.html#Related-Topics)
* Oracle Netsuite - [Customer Deposit](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_0830072110.html#subsect_48073900112)
## Stage 1 - Pre-requisites

Need to enable some features in NetSuite to set up Restlet APIs (Application Programming Interfaces).

**Step 1 -** First go to set up > company > enable features

![setup](https://theonetechnologies.com/Posts/files/Step-1.1-setup_638040892936471919.webp)

**Step 2 -** In the enable features page click on the “SuiteCloud” subtab.

![suitecloud](https://theonetechnologies.com/Posts/files/step1.2-suitecloud_638040892943332200.webp)

**Step 3 -** Scroll down and enable the rest web service features under the “SuiteTalk (Web Services)” and outh 2.0 in the “manage authentication”.

![manage authentication](https://theonetechnologies.com/Posts/files/step1.3-manage-authentication_638040892953819165.webp)

## Stage 2 - Setup NetSuite integration

In this stage, we will collect the Consumer/Client and Token ID and Secret that we will use in the later stages of the process.

**Step 1 -** To generate an integration record or Consumer/Client ID and Secret, go to Setup > Integration > Manage Integration > New

![setup integration](https://theonetechnologies.com/Posts/files/Step-2.1-setup-integration_638040892955777133.webp)

**Step 2 -** In the integration page fill in the data like Name, Stage, Description, etc. Also, take care of the points below:

- If you do not need the “**token-based authentication”**, then uncheck that option.

- Give the “redirect URL” if you are using the local system.

Once you are done with the details, click on the “**save”** button.

![fill details](https://theonetechnologies.com/Posts/files/step-2.2-fill-details_638040892956533794.webp)

**Step 3 -** Once you save the new integration, make sure you note down the “**Consumer key/client ID and Secret**” under the “Client Credentials” section.

![client credential](https://theonetechnologies.com/Posts/files/step-2.3-client-credential_638040892957301047.webp)

**Note: This credential will only appear when you create it for the first time.**

**Step 4 -** To generate Token ID and Secret, go to Setup > Users/Roles > Access Tokens > New

![user roles](https://theonetechnologies.com/Posts/files/step2.4-user-roles_638040892958165781.webp)

**Step 5 -** In the access token page, fill in the data like application name, user, role, and token name. And click on the “save” button.

![access token](https://theonetechnologies.com/Posts/files/step2.5-access-token_638040892959716471.webp)

**Step 6** - Once you save the details, make sure you note down the “**Token ID and Secret**” under the “Token Id/Secret” section.

![confirmation](https://theonetechnologies.com/Posts/files/step2.6-confirmation_638040892960970729.webp)

**Note: This credential will only appear when you create it for the first time.**

## Stage 3 - NetSuite and Postman Integration

Before we go further, we need to download the Postman environment template and collection archive from the Suite Cloud tools download.

**Step 1 -** Login to your NetSuite account.

**Step 2 -** Then we need to download Postman environment template and collection archive. To do this use the below mention URL with your NetSuite “Account ID” 
``https://<accountID>.app.netsuite.com/app/external/integration/integrationDownloadPage.nl``

**Note –** Change the `accountID` with your NetSuite account id. For example: `https:// TSTDRV0000001.app.netsuite.com/app/external/integration/integrationDownloadPage.nl`

**Step 3** -  You will see this page. Download the NetSuiteRestApiSampleRequest.zip file. Unzip that file.
![[Screenshot 2024-02-01 at 12.04.44.png]]

**Step 4 -** Open the Postman and click on the “gear” symbol. Select the “Settings” option.

![netsuite settings](https://theonetechnologies.com/Posts/files/step3.4-netsuite-settings_638040928174782025.webp)

**Step 5 -** This pop-up will come up.
![netsuite setting](https://theonetechnologies.com/Posts/files/step3.5-netsuite-settings_638040928175929153.webp)

**Step 6 -** Go to the “data” subtab and click on the “choose file” button under Import Data
![netsuite choose files](https://theonetechnologies.com/Posts/files/step3.6-netsuite-choose-files_638040928177173312.webp)

**Step 7 -** Go to the “requests” folder form the download file (NetSuiteRestApiSampleRequest).
![netsuite request](https://theonetechnologies.com/Posts/files/step3.7-netsuite-request_638040928178921457.webp)

**Step 8 -** Select the file which is listed within the “requests” folder.
![netsuite tutorial platform](https://theonetechnologies.com/Posts/files/step3.8-netsuite-tutorial-platform_638040928179990196.webp)




**Step 9** - 你可以自己创建一个环境。然后使用如下的变量。你可以在Postman右上角 + 号的地方创建新环境。
![[Screenshot 2024-02-01 at 12.10.25.png]]
然后参考 `6 Filtering`这个文件夹下面的6.1案例，创建6.8案例如下：
![[Screenshot 2024-02-01 at 12.43.32.png]]
如此一来就可以获得所有的Customer Deposits了（当然每次API请求能获取的数据量有限，可能是1000条）



**Step 9 -** 这是晚上流传的另一种方法。Then select the “NetSuite REST Environment” in Postman by clicking the dropdown. 
![netsuite restapi environment template](https://theonetechnologies.com/Posts/files/step3.9-netsuite-restapi-environment-template_638040928181313460.webp)

**Step 10 -** Now click on the “environment quick look” icon next to the environment dropdown.
![step3.10-netsuite-test-request.webp](https://theonetechnologies.com/Posts/files/step3.10-netsuite-test-request_638040928182141800.webp)

**Step 11 -** To set the NetSuite credentials in the environment file, click the “Edit” option in the pop-up window.

![netsuite edit](https://theonetechnologies.com/Posts/files/step3.11-netsuite-edit_638040928183881243.webp)

**Step 12 -** Need to set the credentials like Account ID, Consumer key, Consumer Secret, Token ID, Token Secret (**refer Stage 2**), Company URL, and Restlet Service URL (**steps mentioned below**) to process further.

![netsuite company setup](https://theonetechnologies.com/Posts/files/step3.12-netsuite-company-setup_638040928185486210.webp)

**Step 13 -** To get the company URL and Restlet service URL, go to setup > company > company information, in NetSuite.

![suite talk](https://theonetechnologies.com/Posts/files/step3.13-suitetalk_638040928186272188.webp)

**Step 14 -** In the company information page, go to the company URL subtab. Copy the URL of Suite talk and RestLet and paste the Suite talk URL in the company URL and restlet in the restlet service URL.






































