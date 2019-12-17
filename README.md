# Biotech Blueprint 2.0 Deployment Instructions

[Image: image.png]
## Create a new Master Account

Ideally, this is a fresh new account. Control Tower will not work if the master account has already been enrolled into an AWS Organizations relationship as either the payer or a linked account.

## Setup the Control Tower Landing Zone

Log into your new ‘master’ account with the IAM user credentials you created for yourself.

Navigate to the AWS Control Tower console and click on the “Set up landing zone” button.
[Image: image.png]
Provide emails for Log Archive Account and the Audit Account. These need to be email addresses distinct from each other and the email address you used for the main account. Something like [log-archive@yourcompany.com](mailto:log-archive@yourcompany.com) and [audit@yourcompany.com](mailto:audit@yourcompany.com). 

Acknowledge the permissions notice before you click the “Setup landing zone” button. 

Get some coffee and set a timer for 60 minutes. It will take up an hour for AWS Control Tower to provision the ‘core’ control tower accounts and setup the other AWS services managed by Control Tower. You can safely close the browser and come back when your timer goes off.  
[Image: image.png]

## Set AWS SSO password.

At some point during the AWS Control Tower setup, an email will be sent to the master account’s root email address. 
[Image: image.png]Click the “Accept Invitation” button and create a password. This username/password will only be used one-time during the setup to create the necessary credentials across the Master, Transit, and Research accounts. We will be changing the AWS SSO directory to Active Directory at the end of this guide, so this username/password will not be used after that.

## Create the Transit Account


Once Control Tower has finished setting up the landing zone, its time to provision the Transit Account. The transit account will eventually house the Client VPN Endpoint and Transit Gateway portion of the solution. All future inter-account or inter-vpc networking can and should be configured through the Transit Gateway created by this account. Ideally, only your network administrator will have access to log into the Transit Account’s AWS Console. 

Navigate to Control Tower in the AWS Console and click on the “Account Factory“ link on the left side, then the ”Provision new account“ button.

[Image: image.png]
That will redirect you to the AWS Service Catalog product list. Click on the “AWS Control Tower Account Factory”.  

[Image: image.png]
Click the large “Launch Product” button.
[Image: image.png]
Provide a name for the Transit account stack, for example “TransitAccount”.
[Image: image.png]On the parameters section of the wizard, provide the following parameters:

|Parameter Name	|Reccomended Value	|Notes	|
|---	|---	|---	|
|SSO User Email	|admin-transit@yourcompany.com	|Replace with your company name	|
|Account Email	|admin-transit@yourcompany.com	|Replace with your company name	|
|SSO User First Name	|Admin	|	|
|SSO User Last Name	|Transit	|	|
|Managed Org Unit	|Custom	|Drop down field. You will only see this option.	|
|Account Name	|Transit	|	|

Proceed through the rest of the wizard just accepting defaults and click the “Launch” button when you get to the Review page of the wizard.
[Image: image.png]
You will want to set another timer. It takes about 25 minutes for the account to be created. 

It will look like this once the account has been created:

[Image: image.png]
You have to wait for this to complete before you proceed. 

## Create the Research Account

The Research account is the future home of your bio/chem informatics tools and pipelines. All of the rest of the accounts we have created so far are designed to provide technical separation of duties according to best practices. For example, the master account handles identity, the transit account handles networking, the audit account handles cloudtrail events for all accounts, and the logging account handles cloudwatch logs for all accounts. 

You will need to follow the same steps you went through for the Transit account to create the Research account. Just use the following parameters instead:


|Parameter Name	|Reccomended Value	|Notes	|
|---	|---	|---	|
|SSO User Email	|admin-research@yourcompany.com	|Replace with your company name	|
|Account Email	|admin-research@yourcompany.com	|Replace with your company name	|
|SSO User First Name	|Admin	|	|
|SSO User Last Name	|Research	|	|
|Managed Org Unit	|Custom	|Drop down field. You will only see this option.	|
|Account Name	|Research	|	|

