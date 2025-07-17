# Exercise 2: Build an Open AI application with Power App 

### Estimated Duration: 90 Minutes

This Exercise focuses on creating an application using OpenAI capabilities integrated with Microsoft Power Apps. Participants will learn how to build a functional application that leverages AI for tasks such as natural language understanding, data analysis, or customer interaction within the Power Apps framework.

1. Open a new tab and navigate to https://make.powerapps.com/. On **Welcome to Power Apps**, select your **Country/Region (1)** and click **Get Started (2)**. 

   ![](./images/welcome-1.png)
    
2. Select **Apps (1)** on the left navigation, click on **Import app (2)** dropdown and select **From package (.zip) (3)**. 

    ![](./images/E2S2.png)

3. On the **Import package** page, click on **Upload**.

    ![](./images/E2S3.png)

4. Navigate to `C:\labfile\OpenAIWorkshop-main\scenarios\powerapp_and_python\powerapp` Select the **OpenAI-Playground_20230302010547 (1)** zip folder and click on **Open (2)**.

     ![](./images/openai-play.png)

5. Once the zip file is uploaded, in the **Review Package Content** for **OpenAI Playground**, click on the setup icon under **Action**.

     ![](./images/E2S5.png)

6. In the **Import setup** pane, select **Create as new (1)** from the drop-down for **Setup** and click on **Save (2)**.

      ![](./images/import-setup-1.png)

7. Repeat Steps 5 and 6 for **Openaisummarization**.

8. Next, click on **Import** to import the package into the PowerApps environment.

   ![](./images/E2S8.png)

9. Once the import is completed,click on **Apps (1)**, then click on `⋮` **(2)** next to **OpenAI Playground**, and click on **Edit (3)**.

   ![](./images/E2S9.png) 

   >**Note:** If you're unable to see the **OpenAI Playground** option, please wait for 5 minutes, and in the meantime, try refreshing the page.

11. You will observe that it has imported the Power App canvas app and the Power Automate Flow into the workspace.

      ![](./images/E2S10.png)

    >**Note:** If you see **Welcome to Power Apps Studio**, click **Skip** button.

12. To navigate back, click on **Back (1)**, then click **Leave (2)**.

      ![](./images/E2S11.png)

13. Next, on the **Flows (1)** tab, select radio button  for **Openaisummarization (2)**, then click on `...` **(3)** and **Turn on (4)** your flow.

      ![](./images/E2S12.png)

      >**Note:** If you see **Welcome to Power Automate**, click **Get started** button.

14. Click on **Edit** for Openaisummarization.

      ![](./images/E2S13.png)

15. Edit the Power Automate Flow HTTP step by updating the **https://your-resource-name.openai.azure.com/** with **<inject key="OpenAIEndpoint" enableCopy="true"/> (1)**, **deployment-id** with **<inject key="openaimodulename" enableCopy="true"/> (2)**, **api-version** with **2023-09-15-preview (3)**, **YOUR_API_KEY** with **<inject key="OpenAIKey" enableCopy="true"/> (4)**, and click on **Save**.

      ![](./images/E2S14.png)
   
16. From the **Apps (1)** page, click on the radio button for **OpenAI Playground** app to run the app by clicking on the **Play (2)** button.

     ![](./images/canves-play.png)

17. If you get a popup asking to start a free trial, click on **Start a free trial**, then on **You need a Power Apps license to use this app** window, select **Start a 30-day trial**. Choose your region from the drop-down menu and click on the **Start my trial** button. Wait for 2-3 minutes. It might take to start the trial.
    
18. In the **Canvas**, click on **Summary** **(1)** button and verify the **auto-filled text** **(2)**. Then click on the **Submit** **(3)** button to get the desired **Summary** **(4)**.

     ![](./images/E2S17.png)

    > **Note:** You can try the other available options in the portal, i.e., **SQL**, **ClassifyText**, **Parse**, and **Classify**.

## Summary

In this exercise, you have successfully integrated OpenAI APIs within Power Apps to enable functionalities like natural language processing, sentiment analysis, or personalized recommendations and built an application.

### You have successfully completed the exercise. Click on **Next >>** to proceed with the next exercise.

![](./images/next-page.png)
