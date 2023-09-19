# Deploying a centralized security model in AWS using Terraform

You will need to download all of the ".tf" files above as well as the "modules" and "bootstrap_files" directories which contain additional terraform files that are required for initiating and applying the build.

There are certainly better ways to do this, but here is an example of my working directory on my computer prior to editing anything:
![](/images/workingdir.png)

While I probably should have moved the key-pair-name to the tfvars file, I left it in "variables.tf" because it is an arbitrary name and doesn't identify me without my secret keys. Edit your variables.tf file and insert the key "Name" in the aws_variables variable field which can be found in the EC2>Network & Security>Key Pairs section:
![](/images/variableskeypair.png)

Once you pull all the files down and edit the "variables.tf" file, the next thing you will need to do is create a "terraform.tfvars" file that will contain all of the sensitive data you don't want put in your shareable code. You may edit this as you see fit, but in this working example, the only two things that I don't want out in the public are my secret keys from AWS and my public IP. Here's a snapshot of what my tfvafs file looks like:
![](/images/tfvars.png)

While there are several ways to create your access and secret keys, I opted to do it in the AWS management console followed by saving them on my local machine with awscli.

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

Once downloaded, to add your key information, in terminal (OS X) type "aws configure" which will prompt you for the key ID and secret access key respectively. Enter the information you got from the previous step after creating the keys:
![](/images/awsconfigure.png)

If you intend to bootstrap your firewalls, you may edit the firewall configs in the bootstrap_files directory (specifically the init-cfg.txt file) to change the booted config.

When you are happy with your settings, we will use terraform to initialize the deployment and pull down our OS-dependent binary, plan the deployment to validate our configurations and finally apply the configurations. Jump into your working directory for the project and type "terraform init":
![](/images/tfinit.png)

If you see that terraform was successfully initiated, you can move on to planning your deployment with "terraform plan":
![](/images/tfplan.png)

As long as there are no errors (not warnings), we can move on to apply the settings with "terraform apply". I'll spare you entering the command and only show you what you should expect to see if your resources are successfully deployed:
![](/images/tfapply.png)

If you've reached this point, congratulations! Once your firewall instances have passed both of their "status checks" per the ec2 console you can log in to the management interface of the firewalls with the user name of "paloalto" and password of "Password123!"