## Enable sharing within your AWS Organization

Inside your master account, open the AWS Resource Access Manager (RAM) in the AWS console. AWS RAM is a service that makes sharing resources (like the Transit Gateway that gets created later on) between accounts easier and more secure. 

Go to the ‘Settings’ portion of RAM and check the “Enable sharing within your AWS Organization” and click “Save Settings”

[Image: image.png]

## Elevate permissions in the Transit and Research accounts.

By default, accounts provisioned by Control Tower (excluding the Audit and Logging accounts) don’t automatically grant cross-account administrator privileges to any user or principal. We need to grant administrator permissions to the user you setup in the “Set AWS SSO password” section above for the Transit and Research accounts so that we can start deploying resources into them.

### Grant permissions

Go to the AWS SSO service page in the AWS console and go to the “AWS Accounts” tab:
[Image: image.png]Select the check boxes next to the Research and Transit accounts and click “Assign Users” button.
[Image: image.png]
Select the “AWS Control Tower Admin” account and click “Next: Permission sets”
[Image: image.png]Choose “AWS Administrator Access” and then click the “Finish” button. Should only take a few seconds. “Click the Proceed to AWS Accounts button“

### Extend token expiration

The default token expiration for IAM roles assumed by AWS SSO is 1 hour. It may take you longer than an hour to complete the following steps if you take a break along the way. 

To extend that expiration, click on the “Permission Sets” tab and click on the “AWSAdministratorAccess” permission set directly. (Don’t click the radio button)

[Image: image.png]Click the “Edit”button and set the session duration to something greater than an hour. 
[Image: image.png]It will then ask you which accounts you want to reprovision the policy in. Select the Transit, Master, and Research accounts and click “Reprovision”
[Image: image.png]
Should only take a few seconds.

## Prepare the Deployment Environment with AWS Cloud9

AWS Cloud9 is a web based integrated development environment. You could run all of the same commands below on your local machine, but Cloud 9 comes with many dependencies pre-installed and tends to be a much faster to get started. 

Navigate to the AWS Cloud9 console, and click the “Create environment” button.
[Image: image.png]
Follow the creation wizard, but you only need to specify the following parameters. Leave the rest as their defaults. 

Environment Name (Call it something like Biotech Blueprint Deployment Console)
Instance Type: t2.small

It will take a few minutes, but you will eventually be presented with the following development environment:
[Image: image.png]
Notice the terminal window at the bottom of the IDE. Run the following commands. You may get warnings, but they can be ignored. We are installing the AWS Cloud Development Kit (CDK) and pulling down the Biotech Blueprint source code. 

```
git clone https://github.com/paulu-aws/biotechblueprint.git
cd biotechblueprint
./prepCloud9env.sh
```

The final output of the prepCloud9Env script is the path to the AWS credentials file. Click on the file name, and chose “Open” to open up the AWS Credentials File.
[Image: image.png]
## Prepare Your AWS Credentials File

Go ahead and delete the contents of the credentials file. 

You may get a warning when Cloud9 detects you have changed this file. One of the features of Cloud9 is automatically refreshing this file with temporary keys for the IAM user assigned to Cloud9. Because we are deploying things across multiple accounts, we want to disable this automatic refresh. When the warning prompt appears, click the “permanantly disable refresh button”

We are going to be deploying lots of stuff in the Master, Transit, and Research account. We need to prepare this credentials file that will give the Cloud9 environment permissions.

### **Log into your SSO portal.**

From the master account, visit the AWS SSO page in the console again. When the AWS SSO Dashboard comes up, visit the “User portal URL” listed at the bottom of the dashboard and log in with the username/password that you used in the “Set AWS SSO password.” section above.

[Image: image.png]
You should see something like the following once you have logged in.
[Image: image.png]
Expand the "Research" account, and click on the"Command line or programmatic access" button on the "AWSAdministratorAccess" row. 

