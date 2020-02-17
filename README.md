<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/a97a2tUIs8ZnsloZV9h1xQ?a=76Z38CfplUVzTdS0U8cbWJ4t9falQnQPdrGdchXXOoca' id='VHY9CAgfWbG' alt='' width='800' height='495'></img>
</div>

<h1 id='VHY9CA4fYcj'>Biotech Blueprint 2.0 Deployment Instructions</h1>

<h2 id='VHY9CAhrS7E'>Create a new Master Account</h2>
Ideally, this is a fresh new account. Control Tower will not work if the master account has already been enrolled into an AWS Organizations relationship as either the payer or a linked account.
<br/>

<h2 id='VHY9CAgpq9k'>Setup the Control Tower Landing Zone</h2>

Log into your new ‘master’ account with the IAM user credentials you created for yourself.<br/>

<br/>

Navigate to the AWS Control Tower console and click on the “Set up landing zone” button.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/WiVn5Gy9HDsYpwyXKDsf9Q?a=PX5dTOTLUT8DaPPxrvxR6cBU9boBUSsPmMnzOvpabi8a' id='VHY9CAFrt71' alt='' width='800' height='429'></img></div><br/>

Provide emails for Log Archive Account and the Audit Account. These need to be email addresses distinct from each other and the email address you used for the main account. Something like <a href="mailto:log-archive@yourcompany.com">log-archive@yourcompany.com</a> and <a href="mailto:audit@yourcompany.com">audit@yourcompany.com</a>. <br/>

<br/>

Acknowledge the permissions notice before you click the “Setup landing zone” button. <br/>

<br/>

Get some coffee and set a timer for 60 minutes. It will take up an hour for AWS Control Tower to provision the ‘core’ control tower accounts and setup the other AWS services managed by Control Tower. You can safely close the browser and come back when your timer goes off.  <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/DEB-dGQZGPboadTolJ_r5Q?a=42KLxME7rFFHEQCwjnzxryfRpUVgaMtf4p2FiAL5egga' id='VHY9CAOV0Ms' alt='' width='800' height='429'></img></div><br/>

<h2 id='VHY9CAh3AAt'>Set AWS SSO password.</h2>

At some point during the AWS Control Tower setup, an email will be sent to the master account’s root email address. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/T_AqEagBjiDHJvzrXCxJsQ?a=IyfWTp64tipoLV23N9v6ibYFjRShNcSSY6UwyOGatcsa' id='VHY9CAsoSnn' alt='' width='800' height='613'></img></div>Click the “Accept Invitation” button and create a password. This username/password will only be used one-time during the setup to create the necessary credentials across the Master, Transit, and Research accounts. We will be changing the AWS SSO directory to Active Directory at the end of this guide, so this username/password will not be used after that.<br/>

<h2 id='VHY9CAeQJqF'>Create the Transit Account</h2>

<br/>

Once Control Tower has finished setting up the landing zone, its time to provision the Transit Account. The transit account will eventually house the Client VPN Endpoint and Transit Gateway portion of the solution. All future inter-account or inter-vpc networking can and should be configured through the Transit Gateway created by this account. Ideally, only your network administrator will have access to log into the Transit Account’s AWS Console. <br/>

<br/>

Navigate to Control Tower in the AWS Console and click on the “Account Factory“ link on the left side, then the ”Provision new account“ button.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/jQKIwdKHF0zuqMNs9dnkUg?a=ifGpLM8hgiJ4tBPna5YctF2voV0aGQU2w9pzgpNzgewa' id='VHY9CAdpRnF' alt='' width='800' height='429'></img></div><br/>

That will redirect you to the AWS Service Catalog product list. Click on the “AWS Control Tower Account Factory”.  <br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/acXLgllrglkZypp-sdN_aQ?a=857rvr3QXu6gAn0LDchhEvD50FaA577SCzLB6jwXdrUa' id='VHY9CAALBqS' alt='' width='800' height='429'></img></div><br/>

Click the large “Launch Product” button.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/Rd1sZFVw6IMyRQv70pfPjA?a=6rF9ifM9mWdPFyg5Ep4Pj4hian1CQ1KpRyDY6pXa4aAa' id='VHY9CA1oV1A' alt='' width='800' height='429'></img></div><br/>

Provide a name for the Transit account stack, for example “TransitAccount”.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/XvD8lpGe8Q-Dze5jxLvZkg?a=FzTkRch0YnduVIawCvHa76JnhHnIFInbLlzal7KC9oka' id='VHY9CAk2aGY' alt='' width='800' height='429'></img></div>On the parameters section of the wizard, provide the following parameters:<br/>

<div data-section-style='13'><table id='VHY9CA8uP7J' title='Sheet1' style='width: 46.4138em'><tbody><tr id='VHY9CASoVjD'><td id='s:VHY9CASoVjD;VHY9CAKOZV4' style='' class='bold'>Parameter Name

<br/></td><td id='s:VHY9CASoVjD;VHY9CAVEDpv' style='' class='bold'>Reccomended Value

<br/></td><td id='s:VHY9CASoVjD;VHY9CAW2dH0' style='' class='bold'>Notes

<br/></td></tr><tr id='VHY9CAsShel'><td id='s:VHY9CAsShel;VHY9CAKOZV4' style=''>SSO User Email

<br/></td><td id='s:VHY9CAsShel;VHY9CAVEDpv' style=''>admin-transit@yourcompany.com

<br/></td><td id='s:VHY9CAsShel;VHY9CAW2dH0' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CA6frUf'><td id='s:VHY9CA6frUf;VHY9CAKOZV4' style=''>Account Email

<br/></td><td id='s:VHY9CA6frUf;VHY9CAVEDpv' style=''>admin-transit@yourcompany.com

<br/></td><td id='s:VHY9CA6frUf;VHY9CAW2dH0' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CAy2m3g'><td id='s:VHY9CAy2m3g;VHY9CAKOZV4' style=''>SSO User First Name

<br/></td><td id='s:VHY9CAy2m3g;VHY9CAVEDpv' style=''>Admin

<br/></td><td id='s:VHY9CAy2m3g;VHY9CAW2dH0' style=''>

<br/></td></tr><tr id='VHY9CARoa6o'><td id='s:VHY9CARoa6o;VHY9CAKOZV4' style=''>SSO User Last Name

<br/></td><td id='s:VHY9CARoa6o;VHY9CAVEDpv' style=''>Transit

<br/></td><td id='s:VHY9CARoa6o;VHY9CAW2dH0' style=''>

<br/></td></tr><tr id='VHY9CASM1dY'><td id='s:VHY9CASM1dY;VHY9CAKOZV4' style=''>Managed Org Unit

<br/></td><td id='s:VHY9CASM1dY;VHY9CAVEDpv' style=''>Custom

