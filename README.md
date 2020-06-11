# SQS Trigger Manual Deploy  
### How it works

This is a work around to manually trigger the SQS deployment.  It requires the Github action artifacts run ID 
of the SQS application build.  Edit the **runinfo.json** json file with the appropriate values
```
{
    "runid": 123599725,  <--- This is the github runid shown in the artifacts --->
    "vmhost": "sqsvmdev",
    "resource_group": "rg-sqs-rsm-mrs-nonprod",
    "az_storage": "sqsstononprod",
    "az_container": "deploy"
}
```
 
|JSON Element|Explanation|
|:--------------|:-------------- |
| **runid** |SQS app Gitbub build artifacts runid.  You can get this in the Github Actions of the SQS build workflow|
| **vmhost** |Azure VM host target to deploy the application|
| **resource_group** | Azure resource group name |
| **az_storage** |Azure storage account to store the custom scripts|
| **az_container** |Azure storage container to hold the custom scripts|

 Commit and push the change to *master* branch to trigger the action 

### Configure Secrets
The deployment requires access to Azure storage account and GitHub repository.  GitHub Secrets that needed to be defined are:

- **ARTIFACTS_TOKEN** - This is a PAT Github token
- **AZ_CREDS** - This is the Azure SPN to access Azure.  Store the Azure SPN as:

```
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>"
}
```

### The Process
The deployment are performed by running it as a CustomScript in the target Azure VM.  
1. Github actions workflow generates the scripts and uploads it to the Azure Storage. 
2. The scripts generated are ```DeployCustomScripts.ps1``` and ```customscript.ps1```.  These scripts are uploaded to Azure storage container as defined in the json element above (e.g. **deploy**)
3. ```customscript.ps1``` initiates the VM CustomScript extension call against the target Azure VM.  
4. ```DeployCustomScripts.ps1``` is the deployment script.  It is generated specifically to deploy the SQS application.