[Image: image.png]
Hover over the "Option 2" and copy that text. 

[Image: image.png]
That text will look something like this:

`[111111111111_AWSOrganizationsFullAccess]`
`aws_access_key_id = ASIA....P5MC4E`
`aws_secret_access_key = lZfRUkA.....1daLxqcSOx0E`
`aws_session_token = AgoJb3JpZ............0grr1`

Paste that text into the open credentials file in the Cloud9 environment located at `~/.aws/credentials`

Replace the `[111111111111_AWSOrganizationsFullAccess] `with `[research]`

Do the same for the transit and master account such that your aws credentials file ends up looking like this: 


```
[research]
aws_access_key_id = ASIAR7.....GUMM6
aws_secret_access_key = kN4LVgE......oKidZ+3BxMmyyjwXl8PjD9
aws_session_token = Ago............Jc
region=us-east-1

[master]
aws_access_key_id = ASIAXZ...6GRA6U
aws_secret_access_key = pj4EMH......MsXmjdiWRg
aws_session_token = AgoJ............bD
region=us-east-1

[transit]
aws_access_key_id = ASIA3Q...X4LIPYBL
aws_secret_access_key = D+xfSxawa......gnXCiEYAh
aws_session_token = AgoJb3Jpujc............NU7ZQETpGyO2u
region=us-east-1
```

Make sure you add region=us-east-1, or whatever your desired home region is to each profile. 

Save the credentials file when you are done.

## Set your deployment preferences:

Open the file cdk.json at `~/biotechblueprint/cdk.json`

This file is where various context values are set for the CDK CLI deployment. The only value that you NEED to set is `corporateDnsApex`. That value needs to map to your company’s apex domain. If your company name is ExampleCorp and you own the domain examplecorp.com, use that.

The only other value you might want to change is `corporateNetBiosShortName`. This is used with Active Directory as the domain’s short name. If you don’t know what this is or don’t care, just leave the default value of `corp`.

Leave the rest of the values alone. The deployment script will automatically populate them. 

```
{
  "app": "npx ts-node bin/bb-20.ts",
  "requireApproval": "never",
  "context": {
    
    "corporateDnsApex" : "examplecorp.com",
    "corporateNetBiosShortName": "corp",
    
    "transitGatewayRouteTableSecretArn": "XXXXXXXXXXXX",
    "researchTgAttachmentSecretArn": "XXXXXXXXXXXX",
    "transitGatewaySecretArn": "XXXXXXXXXXXX",
    "researchVpcCidrSecretArn": "XXXXXXXXXXXX",
    "identityTgAttachmentSecretArn": "XXXXXXXXXXXX",
    "identityVpcCidrSecretArn": "XXXXXXXXXXXX",
    "identityAccountAdConnectorSecretArn": "XXXXXXXXXXXX",
    "identityAccountAdConnectorSecretKeyArn": "XXXXXXXXXXXX",
    "masterAcctId": "XXXXXXXXXXXX",
    "transitAcctId": "XXXXXXXXXXXXX",
    "researchAcctId": "XXXXXXXXXXXXX",
    "orgArn": "XXXXXXXXXXXXXXX"
  }
}
```

## Run deployBiotechBlueprint.sh

Now you just need to run the following command 

`./deployBiotechBlueprint.sh`

This script will kick off a number of AWS CDK deployment commands that will stand up all of the components in the Transit, Master, and Research accounts. You will want to set another timer. The deployment will take approximately 1 hour. For the record, 40 minutes of that is waiting for Microsoft’s Active Directory to boot up.


## Connect to the VPN 

The previous deployment script sets up Active Directory in the master account, AWS AD Connectors in the Transit and Research accounts, and an Active Directory integrated Client VPN Endpoint in the Transit account. 

In order to manage users and groups, you need to first connect to the VPN Endpoint which will allow you to route into the Identity stack. From there, you can directly connect the Active Directory domain controllers from your local machine or RDP into the “Domain Controller Console” host and administer AD from there. 