<br/></td><td id='s:VHY9CASM1dY;VHY9CAW2dH0' style=''>Drop down field. You will only see this option.

<br/></td></tr><tr id='VHY9CArf8SS'><td id='s:VHY9CArf8SS;VHY9CAKOZV4' style=''>Account Name

<br/></td><td id='s:VHY9CArf8SS;VHY9CAVEDpv' style=''>Transit

<br/></td><td id='s:VHY9CArf8SS;VHY9CAW2dH0' style=''>

<br/></td></tr></tbody></table></div>Proceed through the rest of the wizard just accepting defaults and click the “Launch” button when you get to the Review page of the wizard.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/ojMliEkWEhAADg4NtcGt2g?a=AJ5skHkpaDJfYU3veGxrT5kkORjWdkfaVJaE5BaaPMoa' id='VHY9CASekwb' alt='' width='800' height='429'></img></div><br/>

You will want to set another timer. It takes about 25 minutes for the account to be created. <br/>

<br/>

It will look like this once the account has been created:<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/UELMsRUs8B8SnUhv8U3m9g?a=sbGyyVfWSXAxaCRFkf1ygo4SF30JoE6US5F2stGRCGoa' id='VHY9CAgUirI' alt='' width='800' height='466'></img></div><br/>

You have to wait for this to complete before you proceed. <br/>

<h2 id='VHY9CACl97i'>Create the Research Account</h2>

The Research account is the future home of your bio/chem informatics tools and pipelines. All of the rest of the accounts we have created so far are designed to provide technical separation of duties according to best practices. For example, the master account handles identity, the transit account handles networking, the audit account handles cloudtrail events for all accounts, and the logging account handles cloudwatch logs for all accounts. <br/>

<br/>

You will need to follow the same steps you went through for the Transit account to create the Research account. Just use the following parameters instead:<br/>

<br/>

<div data-section-style='13'><table id='VHY9CA6nmkQ' title='Sheet2' style='width: 49.4138em'><tbody><tr id='VHY9CAXY69S'><td id='s:VHY9CAXY69S;VHY9CAfqRXK' style='' class='bold'>Parameter Name

<br/></td><td id='s:VHY9CAXY69S;VHY9CAkEvlY' style='' class='bold'>Reccomended Value

<br/></td><td id='s:VHY9CAXY69S;VHY9CAL9fxo' style='' class='bold'>Notes

<br/></td></tr><tr id='VHY9CAZCb3B'><td id='s:VHY9CAZCb3B;VHY9CAfqRXK' style=''>SSO User Email

<br/></td><td id='s:VHY9CAZCb3B;VHY9CAkEvlY' style=''>admin-research@yourcompany.com

<br/></td><td id='s:VHY9CAZCb3B;VHY9CAL9fxo' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CAaTYyF'><td id='s:VHY9CAaTYyF;VHY9CAfqRXK' style=''>Account Email

<br/></td><td id='s:VHY9CAaTYyF;VHY9CAkEvlY' style=''>admin-research@yourcompany.com

<br/></td><td id='s:VHY9CAaTYyF;VHY9CAL9fxo' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CAWHKkr'><td id='s:VHY9CAWHKkr;VHY9CAfqRXK' style=''>SSO User First Name

<br/></td><td id='s:VHY9CAWHKkr;VHY9CAkEvlY' style=''>Admin

<br/></td><td id='s:VHY9CAWHKkr;VHY9CAL9fxo' style=''>

<br/></td></tr><tr id='VHY9CAIrKWj'><td id='s:VHY9CAIrKWj;VHY9CAfqRXK' style=''>SSO User Last Name

<br/></td><td id='s:VHY9CAIrKWj;VHY9CAkEvlY' style=''>Research

<br/></td><td id='s:VHY9CAIrKWj;VHY9CAL9fxo' style=''>

<br/></td></tr><tr id='VHY9CAluuGq'><td id='s:VHY9CAluuGq;VHY9CAfqRXK' style=''>Managed Org Unit

<br/></td><td id='s:VHY9CAluuGq;VHY9CAkEvlY' style=''>Custom

<br/></td><td id='s:VHY9CAluuGq;VHY9CAL9fxo' style=''>Drop down field. You will only see this option.

<br/></td></tr><tr id='VHY9CAYs1Ik'><td id='s:VHY9CAYs1Ik;VHY9CAfqRXK' style=''>Account Name

<br/></td><td id='s:VHY9CAYs1Ik;VHY9CAkEvlY' style=''>Research

<br/></td><td id='s:VHY9CAYs1Ik;VHY9CAL9fxo' style=''>

<br/></td></tr></tbody></table></div><h2 id='VHY9CASClwc'>Enable sharing within your AWS Organization</h2>

Inside your master account, open the AWS Resource Access Manager (RAM) in the AWS console. AWS RAM is a service that makes sharing resources (like the Transit Gateway that gets created later on) between accounts easier and more secure. <br/>

<br/>

Go to the ‘Settings’ portion of RAM and check the “Enable sharing within your AWS Organization” and click “Save Settings”<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/tgFFemcZ8G9jFO2fcQxEkQ?a=PrZ6yRIEHTmXKNcgsxa2kxs0aDqZFl4zUN6jkqakWlAa' id='VHY9CAnYvD9' alt='' width='800' height='429'></img></div><br/>

<h2 id='VHY9CANPCAY'>Elevate permissions in the Transit and Research accounts.</h2>

By default, accounts provisioned by Control Tower (excluding the Audit and Logging accounts) don’t automatically grant cross-account administrator privileges to any user or principal. We need to grant administrator permissions to the user you setup in the “Set AWS SSO password” section above for the Transit and Research accounts so that we can start deploying resources into them.<br/>

<h3 id='VHY9CABy3yX'>Grant permissions</h3>

Go to the AWS SSO service page in the AWS console and go to the “AWS Accounts” tab:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/P_rHXkhdMKNvfIQNoewrmg?a=uzA83bxJ1NFhaqF3x9Sz9H2fNnm4kdc6vYxI85g0gpYa' id='VHY9CAgYytX' alt='' width='800' height='429'></img></div>Select the check boxes next to the Research and Transit accounts and click “Assign Users” button.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/xiVVYo1cI9VY8X-J2M9caA?a=aUMDxdrFWOwrzTQA5sn4f8FrkNe8o0o6AQS39dRjnboa' id='VHY9CAF3RXL' alt='' width='800' height='429'></img></div><br/>

Select the “AWS Control Tower Admin” account and click “Next: Permission sets”<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/MGpoVq5Nnm8sTuxqazRyIQ?a=3amjTkCUIr0WZ14kSaoMCtv16WVQPLD8BzPRDQOx70Ia' id='VHY9CAuQZQ4' alt='' width='800' height='429'></img></div>Choose “AWS Administrator Access” and then click the “Finish” button. Should only take a few seconds. “Click the Proceed to AWS Accounts button“<br/>

