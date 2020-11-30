# SAP HANA VM deployments using Azure Marketplace images

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmimergel%2Fsap-app-server%2Fmain%2Fazuredeploy.json) 

This template takes a minimum amount of parameters and deploys a VM that can be used as SAP Application Server, using the latest patched version of the selected operating system. This template uses Premium Storage with Managed Disks. Filesystems are created via custom script. 

<table>
	<tr>
		<th>Size</th>
		<th>SAP VM</th>
		<th>VM Storage (OS + EXE)</th>
	</tr>
	<tr>
		<th>Small</th>
		<td>E4as_v4 (4vCPU, 32GiB, 6.044 SAPS)</td>
		<td>1xP6 (64 GB) + 1xP6 (64 GB)</td>
	</tr>
	<tr>
		<th>Medium</th>
		<td>E8as_v4 (8vCPU, 64GiB, 12.088 SAPS)</td>
		<td>1xP6 (64 GB) + 1xP6 (64 GB)</td>
	</tr>
	<tr>
		<th>Large</th>
		<td>E16as_v4 (16vCPU, 128GiB, 24.175 SAPS)</td>
		<td>1xP10 (128 GB) + 1xP10 (128 GB)</td>
	</tr>
</table>


# Deployment via Azure DevOps
Steps:
1. Fork this repository 
2. Connect Azure DevOps with your forked repository (https://docs.microsoft.com/en-us/azure/devops/boards/github/connect-to-github?view=azure-devops)
3. Create a pipeline similar to the example in this repository (azure-pipelines.yml) by adapting to your Azure envrionment
4. Enter required variables to the pipeline configuration

# Deployments in an Azure environment without internet access 
There might be multiple solutions to handle this situation which is most common for SAP environments. 
The challenge here is that the deployed HANA VM has no access to Github to download the diskConfig.sh script. 
Furthermore you might want to keep the provided details like subscription and subnetId from the azuredeployparamfile.json file private. 
For me the following solution works fine:

1. Create a storage account with a private endpoint on selected networks (SAP subnets) in your Azure subscription
2. Create a container with read access in this storage account 
3. Upload the diskConfig.sh file into the container
4. Get the URL and update the link to diskConfig.sh in azuredeploy.json of your forked repository
5. Preferable let the pipeline only run manually to avoid automatic deployments during every repository change
