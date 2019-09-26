<img style="float: left;" src="https://dessa.com/wp-content/uploads/2018/05/dessa_logo.svg" height="50">
<br>
<hr>

# Welcome to Foundations Orbit Online Trial

*Estimated time: 30 minutes*

In this tutorial we will go through an example of what Data Science teams need to do to manage machine learning models in production. We will go through this trial in a gamified setting where you will be in control of what actions to take with the ultimate goal of solving a business problem.

We’re providing you with a fully-managed environment so you can focus on understanding and solving the challenges associated with managing live models. This trial environment provides you with
* An in-browser Python IDE with popular data science & ML packages pre-installed
* The Foundations Orbit Graphical User Interface (GUI) 
* A tutorial with example code & data
* A simulated production environment
* Free cloud compute

To complete this trial, you will need 3 things that you should have received from us:
* Your user_key
* Link to the online code editor
* Link to the Foundations Orbit GUI

## Introduction to the problem

Let’s imagine you work at a meal-kit subscription company, called **Hello, Food**. The business model is to prepare & deliver the ingredients for meals every week to subscribed customers for them to cook themselves. 
  
**Hello, Food**'s revenues are falling as customer attrition has overtaken new subscriptions. In an attempt to retain customers, the company has decided to earmark some money to start a retention initiative by sending special offers to a subset of the customer base.

**You are in charge of this initiative! Good luck!**

<details>
  <summary>  Here are some facts about the business (click to expand, no need to memorize): </summary>
<br>
  
* Revenue per customer: $300 per month
* Number of customers (as of the start of this trial): 5000
* Monthly growth rate: 14% 
* Monthly attrition rate (as of the start of this trial): ~17%
* Special offer capacity: 10%. This means the company has budget to send special offer to 10% of customers each month. For simplicity, let’s assume:
  * If a customer wants to churn and receives an offer before they actually do, their churn likelihood will significantly reduce
  * If a customer has no intention to churn but receives an offer, they will still get the benefits of the special offer. However, this would be a waste of the retention budget
<hr>
</details>

**Because the company only has limited resources to address retention, it is critical to predict correctly which customers are likely to churn**. 4 months ago, your team decided to develop a **machine learning model to predict which customers are likely to churn based on their recent behaviour so that they can be targeted for special offers.** 

## Train the model using sample code provided

First, let's open the **code editor** on a browser using the link that we sent you. 

<details>
  <summary>FAQ: Does Orbit ONLY work on specific coding environment</summary>
<br>

The answer is NO. 


Being non-instrusive is a key design principle for us. **Orbit puts no restriction on model development tool and approach. It is very easy to incorporate Orbit in your data science workflow as long as you can run Python.**

In this trial, we are using Visual Studio Code as our code editor.
  
-------------------------------------------------------------------------------------------------------------------------
</details>

Once you are in the code editor you can see the model development code written by **Hello Food**’s Data Scientists. You can navigate to different files using the explorer to the left. At a high level, the `model.py` file includes some code to train the model, some code to use the model for inference, and some code to calculate the performance of the model. There are three functions in `model.py`: `train`, `predict`, and `eval`. **There’s no Foundations Orbit feature in them right now.**

<details>
  <summary>Optional reading</summary>
<br>
  
The train function takes in two arguments: start and end dates of your training period. Given these arguments, the train function fetches the data, processes them, trains a simple random forest model, and saves the model to a specified location. 


The predict function takes in one argument: the inference date. Given the inference date, the function pulls from database the data it needs to run prediction for the specified date, then saves the predictions back to the database.

The eval function takes in one argument: the evaluation date. Given evaluation key, the function pulls from database the data it needs to compute metrics as of the evaluation date, computes the metrics, and use Foundations Orbit’s track_production_metrics to log and store the metrics.

-------------------------------------------------------------------------------------------------------------------------
</details>

Now, let’s run the training code to create the model. Navigate to the terminal window by clicking **Terminal** at the top bar and **New Terminal**. In the terminal window that appears in the bottom half of the editor, Enter the following command, then press the ‘Enter’ key:
```bash
python train_driver.py
```
You can ignore the warning messages in the terminal. Once it completes, you can see in `fitted_objects/` folder that a new model was created as a result of the training, with the name `model.pkl` unless you change the code