<h3 id='VHY9CAPJq55'>Extend token expiration</h3>

The default token expiration for IAM roles assumed by AWS SSO is 1 hour. It may take you longer than an hour to complete the following steps if you take a break along the way. <br/>

<br/>

To extend that expiration, click on the “Permission Sets” tab and click on the “AWSAdministratorAccess” permission set directly. (Don’t click the radio button)<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/RrG3ew9mSYgnaUme7b6C9Q?a=2qtkuNM3FXhgc7RERfaM2tD1OJdtNpS3a34CJ9WgX78a' id='VHY9CA1vBT5' alt='' width='800' height='429'></img></div>Click the “Edit”button and set the session duration to something greater than an hour. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/R1NXBTolbnZOg1sP74ITlQ?a=WgPdz01v2GWSjiyIOaKIa0IEl0Td2udTMiGfASN7i9Ea' id='VHY9CAHemyk' alt='' width='800' height='429'></img></div>It will then ask you which accounts you want to reprovision the policy in. Select the Transit, Master, and Research accounts and click “Reprovision”<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/GgUlhoEi--YKiAhYDzOgUA?a=qTDoaWXyFx2zeWokwaK9H2RU5NjRCQpy1IeyaD7dAVMa' id='VHY9CADZjOi' alt='' width='800' height='429'></img></div><br/>

Should only take a few seconds.<br/>

<h2 id='VHY9CAxs9ZB'>Prepare the Deployment Environment with AWS Cloud9</h2>

AWS Cloud9 is a web based integrated development environment. You could run all of the same commands below on your local machine, but Cloud 9 comes with many dependencies pre-installed and tends to be a much faster to get started. <br/>

<br/>

Navigate to the AWS Cloud9 console, and click the “Create environment” button.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/Jjern2OYcq8RiWnFtgRIbA?a=ZPu6atam7fB7EsTyCo5r8YNZfYTSq9agbYP4cIwyqfwa' id='VHY9CArmYWM' alt='' width='800' height='429'></img></div><br/>

Follow the creation wizard, but you only need to specify the following parameters. Leave the rest as their defaults. <br/>

<br/>

Environment Name (Call it something like Biotech Blueprint Deployment Console)<br/>

Instance Type: t2.small<br/>

<br/>

It will take a few minutes, but you will eventually be presented with the following development environment:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/KwVV8L5vkBHOcdhVfrOIrQ?a=cGbuukPQuaTW0408hV2Hht8nd2Ey30CpiLaHavaFmiYa' id='VHY9CALYQ7u' alt='' width='800' height='429'></img></div><br/>

Notice the terminal window at the bottom of the IDE. Run the following commands. You may get warnings, but they can be ignored. We are installing the AWS Cloud Development Kit (CDK) and pulling down the Biotech Blueprint source code. <br/>

<pre id='VHY9CAgzKUm'>git clone <a href="https://github.com/paulu-aws/biotechblueprint.git">https://github.com/paulu-aws/biotechblueprint.git</a><br>cd biotechblueprint<br>./prepCloud9env.sh</pre>

The final output of the prepCloud9Env script is the path to the AWS credentials file. Click on the file name, and chose “Open” to open up the AWS Credentials File.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/BzYP12hI_gGBEzUtuaP8EQ?a=nTp3BaWkaBnFsgR7lY6aKoT7pADQgY90LL8exKmNxTYa' id='VHY9CAeNiJ1' alt='' width='800' height='470'></img></div><h2 id='VHY9CAU5NKa'>Prepare Your AWS Credentials File</h2>

Go ahead and delete the contents of the credentials file. <br/>

<br/>

You may get a warning when Cloud9 detects you have changed this file. One of the features of Cloud9 is automatically refreshing this file with temporary keys for the IAM user assigned to Cloud9. Because we are deploying things across multiple accounts, we want to disable this automatic refresh. When the warning prompt appears, click the “permanantly disable refresh button”<br/>

<br/>

We are going to be deploying lots of stuff in the Master, Transit, and Research account. We need to prepare this credentials file that will give the Cloud9 environment permissions.<br/>

<h3 id='VHY9CAPNj9Z'><b><span style="color:#333333" textcolor="#333333">Log into your SSO portal.</span></b></h3>

<span style="color:#333333" textcolor="#333333">From the master account, visit the AWS SSO page in the console again. When the AWS SSO Dashboard comes up, visit the “User portal URL” listed at the bottom of the dashboard and log in with the username/password that you used in the “Set AWS SSO password.” section above.</span><br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/cOk52pCm-dW73CVVpltGbQ?a=ZHq7iqy5L6aPOnyPDcVh8GmckcFaIaTuWz4fHy4htxca' id='VHY9CAcHBIH' alt='' width='800' height='429'></img></div><br/>

You should see something like the following once you have logged in.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/iNJHX3C8CutuSt99NDjQgA?a=DVBQV8PJwzaTCA40vpC0OKMtfaDVOIVYFRYiNDr6WaYa' id='VHY9CAe06oO' alt='' width='800' height='466'></img></div><br/>

Expand the "Research" account, and click on the"Command line or programmatic access" button on the "AWSAdministratorAccess" row. <br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/XDwwIgaeKhe7IodhNXZ4rg?a=eyIaGjuP7kR6lY33O3sTd4VngXn96DRgi5pyaaKiqNMa' id='VHY9CAGD8H2' alt='' width='800' height='429'></img></div><br/>

Hover over the "Option 2" and copy that text. <br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/iqPZ5Xcxdyr1AfehgWE_-w?a=lwdiNclu6Gk5hks1DNUruxR4xSOnJzJuqetRn9ftEvca' id='VHY9CA6UiRK' alt='' width='800' height='466'></img></div><br/>

That text will look something like this:<br/>

<br/>

<code>[111111111111_AWSOrganizationsFullAccess]</code><br/>

<code>aws_access_key_id = ASIA....P5MC4E</code><br/>

<code>aws_secret_access_key = lZfRUkA.....1daLxqcSOx0E</code><br/>

<code>aws_session_token = AgoJb3JpZ............0grr1</code><br/>

<br/>

Paste that text into the open credentials file in the Cloud9 environment located at <code>~/.aws/credentials</code><br/>

<br/>

Replace the <code>[111111111111_AWSOrganizationsFullAccess] </code>with <code>[research]</code><br/>

<br/>

Do the same for the transit and master account such that your aws credentials file ends up looking like this: <br/>

<br/>

