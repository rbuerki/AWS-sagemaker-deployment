# Recommender System with Amazon Personalize

Deploying ML models with Amazon SageMaker. The notebooks do not focus on the acutal projects (house price prediction and sentiment analysis, both with XGBoost), but on how to build and deploy models in the AWS cloud.

The IMBD project adresses 2 special concerns:
1. The endpoint that is created when we deploy a model using SageMaker (as in the boston housing project) is secured, meaning that only entities that are authenticated with AWS can send or receive data from the deployed model. This is a problem since authenticating for the purposes of a simple web app is a bit more work than we'd like.
2. The second obstacle is that our deployed model expects us to send it a review after it has been processed. That is, it assumes we have already tokenized the review and then created a bag of words encoding. However, we want our user to be able to type any review into our web app.

To solve these issues some additional Amazon services are used (see below for more info):
- **Amazon Lambda**
- **API Gateway**


### Code / Files

**Project Boston housing**
- `1a-deploy_high-level_boston_housing.ipynb`: Builds and deploys a model using the _high-level API_ of Amazon Sagemaker, where a lot of decisions are managed by Sagemaker in the background.
- `1b-deploy_detailed_boston_housing.ipynb`: Actually the same content as in 1a, but this time the _low-level API_ is used with much more manual coding and specification. This is easier to debug in case of problems.

**Project IMBD sentiment analysis - deployment for public web-app**
- `2a-deploy_web-app_IMBD_sentiment.ipynb`: Launches a little web app to get recommendation for individual user IDs. 
- `index.html`: This is the simple web interface used for entering new reviews as input to the model.


### Install

This project requires access to a notebook instance on AWS SageMaker.
