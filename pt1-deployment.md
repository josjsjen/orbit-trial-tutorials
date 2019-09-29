# Challenge 1: Deploying a model

## The Challenge

Now, we are ready to put the trained model into production. However, it is not that simple.

For one, putting a model into production means that it needs to be wired up with the various IT systems that consume the model's predictions. For example, software developers will need to change the software in the call center to programmatically fetch monthly predictions from your model, likely through an API. You model is not that useful to anyone "outside of the lab" if it can't be consumed by operations. It'd be very time consuming and error-prone if you had to manually generate and deliver predictions.

Putting a model in production requires that you can reproduce the model that was arrived at in the development phase. A model is the final product of a training process that includes many moving parts that evolve at different rates. Very often teams cannot re-create a model for production because they've lost track of the version of code that created the model or the dependencies of the training code, which includes configurations, hard-coded values, libraries, meta-data, artifacts, etc...

## The Solution

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

Next, In the terminal, enter the following command then press the ‘Enter’ key (You can ignore the messages in the terminal):
```bash
foundations orbit serve start --project_name=orbit-trial --model_name=model-v1 --project_directory=./ --env=scheduler
```
_If you are on Windows OS, you need to press `Shift`+`Insert` to paste command into terminal, instead of `Ctrl`+`v`_

The model has been deployed to our trial environment running on GCP.

<details>
  <summary>What does this orbit feature do? (click to expand)</summary>
<br>

Orbit automatically packages the code and model into a "model package", which is a microservice that can be accessed by any IT systems using REST API. The entry points specified in the `foundations_package_manifest.yaml` file will become API endpoints that can receive requests and respond with output from/to other IT systems.

For example, in the yaml file above we tell Orbit to create an API endpoint for this model package called "predict". Requests to this API endpoint will get passed into the predict(...) function, which Orbit will execute. In the file we are also defining two other API endpoints, "evaluate" and "recalibrate", we will discuss them later.

Why is this important?
* The packaging aspect of this feature ensures reproducibility of models
* The serving aspect of this feature makes it easy to turn your model into a service that can be communicated with via API, making it easier to put models in production

-------------------------------------------------------------------------------------------------------------------------
</details>

<details>
  <summary>FAQ: Does Orbit ONLY work on specific cloud infrastructure like Google Cloud Platform?</summary>
  <br>

  The answer is NO. **Orbit is platform agnostic**, meaning it can be configured to work on any insfrastructures that meet the needs of your team, including major cloud platforms (AWS, Azure, GCP), on-premise clusters, or a hybrid of infrastructures.

-------------------------------------------------------------------------------------------------------------------------
</details>

Now please go to the GUI using the other link that we shared with you
* Once you enter the GUI, you will see 1 project in the landing page (you might need to refresh the page if you do not see the project right away)
* Click on the **orbit-trial** project
* You will land on the **Model Management** tab, where you will see information about the model you just deployed

## Congratulations! You've completed this challenge!

To recap what you just achieved: you learned how to take model code, a trained model, and dependencies, package them into something reproducible and deploy it as a micro-service that can interact with other systems via API.

Being non-instrusive to an existing data science workflow is a key focus for Orbit. No matter how you develop your models, you can use Orbit to deploy them and manage them after deployment.

**Please proceed to the next challenge:** [Model Performance Monitoring](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/pt2-performance-monitoring.md)

If you would like to go back:
[Overview](https://github.com/dessa-public/orbit-trial-tutorials/blob/2.0/hello-food-trial-instructions.md)



