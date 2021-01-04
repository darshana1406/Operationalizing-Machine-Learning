# Operationalizing Machine Learning

* This project is part of the Udacity Azure ML Nanodegree. In this project, using Azure ML, a machine learing model is created using AutoML, deployed with Azure Container Instance(ACI) and consumed. A pipeline is created and published which can be used whenever the whole process needs to repeated.

* The dataset named 'Bank marketing dataset' contains data about a Telemarketing strategy implemented by a bank. The aim is to predict if a client would subscribe to a term deposit.

## Architectural Diagram

![Architectural Diagram](Images/Architectural%20Diagram.jpg)

## Key Steps
1. Creating a model
    * Bank marketing dataset is aquired from the url and registered as a Tabular dataset.
    ![1a](./Images/1a.png)
    
    * A compute cluster with a maximum of 6 nodes called 'bm-compute' is created.
    * An AutoML experiment under the name 'bm-exp1' is performed with Bank marketing dataset. The primary metric which will be optimized in the experiment is Accuracy.
    ![1b](./Images/1b.png)
    
    * The best model is Voting Ensemble with an accuarcy around 92%. This model is deployed as 'bm-best-model' using Azure Container Instance(ACI).
    ![1c](./Images/1c.png)
 
2. Enabling Application Insights/Logging
    * Logs are used to monitor and detect problems in applications. Running ```logs.py``` enables logging for the deployed model. In the screenshot below "Application Insights enabled" is True.
    ![2a](./Images/2a.png)
    
    * Logs from the deployed model.
    ![2b](./Images/2b.png)
    
3. Swagger
    * Swagger helps in designing and consuming an API. swagger.sh will download the latest swagger container and run it on port 9000. "http:\\localhost:9000" will display the Swagger UI. 
    * `swagger.json` which is provided by Azure for all deployed models is downloaded. `serve.py` will start a Python server on port 8000. Looking it up on swagger will show the input, output and other information of the API.
    
    ![3a](./Images/3a.png)
    
4. Consuming Model Endpoint
    * `endpoint.py` contains two sets of inputs in the form of JSON. This input will be fed to the deployed model at the scoring uri using the api and the result will also be obtained in the form of JSON.
    
    ![4a](./Images/4a.png)
    
5. Pipeline
    * This pipeline automates the AutoML process and saves a lot of time. Publishing this pipeline creates a REST Endpoint which can be triggered via a HTTP request.
    
    1. Previously created compute cluster is attached to the same experiment 'bm-exp1' in workspace.
    
    2. Bank marketing dataset is loaded in a Tabular Dataset.
    
    3. AutoML run is configured using an `AutoMLConfig` object and outputs are defined using `TrainingOutput`. An AutoML step is created.
    
    4. A pipeline object is created with AutoML step and submitted.
    
    5. Finally the pipeline is publisehed and an endpoint is created. The pipeline can be run when required using a http request. 
    
    * Pipeline Run
    ![5aa](./Images/5aa.png)
    
    * Published Pipeline endpoint
    ![5bb](./Images/5bb.png)
    
    * AutoML
    ![5cc](./Images/5cc.png)
    
    * Pipeline Overview
    ![5dd](./Images/5dd.png)
    
    * Run Details
    ![5ee](./Images/5ee.png)
    
    * Run using Pipeline Endpoint
    ![5ffa](./Images/5ffa.png)
    ![5ffb](./Images/5ffb.png)

## Screen Recording
* [Screencast](https://youtu.be/QDYTmJUNqr0)

## Future Work
* Deploying using AKS instead of ACI to leverage the full potential of containers offered by AKS.

* Benchmarking the endpoint using ApacheBench to measure the performance of the endpoint.

* Increasing the accuracy of the model by tinkering the parameters of AutoML and making the dataset balanced using SMOTE or other techniques.

* Creating a webpage to use the API easily.