<pre id='VHY9CAuvtsI'>[research]<br>aws_access_key_id = ASIAR7.....GUMM6<br>aws_secret_access_key = kN4LVgE......oKidZ+3BxMmyyjwXl8PjD9<br>aws_session_token = Ago............Jc<br>region=us-east-1<br><br>[master]<br>aws_access_key_id = ASIAXZ...6GRA6U<br>aws_secret_access_key = pj4EMH......MsXmjdiWRg<br>aws_session_token = AgoJ............bD<br>region=us-east-1<br><br>[transit]<br>aws_access_key_id = ASIA3Q...X4LIPYBL<br>aws_secret_access_key = D+xfSxawa......gnXCiEYAh<br>aws_session_token = AgoJb3Jpujc............NU7ZQETpGyO2u<br>region=us-east-1</pre>

Make sure you add region=us-east-1, or whatever your desired home region is to each profile. <br/>

<br/>

Save the credentials file when you are done.<br/>

<h2 id='VHY9CAyzLmI'>Set your deployment preferences:</h2>

Open the file cdk.json at <code>~/biotechblueprint/cdk.json</code><br/>

<br/>

This file is where various context values are set for the CDK CLI deployment. The only value that you NEED to set is <code>corporateDnsApex</code>. That value needs to map to your company’s apex domain. If your company name is ExampleCorp and you own the domain examplecorp.com, use that.<br/>

<br/>

The only other value you might want to change is <code>corporateNetBiosShortName</code>. This is used with Active Directory as the domain’s short name. If you don’t know what this is or don’t care, just leave the default value of <code>corp</code>.<br/>

<br/>

Leave the rest of the values alone. The deployment script will automatically populate them. <br/>

<pre id='VHY9CAyyTGq'>{<br>  "app": "npx ts-node bin/bb-20.ts",<br>  "requireApproval": "never",<br>  "context": {<br>    <br>    "corporateDnsApex" : "examplecorp.com",<br>    "corporateNetBiosShortName": "corp",<br>    <br>    "transitGatewayRouteTableSecretArn": "XXXXXXXXXXXX",<br>    "researchTgAttachmentSecretArn": "XXXXXXXXXXXX",<br>    "transitGatewaySecretArn": "XXXXXXXXXXXX",<br>    "researchVpcCidrSecretArn": "XXXXXXXXXXXX",<br>    "identityTgAttachmentSecretArn": "XXXXXXXXXXXX",<br>    "identityVpcCidrSecretArn": "XXXXXXXXXXXX",<br>    "identityAccountAdConnectorSecretArn": "XXXXXXXXXXXX",<br>    "identityAccountAdConnectorSecretKeyArn": "XXXXXXXXXXXX",<br>    "masterAcctId": "XXXXXXXXXXXX",<br>    "transitAcctId": "XXXXXXXXXXXXX",<br>    "researchAcctId": "XXXXXXXXXXXXX",<br>    "orgArn": "XXXXXXXXXXXXXXX"<br>  }<br>}</pre>

<h2 id='VHY9CAHdQlO'>Run deployBiotechBlueprint.sh</h2>

Now you just need to run the following command <br/>

<br/>

<code>./deployBiotechBlueprint.sh</code><br/>

<br/>

This script will kick off a number of AWS CDK deployment commands that will stand up all of the components in the Transit, Master, and Research accounts. You will want to set another timer. The deployment will take approximately 1 hour. For the record, 40 minutes of that is waiting for Microsoft’s Active Directory to boot up.<br/>

<br/>

<h2 id='VHY9CAkAZ8g'>Connect to the VPN </h2>

The previous deployment script sets up Active Directory in the master account, AWS AD Connectors in the Transit and Research accounts, and an Active Directory integrated Client VPN Endpoint in the Transit account. <br/>

<br/>

In order to manage users and groups, you need to first connect to the VPN Endpoint which will allow you to route into the Identity stack. From there, you can directly connect the Active Directory domain controllers from your local machine or RDP into the “Domain Controller Console” host and administer AD from there. <br/>

<h3 id='VHY9CAQfiIV'>Download Client VPN Configuration File</h3>

The deployment script will automatically output the TransitVPN.ovpn file @ <code>~/environment/TransitVPN.ovpn</code>. You can right click on that file in Cloud9 to download it. You can also download again from the AWS web console by locating the Client VPN Endpoint in the Transit Account’s VPC console, selecting it, and clicking the “Download Client Configuration” button.<br/>

<br/>

Location of the TransitVPN.ovpn config file in Cloud9:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/VN6NyCe4PFHTiU93sl9pmQ?a=Eo95nLOXQsWxYpBauE48s5aR1MG8a96GBWoft15DxuIa' id='VHY9CA6k9kN' alt='' width='800' height='466'></img></div>For reference, you can also download the same client configuration file from the AWS VPC console’s "Client VPN Endpoints" page. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/HmtUeeRQRCDt2GpmxHKsWA?a=aMIKzBpr9QJajVhmeI7arteifUFXNZfcMWa0Wyjp75Aa' id='VHY9CA2IFmZ' alt='' width='800' height='429'></img></div><h3 id='VHY9CA3GxkI'>Download Client VPN Software  and import the CONFIGURATION file</h3>

You will first need a Client VPN application installed on your local machine. <a href="https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect.html">These instructions have links to download</a> various compatible VPN Client tools depending on your OS (Windows/Android/iOS/MacOS/Ubuntu). Those instructions also tell you how to import a configuration file. Follow those steps with the TransitVPN.ovpn file you just downloaded on your local machine. <br/>

<h3 id='VHY9CAfLC18'>Connect to the VPN</h3>

Go ahead and connect to the VPN. You will be prompted to login with credentials. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/kuJkz1g7K5fw5TYtIjzpDQ?a=GrKNheK3F6XIlcLYQpj2hjStsu55IuNVZRpwnEukkosa' id='VHY9CApxbHs' alt=''></img></div><br/>

By default, AWS Directory Services provisions a single username, “admin”, that has full domain controller permissions. The Biotech Blueprint deployment script has automatically given this admin permissions to log into this domain. To obtain the password, log into the Master Account and go to the AWS Secrets Manager service page.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/UmActxtymNaPK5zLIRjfYg?a=gdNZUq3Tp5ADV8D2ki1OjYFpxqgaozagqfQK849LdHsa' id='VHY9CAKlUee' alt='' width='800' height='429'></img></div> Click on the <code>ADAdminCreds</code> secret <br/>

<br/>

Scroll down to the “Secret value” section and click the “Retrieve secret value” button:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/L7RuXOKYh8xqv1jmQWyo2A?a=apWABgKqgIhrK4BwLRrza1ICf2xDmaIAaVwLmYzKDAMa' id='VHY9CAGfC7V' alt='' width='800' height='429'></img></div><br/>

You will see the username/password combo that you should use log into the VPN. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/bCSSuMOXnULNO20gb8iMYQ?a=T7aZFnNaIuGOYYmMb2fmYKObgYV5bcPNN0OF77bACx4a' id='VHY9CA5OHKB' alt=''></img></div><br/>

You are now connected to the cloud! Now lets connect to that domain controller and start adding users/groups that are relevant to your company. <br/>