**There is no Orbit magic so far. These are the things that you normally do in a typical data science project, but simplified for illustration purpose.**

## What happens next in real life?

Now the model is ready to be deployed and all production systems are wired up. You are getting ready to deploy the model and hopefully it will have a positive impact on the business: **lower churn, more revenue month over month**

In the real life situation, the following would happen on a monthly basis:
* Every month, the Operations team will get predictions from your model (i.e., getting from your model a list of customers the model predicts to be at risk of churning)
* Some actions are performed by the Operations team based on the predictions (i.e., actually sending out the special offer to the customers)
* As the results, customer behaviours are impacted and captured as data (i.e., some customers leave/stay, reflected by their records in the database)

We’ve created a simulated production environment to mimic what you would face in real life, with every 1 minute of the trial equates to 1 month in real life.

**Below, we will go through a series of 4 challenges to Machine Learning Performance Management that Orbit can solve. Let's get started on this journey!**

## Challenge 1: Deploying a model

### The Challenge

Now, we are ready to put the trained model into production. However, it is not that simple.

For one, putting a model into production means that it needs to be wired up with the various IT systems that consume the model predictions. For example, software developers will need to change the software in call center to programmetically fetch monthly predictions from your model, likely through some kind of API. You model is not that useful to anyone "outside of the lab" if it can't be consumed by operations. It'd be very time consuming and error-prone if you have to manually run it to generate predictions and send them in csv's every time.

Plus, putting model in production requires you being able to reproduce the model. But as you know, a model is just the final product of a training process that includes many moving parts evolving at different rate. Very often teams cannot re-create a model for production because they lost track of the version of code that created the model and the dependencies of the training code,  which includes configurations, hard-coded values, libraries, meta-data, artifacts, etc.

### The Solution

Orbit provides a way to seamlessly package machine learning models for production. Copy and paste the following code in `foundations_package_manifest.yaml`:
```yaml
entrypoints:
  predict:
    module: 'model'
    function: 'predict'
  evaluate:
    module: 'model'
    function: 'eval'
  recalibrate:
    module: 'model'
    function: 'train'
```

