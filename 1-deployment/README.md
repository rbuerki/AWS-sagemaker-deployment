# Model Deployment with Amazon SageMaker

Deploying ML models with Amazon SageMaker. The notebooks do not focus on the acutal projects (house price prediction and sentiment analysis, both with XGBoost), but on how to build and deploy models in the AWS cloud.

The **Boston housing project** goes through two approaches: high-level and detailed. It has also two additional notebooks focusing on hyperparameter tuning to find the best model conifugration (again high-level and detailed approach).

The **IMBD project** does no hyperparameter tuning but adresses 2 special concerns:
1. The endpoint that is created when we deploy a model using SageMaker (as in the boston housing project) is secured, meaning that only entities that are authenticated with AWS can send or receive data from the deployed model. This is a problem since authenticating for the purposes of a simple web app is a bit more work than we'd like.
2. The second obstacle is that our deployed model expects us to send it a review after it has been processed. That is, it assumes we have already tokenized the review and then created a bag of words encoding. However, we want our user to be able to type any review into our web app.

To solve these issues some additional Amazon services are used (see below for more info):
- **Amazon Lambda**
- **API Gateway**


## Code / Files

**Project Boston housing**
- `1a-deploy_high-level_boston_housing.ipynb`: Builds and deploys a model using the _high-level API_ of Amazon Sagemaker, where a lot of decisions are managed by Sagemaker in the background.
- `1b-deploy_detailed_boston_housing.ipynb`: Actually the same content as in 1a, but this time the _low-level API_ is used with much more manual coding and specification. This is easier to debug in case of problems.
- `1c-hyperparam-tuning_high-level_boston.ipynb`: Based on the same data and code as 1a, but with the additional step of hyperparameter tuning to find the best model config using the _high-level API.
- `1d-hyperparam-tuning__detailed_boston.ipynb`: Based on the same data and code as 1b, but with the additional step of hyperparameter tuning to find the best model config using the _low-level API.

**Project IMBD sentiment analysis - deployment for public web-app**
- `2a-deploy_web-app_IMBD_sentiment.ipynb`: Launches a little web app to get recommendation for individual user IDs. 
- `index.html`: This is the simple web interface used for entering new reviews as input to the model.


## On Lambda & Gateway API

As mentioned earlier, there are two obstacles we are going to need to overcome. The first is the security issue and the second is data processing. The way that we are going to approach solving these issues is by making use of Amazon Lambda and API Gateway.

The structure for our web app will look like the diagram below.

<img src="Web App Diagram.svg">

What this means is that when someone uses our web app, the following will occur.

To begin with, a user will type out a review and enter it into our web app.

Then, our web app will send that review to an endpoint that we created using API Gateway. This endpoint will be constructed so that anyone (including our web app) can use it.

API Gateway will forward the data on to the Lambda function

Once the Lambda function receives the user's review, it will process that review by tokenizing it and then creating a bag of words encoding of the result. After that, it will send the processed review off to our deployed model.

Once the deployed model performs inference on the processed review, the resulting sentiment will be returned back to the Lambda function.

Our Lambda function will then return the sentiment result back to our web app using the endpoint that was constructed using API Gateway.


#### AWS Lambda

In general, a Lambda function is an example of a 'Function as a Service'. It lets you perform actions in response to certain events, called triggers. Essentially, you get to describe some events that you care about, and when those events occur, your code is executed.

For example, you could set up a trigger so that whenever data is uploaded to a particular S3 bucket, a Lambda function is executed to process that data and insert it into a database somewhere.

One of the big advantages to Lambda functions is that since the amount of code that can be contained in a Lambda function is relatively small, you are only charged for the number of executions.

In our case, the Lambda function we are creating is meant to process user input and interact with our deployed model. Also, the trigger that we will be using is the endpoint that we will create using API Gateway.


#### AWS API Gateway

Essentially, API Gateway allows us to create an HTTP endpoint (a web address). In addition, we can set up what we want to happen when someone tries to send data to our constructed endpoint.

In our application, we want to set it up so that when data is sent to our endpoint, we trigger the Lambda function that we created earlier, making sure to send the data to our Lambda function for processing. Then, once the Lambda function has retrieved the inference results from our model, we return the results back to the original caller.


### Install

This project requires access to a notebook instance on AWS SageMaker to run the notebooks on.