<br/>

<h2 id='VHY9CACxj8O'>Create your desired AD Users and Groups</h2>

At this point, you can directly connect your local machine to Active Directory and manage it if you happen to have the AD administration tools already installed on your local machine. For simplicity sake, the Biotech Blueprint deployment script launches a small instance in the identity stack’s private subnet that already has the AD management tools installed and is already joined to the domain. All you need to do is RDP into that host. Keep in mind, this host is in a private subnet only routable by clients that are connected to the VPN.<br/>

<br/>

To connect to the host, log into the master account AWS console and visit the EC2 service page. Under “Instances”, you will see the “Domain Controller Console”. Right click on that instance and choose connect. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/hYM1mYdqUMsKc5QSqrrNBw?a=3GzvLehJLdzlw6aZWhSEv529uxceXkb5svtCZV51Vr8a' id='VHY9CALB9hV' alt='' width='800' height='429'></img></div><br/>

Click on the “Download Remote Desktop File” and open it with your remote desktop client:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/-5dT1RgBjBHYEYh-svW9vA?a=uoxhFkG4M9MZqQbncAJBOYGBnultfdVSksiVfayysRka' id='VHY9CA2I5me' alt='' width='800' height='429'></img></div>You may get a warning when you connect. Go ahead and click the connect button.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/FJypPRD3SwpmG0a3GlPZIQ?a=d3uiPPtWaq2Dmt0iiQGO1i1fHxo9MQ0S99iX22aaaLMa' id='VHY9CA5QZGT' alt=''></img></div><br/>

You need to connect with the same AD credentials that you logged into the Client VPN Endpoint with. Your RDP client may default the user to your local machine’s current user. Click the “More choices” link and “Use a different account” option.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/OAUFmFXqHA3pwQc7aIktvA?a=NjgT6EbKu6VaCDn2v1Te0qkuogRF5Rnyy0zHNf7T0D4a' id='VHY9CA4zipy' alt=''></img></div><br/>

Your username needs to be a combination of your NetBIOS shortname and the “admin” username. You would have specified the shortname as the <code>corporateNetBiosShortName</code> value in the “Set your deployment preferences” section above. If you left the default value, the username will be: “corp\admin”. <br/>

<br/>

Your password is the same password you used to log into the VPN as described in the “Connect to the VPN” section above. You can get that same value again from Secrets Manager in the master account if you need to.<br/>

<br/>

Click OK and you will get automatically logged into the Domain Controller Console instance. <br/>

<br/>

When the instance comes up, click the Windows icon in the bottom left corner and type in “Active Directory Users and Computers” and open the app. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/zcq-Jitkv7eva7zNlUc9xQ?a=GxjlQi6HX0PalmjNR2qsTJJwFLJkjkmK1uczWS8HRrka' id='VHY9CA17JPI' alt='' width='800' height='449'></img></div><br/>

The snapin will automatically connect to Active Directory as the domain administrator (because you are logged in as the domain admin).<br/>

<br/>

To start creating groups/users, expand the tree down from “yourcompany.com” to “yourshortname” to “Users”. For example:<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/eONsc_MowKEpHRIWhI0uHw?a=Vk8jlSWtQFauSQhcZgMAqps9VEi7mEP6Qaxg6Fo7doEa' id='VHY9CApy7FF' alt='' width='800' height='431'></img></div><br/>

You will see 4 items in the list:<br/>

<br/>

<div data-section-style='5' class="" style=""><ul id='VHY9CAklNv7'><li id='VHY9CAj5bW5' class='' value='1'>Admin - This USER is created automatically by AWS directory services and the user you are currently logged in as.

<br/></li><li id='VHY9CAKKO6m' class=''>svc_adconnector - This USER is a service account used by the AD Connectors in the Transit and Research accounts to connect to the domain controller.

<br/></li><li id='VHY9CAMjghY' class=''>Connectors - This GROUP gives permissions for the AD connectors in the Transit and Research accounts.

<br/></li><li id='VHY9CA9wjFe' class=''>CorporateVpnUsers - This GROUP gives members permissions to connect to the Transit VPN solution. You will notice Admin is already a member of this group.

<br/></li></ul></div>At this point, you can start creating groups and individual users as you see fit. <br/>

<br/>

Start by creating a user for yourself. Make sure you add yourself to the CorporateVpnUsers group.<br/>

<br/>

If you want to allow someone else to connect to the VPN, create a user for them and add them to the CorporateVpnUsers group.<br/>

<br/>

In the next step, we are going to give AWS permissions based on the AD groups you create. We recommend that you create the following groups and add the user you created for yourself to them.<br/>

<br/>

ResearchAccountAdmins<br/>

ResearchAccountUsers<br/>

TransitAccountAdmins<br/>

MasterAccountAdmins<br/>

AuditAccountAdmins<br/>

LoggingAccountAdmins<br/>

<br/>

We will associate these groups to AWS IAM  permissions in the next step.<br/>

<br/>

For the most part, non-IT staff will never need AWS permissions on the Transit/Master/Audit/Logging accounts and should likely never be added to those groups. <br/>

<br/>

<h2 id='VHY9CA8f8SH'>Switch AWS SSO to use AWS Directory Services</h2>

Up to this point, the AWS SSO service that AWS Control Tower setup for you in the master account has been using the AWS SSO Directory. Because we are maintaining users/groups in Active Directory, we are going to change AWS SSO to use AD instead. Log into the master account and navigate to the AWS SSO service page. <br/>

<br/>

Click on the “Directory” section.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/Ghlo32OkOfNzu2g2tDVPwQ?a=yXb5gUpYOaYOjrHZJmCqcsZNdTpeLuXlpyTVwk2SQk4a' id='VHY9CAHLbDT' alt='' width='800' height='429'></img></div><br/>

Click on the “Change directory” link.<br/>

<br/>

Choose “Microsoft AD directory”.<br/>

<br/>

The identity stack’s AD environment should be prepopulated in the “Existing directories" drop down. Click Next.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/mz6J_ARbxxIJzKBBhzQxfA?a=J4KwQ1E6OhgTdTgEQxjDJocVg27kzqC8D76Ccwju7aAa' id='VHY9CA26zCc' alt='' width='800' height='429'></img></div><br/>

You will get a warning that that all existing permissions will be removed. Type “CONFIRM” to acknowledge this. <br/>

<br/>

It should only take a few seconds. You should see the following if it completed successfully. <br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/4uGqlTdMHQcH9C9dU-bHng?a=ARzXdDaOQfal63IyaaX5a9oQb43S1ZlLfhqIxYTKoB8a' id='VHY9CALC8Wx' alt='' width='800' height='429'></img></div><br/>

