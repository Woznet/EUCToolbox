# EUCToolbox
# Instructions for Use

# Scripted Deployment

First you will need an app-reg with the appropriate permissions which can be created by running "create-app-reg.ps1"
[create-app-reg.ps1](https://raw.githubusercontent.com/EUCToolbox/EUCToolbox/main/App-Deployment/Install%20Scripts/create-app-reg.ps1)
Make a note of the client ID and secret, you will need these later

Second you want to deploy the resources to Azure by clicking this link:
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FEUCToolbox%2FEUCToolbox%2Fmain%2FApp-Deployment%2FInstall%2520Scripts%2Farm-template.json)


Fill in the required details and it will create runbooks and an app service with the web page content.
The runbook script content will be automatically populated, but if you want to start with a forked version, update accordingly
When it is complete, click on Outputs and make a note of the details.
![alt text](https://euctoolbox.com/images/outputs-image.jpg)
Finally, navigate to your new app service website and you will be prompted to enter the details recorded earlier.

Add these and you are now up and running

# Manual Deployment

App Reg:
1) Create a new App Registration (multi-tenant if required) and make a note of the Application (client) ID
2) Add the redirect URIs to Microsoft default values:
- https://login.microsoftonline.com/common/oauth2/nativeclient
- https://login.live.com/oauth20_desktop.srf
- msal2afd3959-6ff7-400b-8cb3-4c4828166bf1://auth
3) Add these API permissions:
- DeviceManagementApps.ReadWrite.All - Application Type
- DeviceManagementConfiguration.ReadWrite.All - Application Type
- Group.ReadWrite.All - Application Type
- GroupMember.ReadWrite.All - Application Type
- Organization.Read.All - Application Type
4) Click on Certificates and secrets and create a new secret.  Make a note of the secret value

Runbooks:
1) Create a new automation account and add these modules to it:
- SvRooij.ContentPrep.Cmdlet
- powershell-yaml
- Microsoft.Graph.Groups
- Microsoft.Graph.DeviceManagement
- Microsoft.Graph.Authentication
- IntuneWin32App
2) Create a runbook for community app deployments, run on Azure, PowerShell v5.1
3) Paste this script into the content:
[app-deploy-runbook.ps1](https://raw.githubusercontent.com/EUCToolbox/EUCToolbox/main/App-Deployment/Runbook%20Script/app-deploy-runbook.ps1)
4) Publish it
5) Click Webhooks and create a new webhook.  Don't populate any of the fields, they are passed from the application itself.
6) Make a note of the URI, it will not display again after leaving the page.
7) Create a second runbook following the same process for this script:
[app-deploy-runbook-manifest.ps1](https://raw.githubusercontent.com/EUCToolbox/EUCToolbox/main/App-Deployment/Runbook%20Script/app-deploy-runbook-manifest.ps1)


Web Service
1) Create an Azure App Service running PHP (latest version), or you can use any other web hosting facilities
2) Copy the contents of this directory into the root:
[Webpage Content](https://github.com/EUCToolbox/EUCToolbox/tree/main/App-Deployment/Webpage%20Content)
3) Navigate to the new URL and populate the fields