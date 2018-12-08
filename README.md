# Watson Studio Local Workshop V1.2.2
In this workshop you will learn how to develop and deploy applications in Watson Studio Local. The workshop has been divided into several stand-alone parts for those who are interested in a specific development tool or deployment task.

## Prerequisites
1. Knowledge of analytics. These labs do not teach you the basics of analytics or how to implement analytics in R, Python and SPSS. The purpose of this workshop is to provide hands-on experience with analytics tools and deployment functions in Watson Local. 
2. To run this workshop you need an instance of Watson Local V1.2.2 or above. 

3. Download [WSL_Demos.zip](https://github.com/bleonardb3/WSL_12-11/blob/master/WSL_Projects/WSL_Demos.zip?raw=true).

### Setting up lab projects in DSX Local
1. Rename the downloaded **WSL_Demos.zip** file and give it a unique name.  For example, add your initials (i.e. WSL_Demos_BB.zip).    *Note: Project names in DSX Local cluster must be unique. When we create a project "from file", the project name is inherited from the file name*.
2. Click on https://169.60.117.66 or https://169.60.117.69 (depending on which cluster you were assigned) to log in to Watson Studio Local. Enter Username and Password, and then click **Sign In**
3. Click **Add Project**.
> <img src="https://github.com/bleonardb3/WSL_12-11/blob/master/images/AddProject.png"/>
4. Select "From File", browse to the **.zip file** (the WSL_Demos zip file that you renamed above) and click **Open**. Then click **Create**.
> <img src="https://github.com/bleonardb3/WSL_12-11/blob/master/images/CreateProjectFromFile.png" border="5"/>

### Lab 1: Build, Save and Test SparkML Models (Jupyter/Python)
1. Click on **Notebooks**. 
> <img src="https://github.com/bleonardb3/WSL_12-11/blob/master/images/Click%20on%20Notebooks.png"/>
2. Click on **TelcoChurn_SparkML** to open the *Jupyter* notebook. This notebook has been implemented for the Python 3.5 runtime. You can verify the runtime by running the first cell in the notebook. 
> <img src="https://github.com/bleonardb3/WSL_12-11/blob/master/images/ClickonTelcoSparkML.png"/>
3. Follow instructions in the notebook.

### Lab 2: Create Batch Script and Test Batch Scoring
1. You must have completed "Lab 1: Build, Save and Test SparkML Models" before working through this lab.
2. Navigate to the **Models** section of the project and click into the saved **Telco_Churn_ML_model**.
3. Click the **Batch score** tab.
4. For **Execution**, select *Local Spark*
5. For **Input data set**, select *TelcoModelFeedback.csv*.
6. For **Output data set**, check **"Local file"** and specify *new_customers_scores.csv*.
7. On the top right, click **Advanced Settings**.
8. Scroll through the Advanced Setting to see the various options.  Click **Save** to save the default settings.
8. Click **Generate Batch Script**.  (Note: the batch script can be edited. For example, to perform pre/post processing tasks)

![batchscoring](https://github.com/bleonardb3/WSL_12-11/blob/master/images/BatchScores.png?raw=true)

9. Click **Run now**,scroll to the bottom and wait till the status of the job changes to Success.
10.Verify that the new_customers_scores.csv is in the **Data Sets** section of your project's assets page  
![verifyscoring](https://www.github.com/bleonardb3/WSL_12-11/blob/master/images/VerifyBatchScoresFile.png?raw=true)

### Lab 3: Create Model Evaluation Script and Test Evaluation
1. You must have completed "Lab 1: Build, Save and Test SparkML Models" before working through this lab.
2. Navigate the to the **Models** section of the project and click into the saved **Telco_Churn_ML_model**.
3. Click the **Evaluate** tab.
4. For the scripts inputs, specify these values.<br/>
![model_eval](/img/model_eval.png?raw=true)

5. Click **Advanced Settings** and change the File Name of the script. For example, you can name it ChurnModelEvalScript. Also change the Job Name to ChurnModelEvalJob. Click **Save**.
6. Click **Generate evaluation Script**.
7. Click **Run now**, scroll down and wait till the status changes to Success. ,br/>
![model_eval_job_results](/img/model_eval_job_results.png?raw=true)
8. Navigate to the **Models** section of the project and click into the **Telco_Churn_ML_model** to see the evaluation results. 

### Lab 4: Deploy Project into Production 
1. You must have completed Lab 1, Lab 2 and Lab 3 before working through this lab.
2. Navigate to the top of the project assets page and click **Commit and push**.<br/>
![commit_push](/img/commit_push.png?raw=true)
3. Specify the **commit message** about the changes you are committing, e.g. "*deploy generated scripts*".
4. Specify a **version tag**, e.g. *workshop-release*.  A version tag marks a deployment ready version of the project, and identifies a specific verion of the project.
5. Click **Commit and push**. Sign off DSX. Login to DSX as Administrator (same userid with an "a" added)
6. Navigate to the **Deployment Manager**.  <br/>
![deployment_manager](/img/deployment_manager.png?raw=true)
7. Click the **+** icon to create a project release.
8. Specify the following inputs
* Name: WorkshopRelease1
* Route: release1 (this string will be used in all URLs that are generated by deployment, that’s why it can’t have spaces, upper case characters, and special characters). Important: if you’re sharing a cluster, make the route unique, for example, add initials to release1. 
* Source project: the project that you want to deploy
* Tag: tag that you specified in the earlier steps. 

9. Click **Create**. This will take a few minutes because DSX is making a copy of all assets in the project.
10. The default view shows all assets that are a part of the project. Notice that you can filter them by type if you select the drop down. 
11. Filter the assets by **Models**.
12. Select **Telco_Churn_ML_Model**
12. Click **Web service** to define an **Online Deployment** for the model.
13. Specify the name *telco-churn-online*.  Notice that the name gets appended to the URL (REST endpoint), that’s why it has to be lowercase with no special characters. 
14. Click **Create**
15. The **REST endpoint** is displayed in the model details. This endpoint won’t be live until we launch the project.<br/>
![mmd_rest_endpoint](/img/mmd_rest_endpoint.png?raw=true)<br/>
16. Filter the assets by **Scripts** and configure jobs for the batch scoring and model evaluation scripts.
17. Click the **Deployments** tab to list all the deployments you have created. <br/>
![mmd_deployments](/img/mmd_deployments.png?raw=true)<br/>
18. The deployments are in a disabled state because the project has not been launch.  Click the **Launch** button at the top right corner.<br/>
Launching the release will:
* Start all environments that will be used for deployment
* Enable the REST endpoints
* Enable URLs for “applications” (notebooks and Shiny)
* Enable schedules (if they are configured)
* Enable on-demand invocation of jobs. 

19. When all the deployments are enabled, click on any of the deployments and test them either with an API call or run a batch job on demand. 
20. For example, let's test the web service deployment with an API call. Select the web service deployment. 
> <img src="https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/img/DeploymentList.png"/>
21. Select the API tab 
> <img src="https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/img/SelectAPI.png"/>
22. Scroll down the page to the Request block. Replace the value of "INSERT_VALUE" after the "International:" and "Dropped" fields with the values 1.0 and 1.0 without quotes (both are float field) and then click on **Submit** 
> <img src="https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/img/InsertValue.png"/>
> <img src="https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/img/ReplaceInsertValue.png"/>
23. Scroll to the right to see the prediction. 
> <img src="https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/img/Response.png"/>
24 Sign off as administrator and sign on as user. 

### Lab 5: Watson Machine Learning
1. Follow the instructions in [Watson Model Builder](https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/Lab%20Instructions/WatsonMachineLearning.pdf). Note you will need to click on the Download button to download the instructions to your machine. Otherwise the hyperlinks in the doucment will not work. 

### Lab 6: SPSS Modeler in DSX 
1. Follow the instructions in [SPSS Modeler](https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/Lab%20Instructions/titanic-spss-modeler-edits-local-1.pdf). 
Note you will need to click on the Download button to download the instructions to your machine. Otherwise the hyperlinks in the document will not work. 

### Lab 7: Data Refinery 
1. Follow the instructions in [Data Refinery](https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/Lab%20Instructions/Data%20Refinery%20Lab_Local_v2.pdf).
Note you will need to click on the Download button to download the instructions to your machine. Otherwise the hyperlinks in the document will not work. 

### Lab 8: Build, Save and Test SparkML models (Zeppelin/Python)
1. Navigate to **Assets** view and open **TelcoChurn_Zeppelin** notebook.  
2. Follow instructions in the notebook.

### Lab 9: Build R models in Jupyter and deploy into Shiny App
1. Follow the instructions in [R in DSXL](https://github.com/bleonardb3/DSX_Local_Workshop_V12/blob/master/Lab%20Instructions/R_in_DSXL.pdf)
Note you will need to click on the Download button to download the instructions to your machine. Otherwise the hyperlinks in the document will not work. 

### Lab 10: Build, Save and Test Scikit-Learn Models (Jupyter/Python)
1. Navigate to **Assets** view and open **CreditCardDefault_SkLearn** notebook.  
2. Follow instructions in the notebook.





