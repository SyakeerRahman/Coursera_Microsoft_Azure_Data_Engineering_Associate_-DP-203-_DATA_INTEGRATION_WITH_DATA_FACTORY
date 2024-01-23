# Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY

  ## Module 1 Integrate data with Azure Data Factory or Azure Synapse Pipeline
    ### Understand Azure Data Factory
    ### Set up and manage Azure Data Factory

It is easy to set up Azure Data Factory from within the Azure portal, you only require the following information:

Name: The name of the Azure Data Factory instance
Subscription: The subscription in which the ADF instance is created
Resource group: The resource group where the ADF instance will reside
Version: select V2 for the latest features
Location: The datacenter location in which the instance is stored

Enable Git provides the capability to integrate the code that you create with a Git repository enabling you to source control the code that you would create. Define the GIT URL, repository name, branch name, and the root folder.

Creating an Azure Data Factory InstanceUI Screengrab of: Creating an Azure Data Factory Instance"
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/00d9c753-a9d4-4ab6-a541-2f1eb23d1e7b)

```
PowerShell
######################################################################
##                PART I: Creating an Azure Data Factory            ##
######################################################################


# Sign in to Azure and set the WINDOWS AZURE subscription to work with
$SubscriptionId = "add your subscription in the quotes"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# register the Microsoft Azure Data Factory resource provider
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory

# DEFINE RESOURCE GROUP NAME AND LOCATION PARAMETERS
$resourceGroupName = "cto_ignite"
$rglocation = "West US 2"

# CREATE AZURE DATA FACTORY
New-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Name "ctoigniteADF" -Location $rglocation
```
Alternatively, there are a number of different ways that you can provision the service programmatically. In this example, you can see PowerShell at work to set up the environment.
    
  ## Module 2 Petabyte-scale ingestion with Azure Data Factory or Azure Synapse Pipeline
    ### Ingest source data
    
  ## Module 3 Perform code-free transformation at scale with Azure Data Factory or Azure Synapse Pipeline
    ### Lesson introduction
    ### Populate slowly changing dimensions in Azure Synapse Analytics pipelines
  
  ## Module 4 Orchestrate data movement and transformation in Azure Data Factory or Azure Synapse Pipeline
    ### Orchestrate data movement and transformations
  
  ## Module 5 Execute existing SSIS packages in Azure Data Factory or Azure Synapse Pipeline
    ### SQL Server Integration Services (SSIS)
  
  ## Module 6 Operationalize your Azure Data Factory or Azure Synapse Pipeline
    ### Use Azure Data Factory to build your data integration solutions
    
  ## Module 7 Receive data with Azure Data Share and transforming with Azure Data Factory
    ### Ingest data from Azure Data Share into Azure Data Factory pipelines
    ### Join and transform shared and owned data
    
  ## Module 8 Course Practice Exam
    ### Practice Exam on Data integration at scale with Azure Data Factory or Azure Synapse Pipeline
    ### Course wrap up
