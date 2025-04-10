# Exercise 1: Build an Open AI Pipeline to Ingest Batch Data, Perform Intelligent Operations, and Analyze in Synapse

### Estimated Duration: 90 minutes

This lab aims to demonstrate how to integrate OpenAI capabilities into Azure Synapse Analytics for processing batch data, performing intelligent operations like natural language processing or predictive analytics, and analyzing the results efficiently within the Synapse environment.

## Lab Objectives

You will be able to complete the following tasks:

- Task 1: Ingest Data to Storage account
- Task 2: Set up Synapse Workspace
- Task 3: Query Results in Our SQL Table

## Task 1: Ingest Data to Storage account

### A. Launch Azure Cloud Shell

1. In the [Azure portal](https://portal.azure.com?azure-portal=true), select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. A Cloud Shell pane will open at the bottom of the portal.

    ![](images/E2I1S1.png)

2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **Bash**. If you don't see this option, skip the step.

3. Within the Getting Started pane, select **Mount storage account (1)**, select your **Storage account subscription (2)** from the dropdown and click **Apply (3)**.

   ![](images/10-06-2024(1).png)

4. Within the **Mount storage account** pane, select **Select existing storage account (1)** and click **Next (2)**.

   ![](images/10-06-2024(2).png)

5. Within the **Advanced settings** pane, enter the following details:

    - **Subscription**: Default- Choose the only existing subscription assigned for this lab (1).
    - **Resource group**: Select **Use existing** **(2)**
      - openai-<inject key="DeploymentID" enableCopy="false"></inject>
    - **Storage account**: Select **Use existing** **(3)**
      - openaistorage<inject key="DeploymentID" enableCopy="false"></inject>
    - **File share**: Create a new file share **(4)**

      ![](images/10-06-2024(3).png)

6. Enter the file share name as **blob (1)**, select **OK**, and click on **Select (2)**.

    ![](images/10-06-2024(4).png)

7.  Once the storage account is created, you will be prompted with the Bash window, as shown in the below screenshot.
    
    ![](images/cloudshell.png)

    > **Note**: Please disregard the warning `the storage file share subscription ID is not registered to Microsoft.CloudShell Namespace` and proceed with the next task.

### B. Upload files to a storage account:

1. Run the following commands in the cloud shell to download and install Miniconda.

     ```bash 
     wget https://repo.anaconda.com/miniconda/Miniconda3-py39_23.1.0-1-Linux-x86_64.sh 
     ```

     ```bash 
     sh Miniconda3-py39_23.1.0-1-Linux-x86_64.sh 
     ```
    
    > **Note:** The following commands are issued in Bash; please ensure you are using **Bash** in the Cloud Shell.

1. Press `Enter` to continue with the installation process.
    
      > **Note:** Press the down arrow key to read or skip the license agreement.

      ![](images/pro1.png)

1. Type **yes** and hit **enter** to accept the agreement, and then hit enter to install on the default path.

   ![](images/pro2.png)

1. Type **yes** and hit **enter** to initialize the conda environment.

    ![](images/E2T1PBS3.png)

1. Run the following command to store the miniconda installed path to the variable.

    ```bash 
    export PATH=~/miniconda3/bin:$PATH
    ```
1. Run the below commands one by one to create and activate the conda environment in Cloudshell.

    ```bash 
    git clone https://github.com/microsoft/OpenAIWorkshop.git
    cd OpenAIWorkshop/scenarios/openai_batch_pipeline/document_generation
    conda create -n document-creation
    conda activate document-creation
    pip install --upgrade pip
    pip install -r reqs.txt
    ```
    > **Note**: If you encounter the "Conda: command not found" error, close your current CloudShell session, start a new bash session, and try running the commands from step 6 again.
    
1. Type **y** and hit enter to proceed.

1. In the [Azure portal](https://portal.azure.com), navigate to your Storage Account with the suffix `functions` resource by selecting the **openai-<inject key="DeploymentID" enableCopy="false"/>** resource group and selecting the Storage Account from the list of resources.

    ![](images/storage-functions-new.png)
    
1. Switch to the **Access keys (1)** blade and select **Show (2)**, which is next to the Connection String value. Select the copy button for the first **connection string (3)**. Paste the value into a text editor, such as Notepad.exe, for later reference.

   ![](images/storage-fuctions2.png)

1. Go back to the cloud shell bash session and run the below command to upload the JSON files to the storage account by replacing the **CONNECTION_STRING** you copied in the previous step. This will take a few minutes to complete.

    ```bash 
    python upload_docs.py --conn_string "<CONNECTION_STRING>"
    ```

   ![](images/batch_file_upload2.png)

   > **Note**: Execute "cd OpenAIWorkshop scenarios/openai_batch_pipeline/document_generation" if you are not inside OpenAIWorkshop/scenarios/openai_batch_pipeline/document_generation directory.
   
1. Once you have successfully uploaded the JSON files to the storage account, navigate to **Containers (1)**, click on **workshop-data (2)** and select **cleansed_documents (3)** folder and verify that the files have been uploaded.
   
   ![](images/pro3.png)

   ![](images/pro4.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="ef9a8088-8af2-426f-9672-70c0b0c227b5" />

## Task 2: Set up Synapse Workspace

### **A. Setup Synape Workspace**

1. In Azure portal, search for **Synapse** and select **Azure Synapse Analytics**.

   ![](images/image(1).png)

2. On the **Azure Synapse Analytics** page, Click on **+ Create**.
3. You will be navigated to **Create Synapse Analytics** page where you will be configuring the synapse workspace.
4. On the Basics tab provide the following details :

   - **Subscription** : Use **Existing Subscription(1)**.
   - **Resource Group** : Use **openai-<inject key="DeploymentID" enableCopy="false"></inject>(2)**
   - **Workspace name** : **synapseworkspace<inject key="DeploymentID" enableCopy="false"></inject>(3)**
   - **Region** : Select Default region(4)
   - **Select Data Lake Storage Gen2** : Select **From Subscription(5)**
   - **Account name** : **asadatalake<inject key="DeploymentID" enableCopy="false"></inject>(6)**
   - **File system name** : **defaultfs(7)**
   - Click on **Next : Security>(8)**

     ![](images/image(2).png)

5. On the **Security** tab, ensure that the Authentication method is set to **Use both local and Microsoft Entra ID authentication** and click on **Next: Networking**

   ![](images/image(3).png)

6. On the networking tab, make sure Managed virtual network is **Disable(1)** and **Allow connections from all IP addresses(2)** is checked then click on **Review + create** and **Create** to deploy the resource.

   ![](images/image(4).png)
   
7. Once the resource is deployed click on **Go to resource group**

   ![](images/image(5).png)

8. Navigate to the synapse workspace which you have created, on the **Overview** page, click on **Open** under Open Synapse Studio to navigate to Azure Synapse Studio.
   
   ![](images/open-synapse-studio.png)

9. In the Azure Synapse Studio, navigate to **Manage > SQL pools** and then click on **+New**.

   ![](images/new-sql-pool.png)
   

10. On the Basics tab of New dedicated SQL pool provide the following details:

    - **Dedicated SQL pool name** : **openaisql01 (1)**
    - **Performance level** : Reduce it to **DW100c (2)**
    - Click on **Review + create (3)**
   
      ![](images/sql-pool-name.png)
      
11. On **Review + create** page, click on **Create** and wait for the deployment to complete.

    ![](images/sql-pool-create.png)

12. Now navigate back to Azure portal, **synapseworkspace<inject key="DeploymentID" enableCopy="false"></inject>**, select **Apache Spark pools (1)** from left pane under Analytics pools and click on **+ New (2)**.

    ![](images/image(01-1).png)  

13. On New Apache Spark pool page, provide the following details and click on **Review + create (6)** then **Create** on the Review + Create tab to create the Apache Spark pool.

    - **Apache Spark pool name** : **openaisparkpool (1)**
    - **Isolated Compute** : **Disabled (2)**
    - **Node size** : **Small(4vCores/32GB) (3)**
    - **Autoscale** : **Disabled (4)**
    - **Number of nodes** : **3 (5)**

      ![](images/image(02-22).png)
   
### **B. Create Target SQL Table**

1. In the [Azure portal](https://portal.azure.com), navigate to **synapseworkspace<inject key="DeploymentID" enableCopy="false"/>** synapse workspace from **openai-<inject key="DeploymentID" enableCopy="false"/>** resource group. From the **Overview** tab, click on **Open** to launch the Synapse workspace.

      ![](images/image(9).png)

1. Click into the **Develop (1)** section of the Synapse Studio, click the **+ (2)** sign in the top left, and select **SQL script (3)**. This will open a new window with a SQL script editor. 

      ![](images/synapse3.png)

1. Copy and paste the following script into the editor **(1)**, then change the **Connect to** value by selecting **openaisql01(2)** from the drop-down, and for **Use database**, ensure that **openaisql01(3)** is selected, and click the **Run (4)** button in the top-left corner, as shown in the picture below. Finish this step by pressing **Publish all (5)** just above the **Run** button to publish our work thus far.

    ```SQL 
    CREATE TABLE [dbo].[cs_detail]
    (
    interaction_summary varchar(8000),
    sentiment varchar(500),
    topic varchar(500),
    product varchar(500),
    filename varchar(500)
    )
    ```
    
      ![](images/openai-61.png)
    
1. Next, click on **Publish** to publish the SQL script.

      ![](images/publish-sqlscript.png)

### **C. Create Source and Target Linked Services**

We'll next need to create two linked services: one for our source (the JSON files in the Data Lake) and another for the Synapse SQL Database that houses the table we created in the previous step.

1. Click back into the **Manage (1)** section of the Synapse Studio and click the **Linked services (2)** option under the **External connections** section. Then click **+ New (3)** in the top-left.

      ![](images/synapse5.png)
   
1. Start by creating the linked services for the source of our data, the JSON files housed in the ADLS Gen2 storage we created with our initial template. In the search bar that opens after you click New, search for **blob (1)**, select **Azure Blob Storage (2)** as depicted below, and click on **Continue (3)**.

      ![](images/synapse6.png)

1. Provide the name for your linked service as **openailinkedservice (1)**. Change the **Authentication type** to **Account key (2)**. Then select the **subscription (3)** you have been working in, and finally select the storage account with the name **openaistorage<inject key="DeploymentID" enableCopy="false"></inject>** **(4)** that you created in the initial template and loaded the JSON files into then click on **Test connection (5)**. Once the connection is successful, click the **Create (6)** button in blue on the bottom left of the New linked service window.

      ![](images/img-6a.png)

1. Click **+ New** in the top-left. Search for **Synapse (1)**, select **Azure Synapse Analytics (2)**, and click on **Continue (3)**.

      ![](images/synapse8.png)

1. In the *New linked service* window that opens, fill in a name for your target linked service as **synapselinkedservice** **(1)**. Select **1.0 (2)** for the version. Select the **Azure subscription (3)** for which you have been working. Select the **synapseworkspace<inject key="DeploymentID" enableCopy="false"/> (4)** for **Server name** and **openaisql01 (5)** as the **Database name**. Be certain to change the **Authentication type** to **System-assigned managed identity (6)**, then scroll down and click on **Test connection (7)** and click **Create (8)**.

      ![](images/synapse1.png)

      ![](images/newlinkedservice1.png)

1. Once you have created the two linked services, be certain to press the **Publish all** button at the top to publish our work. Finalize the creation of the linked services and click **Publish**.

      ![](images/publish-linked.png)
   
### **D. Create Synapse Data Flow**

While still within the Synapse Studio, we will now need to create a **Data flow** to ingest our JSON data and write it to our SQL database. For this workshop, this will be a very simple data flow that ingests the data, renames some columns, and writes it back out to the target table.

1. First, we'll want to go back to the **Develop (1)** tab, select **+ (2)**, and then **Data flow (3)**.

      ![](images/synapse11.png)
   
1. Once the data flow editor opens, click **Add Source**. A new window will open at the bottom of the screen. Select **+ New** on the **Dataset** row while leaving the other options as default.

      ![](images/synapse12.png)

1. A new window should open on the right side of your screen. Next, search for **Azure Blob Storage (1)**, select **Azure Blob Storage (2)**, and then click on **Continue (3)**.
   
      ![](images/synapse13-1.png)

1. Next, select the **JSON (1)** option as our incoming data is in JSON format and click **Continue (2)**.

      ![](images/synapse14-1.png)

1. Select the Linked Service with the name **openailinkedservice (1)** we just set up in the steps above. You will need to select the proper **File path** to select the directory where our JSON files are stored. It should be something to the effect of **workshop-data / cleansed_documents (2)**. Click on the **OK** button to close the window.

      ![](images/synapse15.png)
   
1. Next, we'll need to move to the **Source options (1)** panel and drop down the **JSON settings (2)** options. We need to change the **Document form** option to the **Array of documents (3)** setting. This allows our flow to read each .JSON file as a separate entry into our database.

      ![](images/synapse16.png)   

1. Enable the toggle **data flow debug** session located at the top menu bar adjacent to the validate button, and click on **OK** on the *Turn on data flow debug* pop-up window.

      >**Note:** It will take a minute or two for the **data flow debug** session to get enabled.

1. Now head to the **Data preview** tab and run a preview to check your work thus far.
    
      ![](images/dataflow-datapreview.png)
      
      >**Note**: Click the collapse button in the left tab to clearly view the data preview option, making the preview easier to see.
      >**Note**: If you're unable to view data under the Data Preview tab, please click on the Refresh button repeatedly until the data appears.
   
1. Next, we can add in our **Select** tile and do our minor alterations before writing the data out to the Synapse SQL table. To begin, click the small **+ (1)** sign next to our ingestion tile and choose the **Select (2)** option.

      ![](images/synapse17.png)

1. We can leave all the settings as defaults. Next, we'll add in our **Sink** tile. This is the step that will write our data out to our Synapse SQL database. Click on the small **+ (1)** sign next to our **Select** tile. Scroll all the way to the bottom of the options menu and select the **Sink (2)** option.

      ![](images/synapse18.png)

1. Once the **Sink (1)** tile opens, choose **Inline (2)** for the *Sink type*. Then select **Azure Synapse Analytics (3)** for the *Inline dataset type*, and for the **Linked service**, select **synapselinkedservice (4)**, which was created in the previous step. Ensure to run **Test connection (5)** for the linked service.

      ![](images/sink-1.png)

      > **Note**: If the test connection takes more than 3–4 minutes, follow the below steps.

      - Click on **Edit**.

           ![](images/p18.png)    

      - In the Edit linked service window that opens, select the Azure selection method as **From Azure subscription** **(1)**. Select the **Azure subscription (2)** for which you have been working. Select the **synapseworkspace<inject key="DeploymentID" enableCopy="false"/> (3)** for **Server name** and **openaisql (4)** as the **Database name**, and then click on **Test connection (5)** and click **Save (6)**.

           ![](images/newlinked1.png)
           
           ![](images/new2.png)

1. We will then need to head over to the **Settings (1)** tab and adjust the **Schema name** and **Table name**. If you utilized the script provided earlier to make the target table, the Schema name is **dbo (1)** and the Table name is **cs_detail (2)**.

      ![](images/synapse20.png)

1. Before we finish our work on the data flow, we should preview our data. Previewing our data reveals we only have 3 columns when we are expecting a total of 5. We have lost our Summary and Sentiment columns.

      ![](images/data-preview.png)

1. To correct this, let's use our **Select (1)** tile to change the names as follows to get the expected output values:

    - **summary**: `interaction_summary` **(2)**
    - **customerSentiment**: `sentiment` **(3)**

         ![](images/select-1.png)
    
1. If we return to our **Sink (1)** tile and under **Data preview (2)** click **Refresh (3)**, we will now see our expected 5 columns of output.

      ![](images/refresh-sink-1.png)

1. Once you have reviewed the data and are satisfied that all columns are mapped successfully (you should have 5 columns total, all showing data in a string format), we can press **Publish all** at the top to save our current configuration. A window will open on the right side of the screen; press the blue **Publish** button at the bottom left of it to save your changes.

      ![](images/publish-dataflow.png)

1. Your completed and saved Data flow will look like the following:

      ![](images/completed-dataflow.png)

### **E. Create Synapse Pipeline**

1. Once we have created our **Data flow**, we will need to set up a **Pipeline** to house it. To create a **Pipeline**, navigate to the left-hand menu bar and choose the **Integrate (1)** option. Then click the **+ (2)** at the top of the Integrate menu to **Add a new resource** and choose **Pipeline (3)**.

      ![](images/new-pipeline-1.png)

2. Next, we need to add a **Data flow** to our Pipeline. With your new **Pipeline tab (1)** open, go to the **Activities** section and search for `data` **(2)** and select **Data flow (3)** activity and **drag-and-drop (4)** it into your Pipeline.

      ![](images/data-drag-1.png)

3. Under the **Settings (1)** tab of the **Data flow**, select the **Data flow (2)** drop-down menu and select the name of the data flow you created in the previous step. 
Then expand the **Staging (3)** section at the bottom of the settings and utilize the drop-down menu for the **Staging linked service**. Choose the linked service you created **openailinkedservice (4)** to ensure the **Test connection (5)**. Next, set a **Staging storage folder** at the very bottom and enter **workshop-data/Staging** **(6)**.

      ![](images/staging-1.png)

4. Then click **Publish all** to publish your changes and save your progress, press the blue **Publish** button at the bottom left of it to save your changes.

### **F. Trigger Synapse Pipeline**

1. Once you have successfully published your work, we need to trigger our pipeline. To do this, just below the tabs at the top of the studio, there is a *lightning bolt* icon that says **Add trigger (1)**. Click to add a trigger and select **Trigger now (2)** to begin a pipeline run then when the window opens up click on **OK**.

      ![](images/trigger-1.png)
    
2. To look at the pipeline run, navigate to the left-hand side of the screen and choose the **Monitor (1)** option. Then select the **Pipeline runs (2)** option in the **Integration** section. You will then see the pipeline run that you have triggered under the **Triggered (3)** section as **Pipeline 1 (4)**. This pipeline should take approximately 4 minutes (if you are using the uploaded data for the workshop).

      ![](images/pipeline-run-1.png)



> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

  <validation step="3e716f01-6da8-431e-aa81-4041b9054091" />

## Task 3: Query Results in Our SQL Table

1. Ensure that your pipeline run status has **Succeeded**.

      ![](images/pipline-succeeded.png)

2. Now that the data is in the target table, it is available for usage by running SQL queries against it or connecting PowerBI and creating visualizations. Upload some of the transcript files to the generated_documents folder in your container and see how it creates a new file in the cleansed_documents file.

3. To query the new data, navigate to the menu on the left-hand side, and choose **Develop (1)**. Click on the existing **SQL Script (2)** and replace the content with the **SQL Code (3)** below. Then select **openaisql01 (4)** pool **Run (5)**. 

     ```SQL 
    SELECT sentiment, count(*) as "Sum of Sentiment"
    FROM [dbo].[cs_detail]
    GROUP BY sentiment
    ORDER BY count(*) desc     
     ```

    - Your query results, if you are using the files uploaded as part of this repository or the workshop, you should see similar **Results (6)** to those below.

         ![](images/pro61.png)

## Summary

In this lab, you have ingested the data to a Storage account, set up Synapse Workspace and produced Query Results in Our SQL Table.

### You have successfully completed the lab. Click on **Next >>** to procced with next exercise.
