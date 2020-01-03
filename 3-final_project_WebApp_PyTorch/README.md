# SageMaker Deployment Project

The notebook and Python files provided here, once completed, result in a simple web app which interacts with a deployed recurrent neural network performing sentiment analysis on movie reviews (IMDB set).

The main difference to the sentiment analysis project in the `deployment` folder is that we use a custom model (RNN) istead of a provided algorithm (XGBoost) and a custom inference code. This also alows us to use different pre-processing of the input.