### Download Client VPN Configuration File

The deployment script will automatically output the TransitVPN.ovpn file @ `~/environment/TransitVPN.ovpn`. You can right click on that file in Cloud9 to download it. You can also download again from the AWS web console by locating the Client VPN Endpoint in the Transit Account’s VPC console, selecting it, and clicking the “Download Client Configuration” button.

Location of the TransitVPN.ovpn config file in Cloud9:
[Image: image.png]For reference, you can also download the same client configuration file from the AWS VPC console’s "Client VPN Endpoints" page. 
[Image: image.png]
### Download Client VPN Software  and import the CONFIGURATION file

You will first need a Client VPN application installed on your local machine. [These instructions have links to download](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect.html) various compatible VPN Client tools depending on your OS (Windows/Android/iOS/MacOS/Ubuntu). Those instructions also tell you how to import a configuration file. Follow those steps with the TransitVPN.ovpn file you just downloaded on your local machine. 

### Connect to the VPN

Go ahead and connect to the VPN. You will be prompted to login with credentials. 
[Image: image.png]
By default, AWS Directory Services provisions a single username, “admin”, that has full domain controller permissions. The Biotech Blueprint deployment script has automatically given this admin permissions to log into this domain. To obtain the password, log into the Master Account and go to the AWS Secrets Manager service page.
[Image: image.png] Click on the `ADAdminCreds` secret 

Scroll down to the “Secret value” section and click the “Retrieve secret value” button:
[Image: image.png]
You will see the username/password combo that you should use log into the VPN. 
[Image: image.png]
You are now connected to the cloud! Now lets connect to that domain controller and start adding users/groups that are relevant to your company. 


## Create your desired AD Users and Groups

At this point, you can directly connect your local machine to Active Directory and manage it if you happen to have the AD administration tools already installed on your local machine. For simplicity sake, the Biotech Blueprint deployment script launches a small instance in the identity stack’s private subnet that already has the AD management tools installed and is already joined to the domain. All you need to do is RDP into that host. Keep in mind, this host is in a private subnet only routable by clients that are connected to the VPN.

To connect to the host, log into the master account AWS console and visit the EC2 service page. Under “Instances”, you will see the “Domain Controller Console”. Right click on that instance and choose connect. 

Todo:// need to add instructions on adding RDP ingress rule to security group attached to domain controler
[Image: image.png]
Click on the “Download Remote Desktop File” and open it with your remote desktop client:
[Image: image.png]You may get a warning when you connect. Go ahead and click the connect button.

[Image: image.png]
You need to connect with the same AD credentials that you logged into the Client VPN Endpoint with. Your RDP client may default the user to your local machine’s current user. Click the “More choices” link and “Use a different account” option.
[Image: image.png]
Your username needs to be a combination of your NetBIOS shortname and the “admin” username. You would have specified the shortname as the `corporateNetBiosShortName` value in the “Set your deployment preferences” section above. If you left the default value, the username will be: “corp\admin”. 

Your password is the same password you used to log into the VPN as described in the “Connect to the VPN” section above. You can get that same value again from Secrets Manager in the master account if you need to.

Click OK and you will get automatically logged into the Domain Controller Console instance. 

When the instance comes up, click the Windows icon in the bottom left corner and type in “Active Directory Users and Computers” and open the app. 
[Image: image.png]
The snapin will automatically connect to Active Directory as the domain administrator (because you are logged in as the domain admin).

To start creating groups/users, expand the tree down from “yourcompany.com” to “yourshortname” to “Users”. For example:

[Image: image.png]
You will see 4 items in the list:


* Admin - This USER is created automatically by AWS directory services and the user you are currently logged in as.
* svc_adconnector - This USER is a service account used by the AD Connectors in the Transit and Research accounts to connect to the domain controller.
* Connectors - This GROUP gives permissions for the AD connectors in the Transit and Research accounts.
* CorporateVpnUsers - This GROUP gives members permissions to connect to the Transit VPN solution. You will notice Admin is already a member of this group.

