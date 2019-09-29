# Challenge 2: Monitoring model performance using Orbit

## The Challenge
So we just deployed the first version of our churn model in production. But how do you know if the model is performing well?  Are the predictions accurate? Is it having a positive impact on our business? Is it performing better month over month? Or is it getting worse?

**How can you track the performance metrics of your model over time, and be able to monitor them easily?**

Transparency of model performance is often a challenge in applied AI. Once a model is deployed, it is often very hard for people other than the original model developer to know how it is performing. Often, teams end up building and scheduling ad-hoc performance reporting scripts and communicating back-and-forth with non-technical stakeholders.

## Solution

First, add the following line of code to the `model.py` after line 10
```python
import foundations
```

Add the following code to the `def eval(...)` function in `model.py`. Insert these after the line `# insert foundations metric tracking here #`, which should be around line 113:

```python
    # insert foundations metric tracking here #
    foundations.track_production_metrics("accuracy", {str(eval_date): accuracy})
    foundations.track_production_metrics("roc_auc", {str(eval_date): roc_auc})
    foundations.track_production_metrics("revenue", {str(eval_date): revenue})
    foundations.track_production_metrics("n_active_custs", {str(eval_date): n_active_custs})
```

_If you are new to Python: in Python, indentation consistency is required. Generally 4 whitespaces are used for indentation and is preferred over tabs. When you copy and paste the 4 lines of code above, make sure there are 4 whitespaces in front of each line._

The changes we applied above won't be effective until we deploy a new model. To do that, run this command in terminal (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v2 --project_directory=./ --env=scheduler
```
_If you are on Windows OS, you need to press `Shift`+`Insert` to paste command into terminal, instead of `Ctrl`+`v`_

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>

Orbit allows you to specify custom metric calculation code and tracks the resulting metrics. After we deploy this model, Orbit will package the code and automate metric tracking which is visualized in the Orbit GUI. In this example, we are monitoring 4 metrics. The first two are standard metrics for classification models and the other two are business metrics.

-------------------------------------------------------------------------------------------------------------------------
</details>

Now head back to the GUI:
* Navigate to the **Model Management** tab
* Under **Model Registry** click the **Default** radio button for the newest model package (named "model-v2")
<img style="float: left;" src="https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/screenshots/change_default.gif" height="100">

* Navigate to **Model Evaluation** tab using the side bar

**Now, keep an eye on the model performance in the Model Evaluation tab.** You will need wait 30 seconds before the metrics start to show up. Again, 30 seconds of the trials equates 1 month in real life. You should be able to see that the 4 metrics that we defined earlier are being tracked over time.

<img style="float: left;" src="https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/screenshots/eval_metrics.gif" height="150">

## Congratulations! You've completed this challenge!

To recap what you just achieved: you learned how to define custom metrics and asked Orbit to track them for you in production. Data scientists can achieve this by adding a few lines of code. Later on, these data scientists, their managers, and other non-technical stakeholders will have visibility into the model's performance through on a dashboard, without having to go into the code.


[<<Overview](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/hello-food-trial-instructions.md) &nbsp; [<Back](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt1-deployment.md) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  **[Proceed to the next challenge: Data Health Monitoring>](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt3-data-health-monitoring.md)**

