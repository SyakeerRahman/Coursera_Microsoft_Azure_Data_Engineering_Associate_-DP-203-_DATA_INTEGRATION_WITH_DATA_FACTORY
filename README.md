# Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY

  ## Module 1 Integrate data with Azure Data Factory or Azure Synapse Pipeline
    ### Understand Azure Data Factory
    ### Set up and manage Azure Data Factory

### 1) Example - Set-up Azure Data Factory
It is easy to set up Azure Data Factory from within the Azure portal, you only require the following information:

Name: The name of the Azure Data Factory instance
Subscription: The subscription in which the ADF instance is created
Resource group: The resource group where the ADF instance will reside
Version: select V2 for the latest features
Location: The datacenter location in which the instance is stored

Enable Git provides the capability to integrate the code that you create with a Git repository enabling you to source control the code that you would create. Define the GIT URL, repository name, branch name, and the root folder.

Creating an Azure Data Factory InstanceUI Screengrab of: Creating an Azure Data Factory Instance"
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/00d9c753-a9d4-4ab6-a541-2f1eb23d1e7b)

Alternatively, there are a number of different ways that you can provision the service programmatically. In this example, you can see PowerShell at work to set up the environment.
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

### 2) Example - Create linked services

Before you create a dataset, you must create a linked service to link your data store to the data factory. Linked services are much like connection strings, which define the connection information needed for Data Factory to connect to external resources. There are over 100 connectors that can be used to define a linked service.

A linked service in Data Factory can be defined using the Copy Data Activity in the ADF designer, or you can create them independently to point to a data store or a compute resources. The Copy Activity copies data between the source and destination, and when you run this activity you are asked to define a linked service as part of the copy activity definition

Alternatively you can programmatically define a linked service in the JSON format to be used via REST APIs or the SDK, using the following notation:

```
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
```
The following table describes properties in the above JSON:

<img width="611" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/60c297a3-2c50-4746-9d57-9a8f3842bccd">

Example of a Linked Service
Azure SQL Database

The following example creates a linked service named "AzureSqlLinkedService" that connects to an Azure SQL Database named "ctosqldb" with the userid of "ctesta-oneill" and the password of "P@ssw0rd".

```
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<server-name>.database.windows.net,1433;Database=ctosqldb;User ID=ctesta-oneill;Password=P@ssw0rd;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

Azure Blob Storage

The following example creates a linked service named "StorageLinkedService" that connects to an Azure Blob Store named "ctostorageaccount" with the storage account key used to connect to the data store.

```
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=ctostorageaccount;AccountKey=<account-key>"
    }
  }
}
```

### 3) Example - Create datasets

A dataset is a named view of data that simply points or references the data you want to use in your activities as inputs and outputs. Datasets identify data within different data stores, such as tables, files, folders, and documents. For example, an Azure Blob dataset specifies the blob container and folder in Blob storage from which the activity should read the data.
A dataset in Data Factory can be defined as an object within the Copy Data Activity, as a separate object, or in a JSON format for programmatic creation as follows:

```
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "schema": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```

The following table describes properties in the above JSON:

<img width="626" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/aaa6c494-cd4d-4b57-a017-b6bb8250840b">

 Example of a dataset
Azure Blob
In this procedure, you create two datasets: InputDataset and OutputDataset. These datasets are of type Binary. They refer to the Azure Storage linked service named AzureStorageLinkedService. The input dataset represents the source data in the input folder. In the input dataset definition, you specify the blob container (adftutorial), the folder (input), and the file (emp.txt) that contain the source data. The output dataset represents the data that's copied to the destination. In the output dataset definition, you specify the blob container (adftutorial), the folder (output), and the file to which the data is copied.
In your desktop, create a folder named ADFv2QuickStartPSH in your C drive.

Create a JSON file named InputDataset.json in the C:\ADFv2QuickStartPSH folder with the following content:

```
{
      "name": "InputDataset",
      "properties": {
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
          },
          "annotations": [],
          "type": "Binary",
          "typeProperties": {
              "location": {
                  "type": "AzureBlobStorageLocation",
                  "fileName": "emp.txt",
                  "folderPath": "input",
                  "container": "adftutorial"
              }
          }
      }
  }

  ```

To create the dataset: InputDataset, run the Set-AzDataFactoryV2Dataset cmdlet.
```
Set-AzDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName `
    -ResourceGroupName $ResGrp.ResourceGroupName -Name "InputDataset" `
    -DefinitionFile ".\InputDataset.json"