At this point, you can start creating groups and individual users as you see fit. 

Start by creating a user for yourself. 

If you want to allow someone else to connect to the VPN, create a user for them and add them to the CorporateVpnUsers group.

In the next step, we are going to give AWS permissions based on the AD groups you create. We recommend that you create the following groups and add the user you created for yourself to them.

ResearchAccountAdmins
ResearchAccountUsers
TransitAccountAdmins
MasterAccountAdmins
AuditAccountAdmins
LoggingAccountAdmins

We will associate these groups to AWS IAM  permissions in the next step.

For the most part, non-IT staff will never need AWS permissions on the Transit/Master/Audit/Logging accounts and should likely never be added to those groups. 


## Switch AWS SSO to use AWS Directory Services

Up to this point, the AWS SSO service that AWS Control Tower setup for you in the master account has been using the AWS SSO Directory. Because we are maintaining users/groups in Active Directory, we are going to change AWS SSO to use AD instead. Log into the master account and navigate to the AWS SSO service page. 

Click on the “Directory” section.

[Image: image.png]
Click on the “Change directory” link.

Choose “Microsoft AD directory”.

The identity stack’s AD environment should be prepopulated in the “Existing directories" drop down. Click Next.

[Image: image.png]
You will get a warning that that all existing permissions will be removed. Type “CONFIRM” to acknowledge this. 

It should only take a few seconds. You should see the following if it completed successfully. 

[Image: image.png]
Click on the “Proceed to the directory” button. Then navigate the “AWS accounts” page in the AWS SSO console.
[Image: image.png]
Its from here that we assign Active Directory users or groups IAM permissions. Lets start off giving access to the master account. 

Check the box next to the master account on the list and click the “Assign users” button. 

You will see a search box come up to search for group names. Search for the group name you created earlier that you want to give master account admin privileges too. If you followed the recommended group names above, it would be “MasterAccountAdmins”. (Make sure you added yourself to that group  as described in the “Create your desired AD users and groups“ section above) Then click ”Next: Permissions sets“.

[Image: image.png]
Now we are about to assign permissions to users of that group on the account we selected. In order to give members of this group full administrator access to the account, we just need to check the “AWSAdministratorAccess” policy and click finish. 
[Image: image.png]
As you can see, there are other types of pre-built permission sets that you can assign to users or create your own. You can also select multiple permissions sets for the group. If a users has multiple permissions sets, they will see an option to login to the account for each permission set the belong to. For example, you may want to give users the option of escalating their privileges or emulating another role’s permissions. 

Repeat this process for the Transit, Research, Audit, and Log Archive accounts based on the groups or users you created in Active Directory. 

## Log into your AWS Portal and conquer

Go back to the AWS SSO Dashboard and copy the “User portal URL”.

This URL is the entry point for for all users who need access to the AWS Console, CLI, or SDKs. 
[Image: image.png]
Go ahead and visit that URL and login with the Active Directory username and password you created for yourself earlier. 
[Image: image.png]Expand the various accounts to access the management console or obtain AWS access keys and secret keys. In the image below, all of the accounts are listed as an illustration. If the logged in user only had access to one account, they would only see one account listed (or get logged in automatically).
[Image: image.png]
## Giving account access to other users

In order to allow others to connect to the VPN or the AWS console you need to provide them with 3 things:

* Active Directory Credentials - Whatever username and password you created for them
* For VPN Access, give them the same TransitVPN.ovpn file that you downloaded earlier and connected with. That file contains no passwords and can be shared safely among members of your company. Each user will be logging in with their own unique AD credentials you created for them.
* For AWS Console access, give them the “User portal URL” from the AWS SSO page that you just logged in with. They will need to log in with their AD credentials. 



