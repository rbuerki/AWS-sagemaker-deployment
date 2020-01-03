# SageMaker Deployment Project

The notebook and Python files provided here, once completed, result in a simple web app which interacts with a deployed recurrent neural network performing sentiment analysis on movie reviews (IMDB set).

It is a more sophisticated version of the IMDB sentiment analysis project in the `deployment` folder. The main difference is that we use a custom model (RNN) instead of a provided algorithm (XGBoost) and a custom inference code. This also alows us to use a different pre-processing of the input that happens not within the lambda function.