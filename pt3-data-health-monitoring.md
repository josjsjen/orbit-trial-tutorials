# Challenge 3: Monitoring production data health

## The Challenge

After a couple of minutes, you are probably beginning to see that your model performance is suffering. **Now that your model performance is decaying, revenue is dropping. Do you know what is wrong? What do you do?**

<details>
  <summary>Here are three options you normally have in real-life without Orbit</summary>

1. If you are a Data Science guru, you roll up your sleeves and head to the IDE and do some investigation on the dataset. You are welcome to write some python code to identify & resolve the problem, and re-deploy your new model.
2. If you are not technical, you can reach out to someone else to assist on the task. Is there a data scientist from your company that can spare the time from other initiatives to help you out?
3. You can email the original model developer at a.lu@dessa.com. He will fix the issue for you. He’s quite busy on his new projects, but he will try his best to get back to you in a couple of weeks.

-------------------------------------------------------------------------------------------------------------------------
</details>

**You need to act fast because your company is bleeding money as you read this.** There is a fourth option. You use Foundations Orbit to identify & resolve the issue in a few steps.

## The Solution

First, add the following line of code to the `model.py` after line 11
```python
from foundations_orbit import DataContract
```

Next, add these two lines of code to the `def train(...)` function in `model.py`. Insert this after the line `# insert DataContract creation code here #`, which should be around line 45
```python
    # insert DataContract creation code here #
    dc = DataContract("my_contract", x_train)
    dc.save(".")
```

In terminal, run the following command (You can ignore the warning messages in the terminal):
```bash
python train_driver.py
```
_If you are on Windows OS, you need to press `Shift`+`Insert` to paste command into terminal, instead of `Ctrl`+`v`_

You should be able to see in the explorer on the left hand side that a new file called "my_contract.pkl" has been created.

Next, add these two lines of code to the `def predict(...)` function in `model.py`. Insert this after the line `# insert DataContract validation code here #` which should be around line 74
```python
    # insert DataContract validation code here #
    dc = DataContract.load(".", "my_contract")
    dc.validate(x_inference, inference_date)
```

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>

IT and operations changes can lead to unexpected data anomalies capable of adversely affecting model performance. These changes aren’t tracked by traditional IT systems, which means that teams don’t notice them until it’s too late. For example, the team that maintains the databases might not know that your model is dependent on a particular column and decided to make changes to it, such as using a different value to encode something. Small changes like that can eventually lead to drastic changes by the time the data reaches your models.

Orbit introduces a way to monitor and validate production data for machine learning models. A two-step process is carried out to achieve this:  

First, the code we added to the train(...) function essentially creates a "data contract" object from our training dataset. This "data contract" object automatically summarizes characteristics of the training dataset (x_train), including the schema and key statistics. The ".save" function stores the information into a file that we can reference in the future.

Second, the code we added to the predict(...) function validates the production data. Orbit automatically performs data validation by running tests on the production dataset, making sure the dataset matches what the model is expecting in terms of schema and statistics. The outcome of the validation will be stored and presented on the GUI. Critical issues can trigger downstream processes and notify key stakeholders.

In our example, we create a data contract called "my_contract" from the training data, then apply this data contract on the production data that the predict function uses to generate predictions. In practice, you can create multiple Data Contracts, and place them at any point in your machine learning pipeline, quality-assuring both input and output of your model predictions.

-------------------------------------------------------------------------------------------------------------------------
</details>

The changes we applied above won't be effective until we deploy a new model. To do that, run this command in terminal (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v3 --project_directory=./ --env=scheduler
```
_If you are on Windows OS, you need to press `Shift`+`Insert` to paste command into terminal, instead of `Ctrl`+`v`_

Now head back to the GUI and do the following:
* Navigate to **Model Management** tab
* Under **Model Registry** click the **Default** radio button for the newest model package (named "model-v3")
* Navigate to **Data Health** tab using the side bar
* Under **Select report** drop down, located on the left hand side, select a validation report from the latest date like so:
<img style="float: left;" src="https://dessa.com/wp-content/uploads/2019/09/select-report.png" height="200">

* You should see information populated in the table to the right
* Review information in the three tabs: **Schema Check**, **Population Shift**, and **Data Abnormality**
* Check if any of the attributes have a <img style="float: left;" src="https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/screenshots/critical.png" height="25"> status

Once you’ve identified which attribute is having critical issue, remember the name of the attribute and report that to us. Please enter the name of the attribute in the **Report Data Issue** box in the top right corner of the GUI (as illustrated below), then hit the **SEND** button. 

<img style="float: left;" src="https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/screenshots/data_issue.png" height="100">

**We will fix the issue for you, but only if you correctly report which attribute is having critical issue.** This illustrates what happens in real life: teams that manage data in a large organization have to manage hundreds or more data tables and pipelines that are being updated all the time. They can not fix a data issue for your model if you can't tell them precisely what's wrong.

It might take a minute before the fix becomes effective. You can tell it is fixed by either:
* **Refreshing the Data Health** tab then checking the latest data validation results again
* Checking model performance in the **Model Evaluation** tab. You should see that the model metrics start to recover

While it is not part of this trial, Orbit also offers email and slack notification features so that you can set up monitoring for data issues. The right party will get notified and start investigating right away.

## Congratulations! You've completed this challenge!

To recap what you just achieved: you learned how to add data monitoring and validation for your machine learning pipeline. You achieved this by using Orbit to create an summary of the data your pipeline is expecting (called "Data Contract") and to add a "monitor" to your dataset by adding a few lines of code. Orbit automatically monitors any data issues that might cause your model to suffer and surfaces that information to you ASAP. This helps you keep a pulse on the health of your machine learning pipeline and helps you identify issues so the right parties can fix them faster.


[<<Overview](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/hello-food-trial-instructions.md) &nbsp; [<Back](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt2-performance-monitoring.md) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  **[Proceed to the next challenge: Model Recalibration>](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt4-recalibration.md)**