Click on the “Proceed to the directory” button. Then navigate the “AWS accounts” page in the AWS SSO console.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/xPTJXCZK1nGPRV73VUWReg?a=ucPJukSgQ3A2kzabzZDaGVpDtcuPe5Z7vGfXLe0V63Aa' id='VHY9CANISd1' alt='' width='800' height='429'></img></div><br/>

Its from here that we assign Active Directory users or groups IAM permissions. Lets start off giving access to the master account. <br/>

<br/>

Check the box next to the master account on the list and click the “Assign users” button. <br/>

<br/>

You will see a search box come up to search for group names. Search for the group name you created earlier that you want to give master account admin privileges too. If you followed the recommended group names above, it would be “MasterAccountAdmins”. (Make sure you added yourself to that group  as described in the “Create your desired AD users and groups“ section above) Then click ”Next: Permissions sets“.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/j3CI62gDvGnQBm3863EiTQ?a=frV9zabtGfa6X40X44kbGWvh9qYLV01TPvoADqTmDO8a' id='VHY9CAb4cau' alt='' width='800' height='429'></img></div><br/>

Now we are about to assign permissions to users of that group on the account we selected. In order to give members of this group full administrator access to the account, we just need to check the “AWSAdministratorAccess” policy and click finish. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/X7ACbgj3oeHyzTslMwYSpg?a=W5aWjkyMrZ27rbB8raaJQtH4aLoQQT4nxhM577U9gDUa' id='VHY9CAa8e13' alt='' width='800' height='429'></img></div><br/>

As you can see, there are other types of pre-built permission sets that you can assign to users or create your own. You can also select multiple permissions sets for the group. If a users has multiple permissions sets, they will see an option to login to the account for each permission set the belong to. For example, you may want to give users the option of escalating their privileges or emulating another role’s permissions. <br/>

<br/>

Repeat this process for the Transit, Research, Audit, and Log Archive accounts based on the groups or users you created in Active Directory. <br/>

<h2 id='VHY9CAEcZe0'>Log into your AWS Portal and conquer</h2>

Go back to the AWS SSO Dashboard and copy the “User portal URL”.<br/>

<br/>

This URL is the entry point for for all users who need access to the AWS Console, CLI, or SDKs. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/tKz57e8lZjmr2L2fmQm_-g?a=qEtXPmE67HQ73BwpmtRdcGJdPavhzPTPonIMyNNSjSAa' id='VHY9CA9dik2' alt='' width='800' height='429'></img></div><br/>

Go ahead and visit that URL and login with the Active Directory username and password you created for yourself earlier. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/9HI7xYtTMlGBH6WRZMZuKw?a=o1k6jFdw3MHNm3NrKvPAxvpt3E3a34hP9OrX3OoeVZQa' id='VHY9CAQVqj0' alt='' width='800' height='429'></img></div>Expand the various accounts to access the management console or obtain AWS access keys and secret keys. In the image below, all of the accounts are listed as an illustration. If the logged in user only had access to one account, they would only see one account listed (or get logged in automatically).<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/j2Lj_ZfVz9zoaXdiA4CYgg?a=QO8XrwVWoV8kug9Ff79iX5KF9AFAXlapOXFv23s3JRIa' id='VHY9CAL1nJS' alt='' width='800' height='429'></img></div><h2 id='VHY9CALjcAD'>Giving account access to other users</h2>

In order to allow others to connect to the VPN or the AWS console you need to provide them with 3 things:<br/>

<div data-section-style='5' class="" style=""><ul id='VHY9CAnMtqU'><li id='VHY9CAdWfeK' class='' value='1'>Active Directory Credentials - Whatever username and password you created for them

<br/></li><li id='VHY9CAOdZ48' class=''>For VPN Access, give them the same TransitVPN.ovpn file that you downloaded earlier and connected with. That file contains no passwords and can be shared safely among members of your company. Each user will be logging in with their own unique AD credentials you created for them.

<br/></li><li id='VHY9CA9S4XN' class=''>For AWS Console access, give them the “User portal URL” from the AWS SSO page that you just logged in with. They will need to log in with their AD credentials. 

<br/></li></ul></div><br/>

<h2 id='VHY9CA1jMMq'>Creating and enrolling a new account into the Blueprint</h2>

At this point, you can begin your work in the shared research account that was created during the initial Biotech Blueprint deployment. You may now want to create a new account for a specific department in your company, HIPAA/clinical boundary, a CRO, a university, a partner, a consultant, an internal team, a production/test/development, or even for individual developer sandboxes. Any time you want to create a strong account-level boundary between resources created by AWS, you should consider following this process.<br/>

<br/>

From the master account, you will need to visit the Control Tower console and click the “Provision new account” button:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/LjAtM0p-oReAgSSQnsffSg?a=EjvthwaQnyx2Bv5XQdK373ay1SYixlNWaDsYlztlNfQa' id='VHY9CAtK4tX' alt='' width='800' height='429'></img></div><br/>

From there, launch the “Account Factory” product:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/rJiM7dHhV6Q1RJwa4CicVg?a=NoAY3CXV1gh5OVoqeXiA86OA952MpVnv0L5vy66urPoa' id='VHY9CA1CwbZ' alt='' width='800' height='429'></img></div><br/>

For the purposes of this walkthrough, I’m going to pretend we are creating an account for a fictional Contract Research Organization (CRO) called CRO-ATX. Provide a name for the stack:<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/xaBTaQrxf05nfYj-QfcILg?a=fEdeJ6HZYtEn6BReNamcsX6RvL7lmptOA9sA50ABOLQa' id='VHY9CAM78CN' alt='' width='800' height='429'></img></div><br/>

You will need to supply the following parameters. You may be confused by the SSO* parameters. Didn't we switch SSO to use Active Directory instead of the AWS SSO identity store? The Account Factory portion of Control Tower still requires you supply the values but it does not use them.<br/>

<div data-section-style='13'><table id='VHY9CAxB1zS' title='Sheet3' style='width: 49.4138em'><tbody><tr id='VHY9CAJ449K'><td id='s:VHY9CAJ449K;VHY9CAKlfk9' style='' class='bold'>Parameter Name

<br/></td><td id='s:VHY9CAJ449K;VHY9CAtvh0a' style='' class='bold'>Reccomended Value

<br/></td><td id='s:VHY9CAJ449K;VHY9CA4Uf5R' style='' class='bold'>Notes

<br/></td></tr><tr id='VHY9CAYmgaj'><td id='s:VHY9CAYmgaj;VHY9CAKlfk9' style=''>SSO User Email

<br/></td><td id='s:VHY9CAYmgaj;VHY9CAtvh0a' style=''>admin-[groupname]@yourcompany.com

<br/></td><td id='s:VHY9CAYmgaj;VHY9CA4Uf5R' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CADBHCz'><td id='s:VHY9CADBHCz;VHY9CAKlfk9' style=''>Account Email