```
Here is the sample output:
```
DatasetName       : InputDataset
ResourceGroupName : <resourceGroupname>
DataFactoryName   : <dataFactoryName>
Structure         :
Properties        : Microsoft.Azure.Management.DataFactory.Models.BinaryDataset
```

Repeat the steps to create the output dataset. Create a JSON file named OutputDataset.json in the C:\ADFv2QuickStartPSH folder, with the following content:
```
{
    "name": "OutputDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "annotations": [],
        "type": "Binary",
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "folderPath": "output",
                "container": "adftutorial"
            }
        }
    }
}
```

Run the Set-AzDataFactoryV2Dataset cmdlet to create the OutDataset.
```
Set-AzDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName `
    -ResourceGroupName $ResGrp.ResourceGroupName -Name "OutputDataset" `
    -DefinitionFile ".\OutputDataset.json"
```

Here is the sample output:
```
DatasetName       : OutputDataset
ResourceGroupName : <resourceGroupname>
DataFactoryName   : <dataFactoryName>
Structure         :
Properties        : Microsoft.Azure.Management.DataFactory.Models.BinaryDataset
```

### 4) Create data factory activities and pipelines

Activities within Azure Data Factory define the actions that will be performed on the data and there are three categories including:
Data movement activities

Data transformation activities

Control activities

Data movement activities
Data movement activities simply move data from one data store to another. You can use the Copy Activity to perform data movement activities, or by using JSON. There are a wide range of data stores that are supported as a source and as a sink. This list is ever increasing, and you can find the 
latest information here
.
Data transformation activities
Data transformation activities can be performed natively within the authoring tool of Azure Data Factory using the Mapping Data Flow. Alternatively, you can call a compute resource to change or enhance data through transformation, or perform analysis of the data. These include compute technologies such as Azure Databricks, Azure Batch, SQL Database and Azure Synapse Analytics, Machine Learning Services, Azure Virtual machines and HDInsight. You can make use of any existing SQL Server Integration Services (SSIS) Packages stored in a Catalog to execute in Azure
As this list is always evolving, you can get the 
latest information here
.
Control activities
When graphically authoring ADF solutions, you can use the control flow within the design to orchestrate pipeline activities that include chaining activities in a sequence, branching, defining parameters at the pipeline level, and passing arguments while invoking the pipeline on-demand or from a trigger. The current capabilities include:

<img width="603" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/cb2ed6d4-106d-46fb-bd16-5a347b415119">

Activities and pipelines
Defining activities
When using JSON notation, the activities section can have one or more activities defined within it. There are two main types of activities: Execution and Control Activities. Execution (also known as Compute) activities include data movement and data transformation activities. They have the following top-level structure:
```
{
    "name": "Execution Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "linkedServiceName": "MyLinkedService",
    "policy":
    {
    },
    "dependsOn":
    {
    }
}
```

The following table describes properties in the above JSON:
<img width="602" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/17dff43b-7f3c-4d0c-b6c8-fa7f7ef36b9f">

Defining control activities
A Control Activity in Data Factory is defined in JSON format as follows:
```
{
    "name": "Control Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "dependsOn":
    {
    }
}
```

The following table describes properties in the above JSON:
<img width="617" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/f1b7197b-77cb-4e1f-b4f6-6e07d002caac">


Defining pipelines

Here is how a pipeline is defined in JSON format:
```
{
    "name": "PipelineName",
    "properties":
    {
        "description": "pipeline description",
        "activities":
        [
        ],
        "parameters": {
         }
    }
}
```

The following table describes properties in the above JSON:
<img width="613" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/5735f309-da02-4673-b551-b981423cf19c">

Example
The following JSON defines pipeline named "MyFirstPipeline" that contains one activity type of HDInsightHive that will call a query from a script name "partitionweblogs.hql" that is stored in the linked service named "StorageLinkedService", with an input named "AzureBlobInput" and an output named "AzureBlobOutput". It executes this against the compute resource defined in the linked service named "HDInsightOnDemandLinkedService"

The pipeline is scheduled to execute on a monthly basis, and will attempt to execute 3 times should it fail.
```
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
              },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2017-04-01T00:00:00Z",
        "end": "2017-04-02T00:00:00Z",
        "isPaused": false,
        "hubName": "ctogetstarteddf_hub",
        "pipelineMode": "Scheduled"
    }
}
```



  ## Module 2 Petabyte-scale ingestion with Azure Data Factory or Azure Synapse Pipeline
    ### Ingest source data

### 1) Example -Use the data factory copy activity - Example

Once the creation of the Data Factory instance is complete, you can go to the resource where you can begin to create your data pipelines by clicking on Azure Data factory Studio.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/5bb61d33-bea1-44c5-9289-6d6e994a72bd)

This will open the Azure Data factory home page.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/e4c314ad-cbc5-470c-9384-5054724ddc06)

The first step in your pipeline is creating a Copy Activity that copies data between the source and destination using the following steps.

1. Open the authoring canvas by clicking on the pencil icon on the left sidebar or the create pipeline button to open the authoring canvas.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/da55b787-0c45-4285-b7a7-388741a90260)

2. Create the pipeline. Click on the + button in the Factory Resources pane and select Pipeline.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/8d0210da-5197-4d5c-8d42-a06adf392b7f)

3. Add a copy activity. In the Activities pane, open the Move and Transform accordion and drag the Copy Data activity onto the pipeline canvas.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/c2a053ee-446f-42a2-a8e9-46d55c443c39)

With the Copy Activity added, you then start to define the source data.

1. In the Source tab of the Copy Activity settings, click + New to select a data source.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/e137a9e2-e6b9-4c81-ad5a-6cf53357b720)

2. For example, In the data store list, select the Amazon S3 tile and click continue.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/d01946b2-e9ca-4875-8101-d98d732b4408)

3. In the file format list, select the DelimitedText format tile and click continue
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/a041ac99-5635-443f-b8b4-3fb381d607aa)

4. In Set Properties window, give your dataset an understandable name and click on the Linked Service dropdown. If you have not created your S3 Linked Service, select New.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/b7e377ed-bce0-4755-9efa-bf2eaa294a03)

5. Specific to the S3 Linked Service configuration pane, specify your S3 access key and secret key. The Data Factory service encrypts credentials with certificates managed by Microsoft. For more information, see Data Movement Security Considerations. To verify your credentials are valid, click Test Connection. Click Create when finished
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/885f51fd-ad8c-41ec-bfd4-6a392c34849d)

6. Once you have created and selected the linked service, specify the rest of your dataset settings. These settings specify how and where in your connection you want to pull the data. Click Finish once completed.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/655f4f89-788a-40eb-8fc3-9e6c217f1476)

7. To verify your dataset is configured correctly, click Preview Data in the Source tab of the Copy Activity to get a small snapshot of your data.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/90d9b7b7-e1f9-4678-b93e-ceab153048fe)

With the source data defined, then you will define the sink into which the data will be loaded. In this example the sink will be Azure Data Lake Storage Gen2 by performing the following steps:
1. In the Sink tab, click + New
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/b55533ba-2e2e-43a7-9586-4dcad94ba09b)

2. Select the Azure Data lake Storage Gen2 tile and click continue.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/47d6eeae-ffb8-4a4e-bdb5-675cc5e8ddcc)

3. In Set Properties side nav, give your dataset an understandable name and click on the Linked Service dropdown. If you have not created your ADLS Linked Service, select New.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/d7d270ab-6cdf-4393-a29c-955fdd6cae02)

4. In the ADLS linked service configuration pane, select your authentication method and enter your credentials. In the example below, an account key and selected my storage account from the drop down.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/a65f47fe-f146-41b7-928e-a635fe1670ad)

5. Once you have configured your linked service, enter in the ADLS dataset configuration. Click finish once completed.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/8759e9ea-3ff8-417e-b3b7-0838b9064308)

At this point, you have fully configured your copy activity.
1. To test it out, click on the Debug button at the top of the pipeline canvas. This will start a pipeline debug run
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/ce6f591c-9490-4412-ac34-b5448ae80a85)

2. To monitor the progress of a pipeline debug run, click on the Output tab of the pipeline
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/bbf16890-7f3d-49f4-862d-e681f44e0ab5)

3. To view a more detailed description of the activity output, click on the eyeglasses icon. This will open up the copy monitoring screen which provides useful metrics such as Data read/written, throughput, and in-depth duration statistics.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/cb62fa19-64fd-4a84-b1f7-94282493085e)

### 2) Example - Manage the self-hosted integration runtime

In Data Factory, an activity defines the action to be performed. A linked service defines a target data store or a compute service. An integration runtime provides the bridge between the activity and linked services.

Self-hosted integration runtime
A self-hosted integration runtime is capable of:
1. Running copy activity between a cloud data store and a data store in the private network.
2. Dispatching the following transform activities against compute resources in on-premises or Azure Virtual Network:
HDInsight Hive activity (BYOC-Bring Your Own Cluster)

HDInsight Pig activity (BYOC)
HDInsight MapReduce activity (BYOC)
HDInsight Spark activity (BYOC)
HDInsight Streaming activity (BYOC)
Machine Learning Batch Execution activity
Machine Learning Update Resource activities
Stored Procedure activity
Data Lake Analytics U-SQL activity
Custom activity (runs on Azure Batch)
Lookup activity
Get Metadata activity

The self-hosted integration runtime is logically registered to the Azure Data Factory and the compute resource used to support its function as provided by you. Therefore there is no explicit location property for self-hosted IR. When used to perform data movement, the self-hosted IR extracts data from the source and writes it into the destination.

Create a self-hosted Integration Runtime within Azure Data Factorty
1, On the Let's get started page of Azure Data Factory UI, select the Author tab on the leftmost pane.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/181def47-2581-4b56-a454-5a9894b2ab5d)

2. Select Manage in the leftmost pane, and select Integration runtimes. Select +New.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/233b31af-ddae-45ed-bdc9-eb81be0d5bfe)

3. On the Integration runtime setup page, select Azure, Self-Hosted, and then select Continue.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/80a8d470-f48a-47a1-98aa-5baca8b23d19)

4. On the Integration runtime setup page, type in a name of MySelfHostedIR, and click Create
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/ababf5ac-159e-43f4-b657-f539eacea26c)

5. Copy and paste the authentication key. Select Download and install integration runtime.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/7d262d8f-7252-4dd5-b374-614b8ed43a0e)

6. Download the self-hosted integration runtime on a local Windows machine. Run the installer.

7. On the Register Integration Runtime (Self-hosted) page, paste the key you saved earlier, and select Register.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/47a524c5-f709-43ff-8b7c-f82e1435d76f)

8. On the New Integration Runtime (Self-hosted) Node page, select Finish.

9. After the self-hosted integration runtime is registered successfully, you see the following window:
Create 9
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/3d09f1f4-6a63-4dba-a579-2f711d839383)
 
Automated deployments
You can also set up a self-hosted IR on an Azure VM via an Azure Resource Manager template, or by using PowerShell.
1. Run the following command in PowerShell
```
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
```

2. Download and install the self-hosted integration runtime on a local machine.

3. Retrieve the authentication key and register the self-hosted integration runtime with the key. Here is a PowerShell example:
```
Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName 
```


### 3) Set up the Azure integration runtime

In Data Factory, an activity defines the action to be performed. A linked service defines a target data store or a compute service. An integration runtime provides the bridge between the activity and linked services.

Azure integration runtime
An Azure integration runtime is capable of:

Running Data Flows in Azure

Running Copy Activity between cloud data stores

Dispatching the following transform activities in public network: Databricks Notebook/ Jar/ Python activity, HDInsight Hive activity, HDInsight Pig activity, HDInsight MapReduce activity, HDInsight Spark activity, HDInsight Streaming activity, Machine Learning Batch Execution activity, Machine Learning Update Resource activities, Stored Procedure activity, Data Lake Analytics U-SQL activity, .NET custom activity, Web activity, Lookup activity, and Get Metadata activity.

You can set a certain location of an Azure IR, in which case the data movement or activity dispatch will happen in that specific region. If you choose to use the auto-resolve Azure IR which is the default, ADF will make a best effort to automatically detect your sink and source data store to choose the best location either in the same region if available or the closest one in the same geography for the Copy Activity. For anything else, it will use the IR in the Data Factory region. Azure Integration Runtime also has support for virtual networks.

Create and configure Azure integration runtime
Use the following steps to create an Azure IR using Azure Data Factory UI.

1. On the Let's get started page of Azure Data Factory UI, select the Manage tab from the leftmost pane
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/429598d1-746c-416b-bad6-bdc4eb87982b)

2. Select Integration runtimes on the left pane, and then select +New.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/8cc980ec-90b8-4e6e-b094-f2f92eb2d18f)

3. On the Integration runtime setup page, select Azure, Self-Hosted, and then select Continue.
4. On the following page, select Azure to create an Azure IR, and then select Continue.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/93e8177c-6606-4cb9-974a-78f86c3fa9fa)

5. Enter a name for your Azure IR, and select Create.
![image](https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/d9936b2f-4094-481a-bb57-a73e02d0cd18)

You'll see a pop-up notification when the creation completes. On the Integration runtimes page, make sure that you see the newly created IR in the list.

Automated deployments
You can also set up Azure IR via an Azure Resource Manager template, or by using PowerShell.
1. Run the following command in PowerShell
```
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```
When Azure IR, the type must be set to Managed. You do not need to specify compute details because it is fully managed elastically in cloud.

   
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

<img width="374" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/db9824c8-4c44-43da-921d-429a86f5455a">
<img width="374" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/818fa211-0e5d-4126-ad6c-76ed824c8937">
<img width="259" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/3f820e92-8607-47af-8bc4-ae893135e91e">
<img width="391" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/5e83c861-aad0-4865-83bd-7a2ee86ef02e">
<img width="485" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/e1642e6d-539c-4d37-9c2d-299219bbed95">
<img width="482" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/b97fdf45-2896-4309-b42b-595bfdd2fd3c">
<img width="379" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/daea3e29-db80-4e0b-946d-30a23378f790">
<img width="371" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/6e210cfb-5b75-4a2a-b51b-149daef56546">
<img width="397" alt="image" src="https://github.com/SyakeerRahman/Coursera_Microsoft_Azure_Data_Engineering_Associate_-DP-203-_DATA_INTEGRATION_WITH_DATA_FACTORY/assets/105381652/940c3557-1a00-45b4-88b0-62c11a3e9352">




