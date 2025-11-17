This repo contains demos and quick starts for the various built in AI/ML functions that are present in Confluent Cloud for use with Flink. To get started esure you have a confluent cluster started with a Flink compute pool located in the same region. Once it is running the above demos can be ran to get an idea of what is possible with Flink in Confluent Cloud or as a quick start. 

There are currently several support functions that allow you to connect to external sources using Flink in Confluent Cloud. 

CREATE_CONNECTION -- Allows you to connect to various external model hosting platforms. As of 11/26, the following are supported: AWS Bedrock, AWS Sagemaker, Azure Machine Learning (Azure ML), Azure OpenAI, Google AI, OpenAI, Vertex AI  

CREATE_MODEL -- Allows you to create a model within Flink using a connection to an external model hosting platform. Models are 'imported' and not trained. All training must be done before the model is imported (for example the model is created in AWS and then imported to Flink). 

There are currently 7 AI/ML functions specifically for Confluent Flink. The ones bolded are the ones currently with quick starts located above in the .md files 

**AI_COMPLETE: Generate text completions.**  
AI_EMBEDDING: Create embeddings.  
**AI_FORECAST: Forecast trends.**  
AI_TOOL_INVOKE: Invoke MCP tools.  
**ML_DETECT_ANOMALIES: Detect anomalies in your data.**  
ML_EVALUATE: Evaluate the performance of an AI/ML model.  
ML_PREDICT: Run a remote AI/ML model for tasks like predicting outcomes, generating text, and classification.

References: 
https://docs.confluent.io/cloud/current/ai/builtin-functions/overview.html
