# Model Updating with Amazon SageMaker

In these notebooks we are looking at updating an existing endpoint so that it conforms to a different endpoint configuration. There are many reasons for wanting to do this, the two that we will look at are:
1. performing an A/B test (boston housing project from before)
2. updating a model which is no longer performing as well (IMDB sentiment analysis project from before).


**Boston housing project**
We will look at performing an A/B test between two different models. Then, once we've decided on a model to use, updating the existing endpoint so that it only sends data to a single model without having to shut it down.

**IMBD sentiment analysis project**
For the second example, it may be the case that once we've built a model and begun using it, the assumptions on which our model is built begin to change. For instance, in the sentiment analysis examples that we've looked at our models are based on a vocabulary consisting of the 5000 most frequently appearing words in the training set. But what happens if, over time, the usage of words changes? Then our model may not be as accurate. When this happens we may need to modify our model, often this means re-training it. When we do, we'd like to update the existing endpoint without having to shut it down. 

### Install

This project requires access to a notebook instance on AWS SageMaker to run the notebooks on.