Next, In the terminal, enter this command then press ‘Enter’ key (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v1 --project_directory=./ --env=scheduler
```
The model has been deployed to our trial environment running on GCP.

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>
  
Orbit automatically package up the code and model into what we call a "model package", which is a microservice that can be accessed by any IT systems using REST API. The entrypoints specified in the `foundations_package_manifest.yaml` file will become API endpoints that can receive requests and response with output from/to IT systems. 

For example, in the yaml file above we tell Orbit to create an API endpoint for this model package called "predict". Requests to this API endpoint will get passed into the predict(...) function, which Orbit will execute. In the file we are also defining two other API endpoints, "evaluate" and "recalibrate", we will discuss them later.

Why is this important?
* The packaging aspect of this feature ensures reproducibility of models
* The serving aspect of this feature essentially makes it easy to turn your model into a service that can communicate with modern technology systems via API, making it easier to put models in production

-------------------------------------------------------------------------------------------------------------------------
</details>

<details>
  <summary>FAQ: Does Orbit ONLY work on specific cloud infrastructure like Google Cloud Platform?</summary>
  <br>
  
  The answer is NO. **Orbit is platform agnostic**, meaning it can be configured to work on any insfrastructures that meet the needs of your team, including major cloud platforms (AWS, Azure, GCP), on-premise clusters, or a hybrid of infrastructures.
  
-------------------------------------------------------------------------------------------------------------------------
</details>

Now please go to the GUI using the other link that we shared with you
* Once you enter the GUI, you will see 1 project in the landing page
* Click on the **orbit-trial** project
* You will land on the **Model Management** tab, where you will see information of the model you just deployed

### Congradulations! You've completed this challenge!

To recap what you just achieved: you learned how to take model development code, resulting model, dependencies, package them into something reproducible and deploy it as a micro-service that can interact with modern IT systems via API. 

In addition, being non-instrusive to existing data science workflow is a key focus for Orbit. No matter how you develop your model, you can use Orbit to deploy them and manage them after deployment. 

## Challenge 2: Monitoring model performance using Orbit

### The Challenge
So we just deployed the first version of our churn model in production. But how do you know if the model is performing well?  Are the predictions accurate? Is it creating positive impact to our business? Is it performing better month over month? or worst than we expected?

**How can you track the performance metrics of your model over time, and be able to monitor them easily?** 

Transparency of model performance is often a challenage in applied AI. Once a model is deployed, it is often very hard for people other than the original model developer to know how it is performing. Teams often end up building and scheduling ad-hoc performance reporting scripts and communicating back-and-forth with non-technical stakeholders. 

### Solution

First, add the following line of code to the `model.py` after line 10
```python
import foundations
```

Add the following code to the `eval(...)` function in `model.py`. Insert these after the line `# insert foundations metric tracking here #`, which should be around line 109:

```python
    # insert foundations metric tracking here #
    foundations.track_production_metrics("accuracy", {str(eval_date): accuracy})
    foundations.track_production_metrics("roc_auc", {str(eval_date): roc_auc})
    foundations.track_production_metrics("revenue", {str(eval_date): revenue})
    foundations.track_production_metrics("n_active_custs", {str(eval_date): n_active_custs})
```
The changes we applied above won't be effective until we deploy a new model. To do that, run this command in terminal (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v2 --project_directory=./ --env=scheduler
```

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>

Orbit allows you to specify custom metric calculation code and tracks the resulting metrics. After we deploy this model, Orbit will package the code and automate the execution of these metric calculation function and the tracking of the resulting metrics, which you can visualize in the Orbit GUI. In this example, we are monitoring 4 metrics. The first two are typical mathematical metrics for classification problems and the last two are business metrics for this initiative.

-------------------------------------------------------------------------------------------------------------------------
</details>

Now head back to the GUI:
* Navigate to the **Model Management** tab
* Under **Model Registry** click the **Default** radio button for the model package that you just deployed (named "model-v2")
* Navigate to **Model Evaluation** tab using the side bar

Again, 1 minute of trial simulates 1 month in real life. **Now, keep an eye on the model performance in the Model Evaluation tab.** You should be able to see the 4 metrics that we defined earlier are being tracked over time.

### Congratulations! You've completed this challenge!

To recap what you just achieved: you learned how to define custom metrics and asked Orbit to track them for you in production. Data scientists can achieve this by adding a few lines of code. Later on, these data scientists, their managers, and other non-technical stakeholders will have visibility into the model's performance through on a dashboard, without having to go into the code.

## Challenge 3: Monitoring production data health

### The Challenge

After a couple of minutes, you are probably beginning to see that your model performance is suffering. **Now that your model performance is decaying, revenue is dropping. Do you know what is wrong? What do you do?**

<details>
  <summary>Here are three options you normally have in real-life without Orbit</summary>

1. If you are a Data Science guru, you roll up your sleeves and head to the IDE and do some investigation on the dataset. You are welcome to write some python code to identify & resolve the problem, and re-deploy your new model
2. If you are not technical, you can reach out to someone else to assist on the task. Is there a data scientist from your company that can spare the time from other initiatives to help you out?
3. You can email the original model developer at a.lu@dessa.com. He will fix the issue for you. He’s quite busy on his new projects, but he will try his best to get back to you in a couple of weeks

-------------------------------------------------------------------------------------------------------------------------
</details>

**You need to act fast because your company is bleeding money now as you are reading this.** There is a fourth option. You use Foundations Orbit to identify & resolve the issue in a few steps.

### The Solution

First, add the following line of code to the `model.py` after line 11
```python
from foundations_orbit import DataContract
```

Next, add these two lines of code to the `train(...)` function in `model.py`. Insert this after the line `# insert DataContract creation code here #`, which should be around line 42
```python
    # insert DataContract creation code here #
    dc = DataContract("my_contract", x_train)
    dc.save(".")
```

In terminal, run the following command (You can ignore the warning messages in the terminal):
```bash
python train_driver.py
```
You should be able to see in the explorer on the left hand side that a new file called "my_contract.pkl" has been created.

Next, add these two lines of code to the `predict(...)` function in `model.py`. Insert this after the line `# insert DataContract validation code here #` which should be around line 72
```python
    # insert DataContract validation code here #
    dc = DataContract.load(".", "my_contract")
    dc.validate(x_inference, inference_date)
```

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>

IT and operations changes can lead to unexpected data anomalies capable of adversely affecting model performance. These changes aren’t tracked by traditional IT systems, which means that teams don’t notice them until it’s too late. For example, the team that maintains the databases might not know that your model is dependent on a particular column and decided to make changes to it, such as using a different value to encode something. Small changes like that could proliferate through various data systems and eventually leads to drastic changes by the time the data reach your models


Orbit introduces a way to monitor and validate production data for machine learning models. A two-step process is carried out to achieve this:  

First, the code we added to the train(...) function essentially creates a "data contract" object from our training dataset. This "data contract" object automatically summarizes characteristics of the training dataset (x_train), including schema and key statistics. The ".save" function stores the information into a file that we can reference in the future.

Second, the code we added to the predict(...) function essentially validates the production data at prediction times. Orbit automatically performs data validation by running different tests on the production dataset, making sure the dataset matches what the model is expecting in terms of schema, shape, statistics, etc. The outcome of the validation will be stored and presented on GUI. Critical issues can trigger downstream processes and notifications to key stakeholders.

In our example, we create one data contract called "my_contract" from the training data, then apply this data contract on the production data that the predict function uses to generate predictions. In practice, you can create multiple Data Contracts at multiple points of your machine learning pipeline, quality-assuring both input and output of your model predictions.

-------------------------------------------------------------------------------------------------------------------------
</details>

The changes we applied above won't be effective until we deploy a new model. To do that, run this command in terminal (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v3 --project_directory=./ --env=scheduler
```

Now head back to the GUI and do the following:
* Navigate to **Model Management** tab
* Under **Model Registry** click the **Default** radio button for the model package that you just deployed (named "model-v3")
* Navigate to **Data Health** tab using the side bar
* Under **Select report** drop down, located on the left hand side, select a validation report from the latest date like so:
<img style="float: left;" src="https://dessa.com/wp-content/uploads/2019/09/select-report.png" height="200">

* You should see information populated in the table to the right
* Review information in the three tabs: **Schema Check**, **Population Shift**, and **Data Abnormality**
* Check if any of the attributes have a `critical` status

Once you’ve identified which attribute is having critical issue, remember the name of the attribute and report that to us by following the instructions below. **We will fix the issue for you, but only if you correctly report which attribute is having critical issue.** To report and fix the issue, navigate to the code editor, enter the following command in the terminal window

```bash
curl http://<user_key>:31998/simulator/fix_special_value?column_name=<attribute_name>
```
Replacing `<user_key>` with your **user_key** and `<attribute_name>` with the name of the attribute you want to report. Make sure you do not leave the '<' and ">" in the command.

Once you correctly report the issue, we will fix it. You can tell it is fixed by **refreshing the Data Health** tab then checking the latest validation report and checking the latest model performance in the **Model Evaluation** tab. You should see that the model metrics start to recover.

While it is not part of this trial, Orbit also offers email and slack notification features so that you can set up monitoring for data issues. The right party will get notified and start investigating right away.

### Congradulations! You've completed this challenge!

To recap what you just achieved: you learned how to add data monitoring and validation for your machine learning pipeline. You achieved this by using Orbit to create an summary of the data your pipeline is expecting (called "Data Contract") and to add a "monitor" to your dataset by adding a few lines of code. Orbit automatically monitors any data issues that might cause your model to suffer and surfaces that information to you ASAP. This helps you keep a pulse on the health of your machine learning pipeline and helps you identify issues so the right parties can fix them faster.


## Challenge 4: Addressing concept drift with recalibration

### The Challenge

By now, you should see that your model performance has declined once again (give it a minute or two, it will happen). However, there isn't any issue you can see in the **Data Health** tab. So we know the model is NOT declining due to broken production data.

It is very common for machine learning models to suffer from concept drift. Models are trained using historical data, but changes in customer behaviours and business operations happen over time, changing the underlying relationships between model input and output. In reality, models in production degrade in performance. It is only a matter of time before they become obsolete.

Data Scientists team often address this by going back to model development phase again, it will take a long time and resource while the model degradation is causing the company millions of dollars.

### The Solution

With what we did in **Challenge 1** earlier we actually have the ability to recalibrate the model easily. The model package that we deployed has an entrypoint for model recalibration, which will try trigger the `train(...)` function to run and create a new model. Recall that the `train(...)` function is expecting two arguments: start and end dates of the training period. 

First, we need to detemine the time range of data with which we want to recalibrate the model:
* Navigate to **Model Evaluation** tab
* Identify the **latest date available** in the simulated environment. This is the `end_date` of our recalibration
(hint: Look for the latest date available on the charts)
* Determine the `start_date` of our recalibration
(hint: we recommend 3 months before the `end_date`)
* Note down `start_date` and `end_date` in the format of "yyyy-mm-dd". We will need them in a moment.

Then please do the following on the GUI:
* Navigate to **Model Management** tab 
* Click the **Recalibrate** button of your default model (should be "model-v3"), it's the _green_ button on the right
* A modal should appear on the screen
* Under **Model Name**, enter a name for the model that you are about to create. For example, enter “model-v4”
* Under **Parameters**, enter the following:

  | Key        | Value                      |
  | -----------|--------------------------- |
  | start_date | start date in yyyy-mm-dd   |
  | end_date   | end date in yyyy-mm-dd     |

With the start_date and end_date values that you noted down earlier.

* Then click the **Run Recalibration** button

**----------Recalibration will take a moment. Please wait a minute then hit refresh----------**

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
  <br>

Since our model package has a "recalibrate" entrypoint defined. Upon request from API, Orbit will execute the function specified by the entrypoint and launch a new model package one the recalibration is done.

As a user, you can define what API arguments the model microservice is expecting to successfully kickoff a recalibration job. In this example, the `train` function expects two arguments, start and end dates of the training period.

-------------------------------------------------------------------------------------------------------------------------
</details>

* Under **Model Registry** click the **Default** radio button for the newest model package (named "model-v4")

You can see the model performance again in **Model Evaluation**. You should be able to see the model accuracy and ROC_AUC have improved as a result, and revenue and number of active customers are starting to recover.

### Congradulations! You've completed this lesson!

To recap what you just achieved: you learned how to recalibrate your model to make it up-to-date with the latest data. This address the concept drift issue where historical relationship between model input and output changes overtime. All models suffer from this from the moment they are created.

More importantly you achieved this without having to go into the code. With Orbit you can automate this process so that you maximize the benefit that you machine learning models bring. While not part of this trial, Orbit also provides more sophisticated control on how the model can be recalibrated, including the ability to schedule or trigger recalibration based on model performance


## Congratulations. You’ve completed the trial.

**To Summarize, in this trial, we’ve shown you several key features of Foundations Orbit:**
* Easily convert your models and code into consumable micro-services   
  -> This ensures reproducibility of models and reduces headache when putting models in production
* Monitor model performance in production   
  -> This gives your organization transparency into how model are performing
* Monitor production data issues and distribution shift   
  -> This prevents data issues that would adversely impact your models from being undetected, costing you millions
* Automatically recalibrate your models with new data   
  -> This ensure the model is up-to-date without the hustle and delay from rebuilding models

**In addition, Orbit has other features that address different challenges with managing machine learning models once they are deployed, including:**
* Audit Trail
* A/B, canary, blue-green deploy, and out-of-time testing 
* Notification integration with email and Slack
* Scheduling, automation, and orchestration of various production tasks
* Custom data monitoring functions
* Model version control

We do not have time to cover these features today. Contact us for a full demo.

## What are the next steps? 

1. We would like to hear your what you think about Foundations Orbit!
Fill out the post-trial survey that we sent you. Tell us what you thought of Foundations Orbit via email or tweet us @Dessa

2. Try this on your own infrastructure and machine learning user cases. There are several options, contact us at 
foundations@dessa.com to learn more

3. Try out our model development platform, Foundations Atlas. This is a tool to give superpower to Data Scientists so they can develop machine learning models better and faster. You download the Community Edition for free at https://www.atlas.dessa.com/
