# Deploying a centralized security model in AWS using Terraform

You will need to download all of the ".tf" files above as well as the modules directory which contains additional terraform files that are required for initiating and applying the build.

There are certainly better ways to do this, but here is an example of my working directory on my computer prior to editing anything:
![](/images/directory.png)

Once you pull all the files down, the first thing you will need to do is edit the "variables.tf" file and enter your EC2 key pair. You will also need the public IP you will be accessing the firewall's management interface from because it will be used in a security group rule to allow inbound traffic.

The key pair in question is not your individual user's access key but rather the key found in the EC2>Network & Security>Key Pairs section...specifically you will want to reference the key pair "Name" as seen below:
![](/images/variableskeypair.png)

While we are on this key tangent, you will also want to make sure you have an access key as well as an access key secret. While there are several ways to do this, I opted to do it in the AWS management console followed by saving them on my local machine with awscli.

To create them in the console go to the IAM console, click on the "Users" sub menu and select "Create User":
![](/images/iamcreate.png)

Give your user a name then click "Next" on the "Specify user details" step. In the "Set permissions" step, select "Attach policies directly" which will drop down a menu of existing policies. For the sake of this document, I selected the "AdministratorAccess" policy checkbox for full rights, but with great privilege comes great responsibility so please understand your organizations authorization policies:
![](/images/iampermissions.png)

Click "Next" to go to the review stage and finally select "Create user" to make a new user.

Now we need to create access keys for our user. Back at the main Users menus click on the blue hyperlink that is your newly created username which will take you to the settings page for that user. Towards the bottom of the screen select "Security Credentials":
![](/images/usersettings.png)

Scroll down in the security credentials section to the "Access keys" section and click on "Create access key". Select "Command Line Interface (CLI) as your use case, click the check box at the bottom confirming your understaning of the settings then click "Next". Finally click "Create access key" to generate your key and secret. The next page will show you your "Access key" and "Secret access key" which we will need to save for the next step to store the credentials locally using awscli:
![](/images/accesskey.png)

Now that we have our keys we will want to enter them into awscli. If you don't have awscli installed you can follow this guide then move on:

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

Once downloaded, to add your key information, in terminal (OS X) type "aws configure which will prompt you for the key ID and secret access key respectively. Enter the information you got from the previous step after creating the keys:
![](/images/awsconfigure.png)