<br/></td><td id='s:VHY9CADBHCz;VHY9CAtvh0a' style=''>admin-[groupname]@yourcompany.com

<br/></td><td id='s:VHY9CADBHCz;VHY9CA4Uf5R' style=''>Replace with your company name

<br/></td></tr><tr id='VHY9CA5rBou'><td id='s:VHY9CA5rBou;VHY9CAKlfk9' style=''>SSO User First Name

<br/></td><td id='s:VHY9CA5rBou;VHY9CAtvh0a' style=''>Admin

<br/></td><td id='s:VHY9CA5rBou;VHY9CA4Uf5R' style=''>

<br/></td></tr><tr id='VHY9CANyKSd'><td id='s:VHY9CANyKSd;VHY9CAKlfk9' style=''>SSO User Last Name

<br/></td><td id='s:VHY9CANyKSd;VHY9CAtvh0a' style=''>[Group Name]

<br/></td><td id='s:VHY9CANyKSd;VHY9CA4Uf5R' style=''>The logical name of the group you are creating the account for

<br/></td></tr><tr id='VHY9CALzF0Z'><td id='s:VHY9CALzF0Z;VHY9CAKlfk9' style=''>Managed Org Unit

<br/></td><td id='s:VHY9CALzF0Z;VHY9CAtvh0a' style=''>Custom

<br/></td><td id='s:VHY9CALzF0Z;VHY9CA4Uf5R' style=''>Drop down field. You will only see this option.

<br/></td></tr><tr id='VHY9CA0hPAO'><td id='s:VHY9CA0hPAO;VHY9CAKlfk9' style=''>Account Name

<br/></td><td id='s:VHY9CA0hPAO;VHY9CAtvh0a' style=''>[Group Name]

<br/></td><td id='s:VHY9CA0hPAO;VHY9CA4Uf5R' style=''>The logical name of the group you are creating the account for

<br/></td></tr></tbody></table></div>Click through the wizard prompts accepting the default values and click the “Launch” button on the final review page. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/SJASVJ06YQ9hEqq_yZu8-w?a=FJ3B5kG6ar1jRA2SWMBH8ljqMCHavpa5WlMBVe3yzkwa' id='VHY9CAMls09' alt='' width='800' height='429'></img></div><br/>

Set another time for 20 minutes, watch some cat videos or get a coffee. After about 20 minutes you should see the deployment has succeeded under the “Status” section.<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/I_h_lofIzxJiigWu7-JFGQ?a=a5g6NY7LnNakXRNlmaWmZN7OXrPIpi05N8k6GVZjq1ga' id='VHY9CACxuTo' alt='' width='800' height='429'></img></div><br/>

Now, we need to give yourself administrator access to that environment. Visit the AWS SSO console’s “AWS Accounts” section, select the checkbox next to the new account, and click the “Assign users” button.<br/>

<br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/X7G8Y5grkPlhq_-b_DtaLg?a=YSGfTCUVVaaXys9pJZEEJqDgi9RZru2xBNADzmEs8QQa' id='VHY9CAv0xQ3' alt='' width='800' height='429'></img></div>Search for yourself or an AD group you created earlier and move on to the permission sets page. Choose the “AWSAdministratorAccess” permission set and click the “Finish” button. <br/>

<div data-section-style='11' style='max-width:100%'><img src='https://quip-amazon.com/blob/VHY9AAorigC/n8rwqHxLqT7JmW4ImHW4WQ?a=NbOaeuuhoPYbmLaOEq90OaEVcuoeo96MaTjiztO60VEa' id='VHY9CANzr2v' alt='' width='800' height='429'></img></div><br/>

Now that we have the account created, we need to ‘enroll’ it into the Transit and Identity stacks. This sets up the necessary networking infrastructure required to integrate with the transit and identity solutions the blueprint has deployed. <br/>

<br/>

The Biotech Blueprint follows a ‘infrastructure as code’ philosophy, so to deploy the necessary components we are going to extend the Biotech Blueprint code base and begin customizing to your company. Dont worry, this only involves adding a few lines to the end of 2 files.<br/>

<br/>

Open back up the Cloud9 environment in the Master account we used to deploy the blueprint.<br/>

<br/>

You will likely need to refresh the ~/.aws/credentials file like we did before. Follow the instructions in the “Prepare Your AWS Credentials File” section above, but this time add the additional account profile. It should now look something like this<br/>

<pre id='VHY9CADuFAy'><br>[research]<br>aws_access_key_id = AS....GZ<br>aws_secret_access_key = 3ZE.....Qk<br>aws_session_token = IQoJb3J.....QDcjvOBA<br>region=us-east-1<br><br>[master]<br>aws_access_key_id = A...OI<br>aws_secret_access_key = Q....EUJz+<br>aws_session_token = IQoJ....wcYKP<br>region=us-east-1<br><br>[transit]<br>aws_access_key_id = ASI....Y77<br>aws_secret_access_key = 0vaFfY....5s<br>aws_session_token = IQoJb3...GpgmTbs<br>region=us-east-1<br><br>[CROatx]<br>aws_access_key_id = AS.....JCY<br>aws_secret_access_key = 2RF......EgIq<br>aws_session_token = IQoJb3J.....y3Tsm<br>region=us-east-1</pre>

Now, open up the <code>biotechblueprint\cdk.json </code>file.<br/>

<br/>

For demonstration, we are using a fictional CRO called “CROatx”. In the following three snippets, do a find/replace for “CROatx” to a more appropriate name for the account you are creating. It needs to start with a capital letter, no spaces, no hyphens.<br/>

<br/>

Add the following <b>bolded </b>properties to the end of  the <code>context</code> property. <br/>

<pre id='VHY9CA7an0d'>{<br>  "app": "npx ts-node bin/bb-20.ts",<br>  "requireApproval": "never",<br>  "context": {<br>        ....<br><b>    </b><b>"envCROatxAccountId"</b><b>:</b><b> </b><b>"XXXXXXXXX"</b><b>,</b><br><b>    </b><b>"CROatxTgAttachmentSecretArn"</b><b>:</b><b> </b><b>"XXXXXXXXXXXXX"</b><b>,</b><br><b>    </b><b>"CROatxVpcCidrSecretArn"</b><b>:</b><b> </b><b>"XXXXXXXXXXXXX"</b><br>  }<br>}</pre>

Leave the <code><b>XXXXXXXXXXXXX</b></code><b> </b>values as Xs. The script automatically populates them.<br/>

<br/>

Now, open up the file <code>biotechblueprint\bin\bb-20.ts </code>and scroll down to the bottom.<br/>

<br/>

You will see an ‘example’ section where you can uncomment lines as necessary.  Again, find replace CROatx with the more meaningful name you used for the snippet above. <br/>

