# Challenge 4: Addressing concept drift with recalibration

## The Challenge

Let's first head back to the **Model Evaluation** tab. You should see that your model performance has declined once again (give it a minute or two, it will happen). However, there isn't any issue you can see in the **Data Health** tab. So we know the model is NOT declining due to broken production data.

It is very common for machine learning models to suffer from concept drift. Models are trained using historical data, but changes in customer behaviours and business operations happen over time, changing the underlying relationships between model input and output. In reality, models in production degrade in performance. It is only a matter of time before they become obsolete.

Data Scientists team often address this by going back to model development phase again, it will take a long time and resource while the model degradation is causing the company millions of dollars.

## The Solution

With what we did in **Challenge 1** earlier we actually have the ability to recalibrate the model easily. The model package that we deployed has an entrypoint for model recalibration, which will try trigger the `train(...)` function to run and create a new model. Recall that the `train(...)` function is expecting two arguments: start and end dates of the training period.

First, we need to determine the time range of data with which we want to recalibrate the model:
* Identify the **current date** in the simulated environment. This is the `end_date` of our recalibration
(reminder: you can tell by checking the date in the top right corner of the GUI)
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

**IMPORTANT: make sure that the start_date is a date earlier than the end_date, and both values are in yyyy-mm-dd format**

* Then click the **Run Recalibration** button

**-----------------------------------------------------------------------------------------------------------------**

**Once the recalibration job is launched, it will take some time for the job to finish and the new model to be deployed.
Please wait for 1-2 minutes then hit refresh, you should see a new model package in Model Registry**

**-----------------------------------------------------------------------------------------------------------------**

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
  <br>

Since our model package has a "recalibrate" entry point defined. Upon request from the API, Orbit will execute the function specified by the entry point and launch a new model package once the recalibration is done.

As a user, you can define what API arguments the model microservice is expecting to successfully kickoff a recalibration job. In this example, the `train` function expects two arguments, start and end dates of the training period.

-------------------------------------------------------------------------------------------------------------------------
</details>

* Under **Model Registry** click the **Default** radio button for the newest model package (named "model-v4")

You can see the model performance again in **Model Evaluation**. You should be able to see the model accuracy and ROC-AUC have improved, and revenue and number of active customers are starting to recover.

## Congratulations! You've completed this lesson!

To recap what you just achieved: you learned how to recalibrate your model to make it up-to-date with the latest data. This addresses the concept drift issue where the relationship between model input and output changes overtime. All models suffer from this from the moment they are created.

More importantly you achieved this without having to go into the code. With Orbit you can automate this process so that you maximize the benefit that you machine learning models provide. While not part of this trial, Orbit also provides more sophisticated control on how the model can be recalibrated, including the ability to schedule or trigger recalibration based on model performance.


# Congratulations. You’ve completed the trial.

**To Summarize, in this trial, we’ve shown you several key features of Foundations Orbit:**
* Easily convert your models and code into consumable micro-services   
  -> This ensures reproducibility of models and reduces headache when putting models in production
* Monitor model performance in production
  -> This gives your organization transparency into how models are performing
* Monitor production data issues and distribution shift   
  -> This prevents data issues from going undetected that would otherwise adversely impact your models, costing you millions
* Automatically recalibrate your models with new data   
  -> This ensures models are kept up-to-date without the need to rebuild manually

**In addition, Orbit has other features that address different challenges with managing machine learning models once they are deployed, including:**
* Audit Trail
* A/B, canary, blue-green deploy, and out-of-time testing
* Notification integration with email and Slack
* Scheduling, automation, and orchestration of various production tasks
* Custom data monitoring functions
* Model version control

Contact us for a demo of these additional features.

# What are the next steps?

1. We would love to hear what you think about Foundations Orbit!
Fill out the post-trial survey that we sent you. Tell us what you thought of Foundations Orbit via email or tweet us @Dessa

2. Try this on your own infrastructure and machine learning use cases. There are several options, contact us at
foundations@dessa.com to learn more.

3. Try out our model development platform, Foundations Atlas. This tool gives superpowers to Data Scientists so they can develop machine learning models better and faster. You download the Community Edition for free at https://www.atlas.dessa.com/

[<<Overview](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/hello-food-trial-instructions.md) &nbsp; [<Back](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt3-data-health-monitoring.md) 
