# My Azure Test Sandbox 
### How it works

This is a work around to manually trigger the SQS deployment.  It requires the Github action run ID 
of the application build.  Edit the **runinfo.json** json file with the appropriate values
```
{
    "runid": 123599725,  <--- EDIT THIS --->
    "vmhost": "sqsvmdev",
    "resource_group": "rg-sqs-rsm-mrs-nonprod",
    "az_storage": "sqsstononprod"
    "az_container": "deploy"
}
```
 
- **runid** - SQS app build Github runid.  You can get this in the Github Actions of the SQS build workflow
- **vmhost** - Azure VM host target to deploy the binaries
- **az_storage** - Azure storage account to hold the custom script
- **az_container** - Azure storage container to hold the custom script