<pre id='VHY9CANaR3b'>/////////////////////////////////////////<br>/// Example of how to onboard an additional account into the Blueprint<br>/// Just find and replace 'CROatx' below with a more meaningful account name (must start with a capital letter, no spaces/numbers/hyphens)<br>/// Then uncomment the lines of code below. <br><br><br>// Example:<br>const envCROatx =   { account: app.node.tryGetContext("envCROatxAccountId"), desiredVpcCidr: "13.0.0.0/16"};<br>const CROatxAccount = new childAccountStack(app,'CROatxAccountStack', {env: envCROatx, desiredVpcCidr: envCROatx.desiredVpcCidr, desiredVpcName: "CROatxVpc"});<br><br><br>/// Here you need to make a descion about what the account should have access to.<br>/// Do you want users of this account to access resources via VPN in the transit stack? You need to instantiate the TransitToCROatxVpcRoute and CROatxToTransitVpcRoute<br>/// Do you want users/resources in this account to be able to route into the research vpc and vice versa? You need to instantiate ResearchToCROatxVpcRoute and CROatxToResearchVpcRoute<br>/// Do you want users/resources of this account need to communicate with the domain controller in the identity stack? You need to instantiate IdentityToCROatxVpcRoute and CROatxToIdentityVpcRoute<br><br>// Example:<br>const CROatxToTransitVpcRoute = new VpcRouteTableTransitRouteStack(app,'CROatxToTransitVpcRoute', {<br>  env: envCROatx,   destinationCidr: envTransit.desiredVpcCidr,   targetVpc: CROatxAccount.Vpc });<br>const TransitToCROatxVpcRoute = new VpcRouteTableTransitRouteStack(app,'TransitToCROatxVpcRoute', {<br>  env: envTransit,   destinationCidr: envCROatx.desiredVpcCidr,   targetVpc: transitAccount.vpc });<br>  <br>const CROatxToIdentityVpcRoute = new VpcRouteTableTransitRouteStack(app,'CROatxToIdentityVpcRoute', {<br>  env: envCROatx,   destinationCidr: envIdentity.desiredVpcCidr,   targetVpc: CROatxAccount.Vpc });<br>const IdentityToCROatxVpcRoute = new VpcRouteTableTransitRouteStack(app,'IdentityToCROatxVpcRoute', {<br>  env: envIdentity,   destinationCidr: envCROatx.desiredVpcCidr,   targetVpc: identityAccount.Vpc });<br>  <br>const CROatxToResearchVpcRoute = new VpcRouteTableTransitRouteStack(app,'CROatxToResearchVpcRoute', {<br>  env: envCROatx,   destinationCidr: envResearch.desiredVpcCidr,   targetVpc: CROatxAccount.Vpc });  <br>const ResearchToCROatxVpcRoute = new VpcRouteTableTransitRouteStack(app,'ResearchToCROatxVpcRoute', {<br>  env: envResearch,   destinationCidr: envCROatx.desiredVpcCidr,   targetVpc: researchAccount.Vpc });  <br><br>const CROatxTransitEnrolledAccountStack = new TransitEnrolledAccount(app,'CROatxTransitEnrolledAccountStack', {<br>  env: envTransit,<br>  AccountDescription: "CROatx",<br>  AccountToEnrollVpcCidr: envCROatx.desiredVpcCidr,<br>  targetVpcTransitSecretsArn: app.node.tryGetContext("CROatxTgAttachmentSecretArn"),<br>  transitVPCRouteTableSecretsArn: app.node.tryGetContext("transitGatewayRouteTableSecretArn"),<br>  targetVPCCidrRangeSecretsArn: app.node.tryGetContext("CROatxVpcCidrSecretArn")<br>});<br><br>/// END -  Example of how to onboard an additional account into the Blueprint<br>/////////////////////////////////////////</pre>

Now, open up the file <code>biotechblueprint\deployBiotechBlueprint.sh </code>and scroll down to the bottom.<br/>

<br/>

You will see another example section where you can uncomment the commands and find/replace the CROatx <br/>

<pre id='VHY9CAbmEHC'>#########################################################<br>### Example of how to onboard an additional account into the Blueprint<br>### ...<br><br>CROatxAcctId="$(aws sts get-caller-identity --profile CROatx --query "Account" --output text)"<br>jq '.context.envCROatxAccountId = $accountID' --arg accountID $CROatxAcctId cdk.json &gt; tmp.$$.json &amp;&amp; mv tmp.$$.json cdk.json<br>cdk deploy CROatxAccountStack --context transitGatewaySecretArn=$transitGatewayIdSecretArn --profile CROatx<br><br>CROatxGatewayAttachment="$(aws secretsmanager get-secret-value --secret-id ga --profile CROatx | grep -Po 'tgw-attach-.{17}')"<br>aws secretsmanager put-secret-value --secret-id ga --secret-string $CROatxGatewayAttachment --profile CROatx<br>CROatxTgAttachmentSecretArn="$(aws secretsmanager get-secret-value --secret-id ga --profile CROatx | grep -Po 'arn:aws:secretsmanager.*ga')"<br>CROatxVpcCidr="$(aws secretsmanager get-secret-value --secret-id vc --profile CROatx | grep -Po '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}/..')"<br>aws secretsmanager put-secret-value --secret-id vc --secret-string $CROatxVpcCidr --profile CROatx<br>CROatxVpcCidrSecretArn="$(aws secretsmanager get-secret-value --secret-id vc --profile CROatx | grep -Po 'arn:aws:secretsmanager.*vc')"<br><br>cdk deploy CROatxToTransitVpcRoute CROatxToIdentityVpcRoute CROatxToResearchVpcRoute \<br>    --context transitGatewaySecretArn=$transitGatewayIdSecretArn \<br>    --profile CROatx <br><br>cdk deploy ResearchToCROatxVpcRoute \<br>    --context transitGatewaySecretArn=$transitGatewayIdSecretArn \<br>    --profile research     <br>    <br>cdk deploy IdentityToCROatxVpcRoute \<br>    --context transitGatewaySecretArn=$transitGatewayIdSecretArn \<br>    --profile master <br>    <br>cdk deploy TransitToCROatxVpcRoute \<br>    --context transitGatewaySecretArn=$transitGatewayIdSecretArn \<br>    --profile transit     <br>    <br><br>cdk deploy CROatxTransitEnrolledAccountStack \<br>    --context identityAccountAdConnectorSecretArn=$identityAccountAdConnectorSecretArn \<br>    --context identityAccountAdConnectorSecretKeyArn=$identityAccountAdConnectorSecretKeyArn \<br>    --context transitGatewayRouteTableSecretArn=$transitGatewayRouteTableSecretArn \<br>    --context CROatxTgAttachmentSecretArn=$CROatxTgAttachmentSecretArn \<br>    --context CROatxVpcCidrSecretArn=$CROatxVpcCidrSecretArn \<br>    --profile transit         </pre>

<br/>
