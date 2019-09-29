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

To complete this trial, you will need 2 links that you should have received from us:
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

Once you are in the code editor you will be able see the code written by **Hello Food**’s Data Scientists. From the left side of the code editor you can navigate to different files. At a high level, the `model.py` file includes code to: train the model, run inference, and evaluate model performance. These are specified in the `train`, `predict`, and `eval` functions respectively. **There’s no Foundations Orbit features in them right now.**

<details>
  <summary>Optional reading</summary>
<br>

The train function takes two arguments: the start and end dates of your training period. Given these arguments, the train function fetches the data, processes it, trains a simple random forest model, and saves the model to a specified location.

The predict function takes one argument: the inference date. Given the inference date, the inference function fetches the data it needs to run predictions for the specified date, then it saves those predictions back to the database.

The eval function takes one argument: the evaluation date. Given evaluation date, the function fetches the data it needs to compute metrics as of the evaluation date, computes the metrics, and returns them.

-------------------------------------------------------------------------------------------------------------------------
</details>

Now, let’s run the training code to create the model. Navigate to the terminal window by clicking **Terminal** at the top bar and **New Terminal**. 

<img style="float: left;" src="https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/screenshots/terminal.png" height="70">

In the terminal window that appears in the bottom half of the editor, Enter the following command, then press the ‘Enter’ key:
```bash
python train_driver.py
```
_If you are on Windows OS, you need to press `Shift`+`Insert` to paste command into terminal, instead of `Ctrl`+`v`_

You can ignore the warning messages in the terminal. Once it completes, you can see in `fitted_objects/` folder that a new model was created as a result of the training, with the name `model.pkl` unless you change the code

**There is no Orbit magic so far. These are the things that you normally do in a typical data science project, but simplified for illustration purpose.**

## What happens next in real life?

Now the model is ready to be deployed and all production systems are wired up. You are getting ready to deploy the model and hopefully it will have a positive impact on the business: **lower churn, more revenue month over month**

In a real life situation, the following would happen on a monthly basis:
* Every month, the Operations team will get predictions from your model (i.e., getting from your model a list of customers the model predicts to be at risk of churning)
* Some actions are performed by the Operations team based on the predictions (i.e., actually sending out the special offer to the customers)
* As the results, customer behaviours are impacted and captured as data (i.e., some customers leave/stay, reflected by their records in the database)

We’ve created a simulated production environment to mimic what you would face in real life, every 30 seconds of the trial equates to 1 month in "real life".

**Next, we will go through a series of 4 Machine Learning Performance Management challenges that Orbit can solve.** 
1. [Model Deployment](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt1-deployment.md)
2. [Model Performance Monitoring](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt2-performance-monitoring.md)
3. [Data Health Monitoring](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt3-data-health-monitoring.md)
4. [Model Recalibration](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt4-recalibration.md)

**Let's get started on this journey! Please proceed to the first challenge:** [Model Deployment>](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt1-deployment.md)

