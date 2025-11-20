This repo contains demos and quick starts for the various built in AI/ML functions that are present in Confluent Cloud for use with Flink. To get started esure you have a confluent cluster started with a Flink compute pool located in the same region. Once it is running the above demos can be ran to get an idea of what is possible with Flink in Confluent Cloud or as a quick start. 

There are currently several support functions that allow you to connect to external sources using Flink in Confluent Cloud: 

CREATE_CONNECTION -- Allows you to connect to various external model hosting platforms. As of 11/26, the following are supported: AWS Bedrock, AWS Sagemaker, Azure Machine Learning (Azure ML), Azure OpenAI, Google AI, OpenAI, Vertex AI  

CREATE_MODEL -- Allows you to create a model within Flink using a connection to an external model hosting platform. 

There are currently 7 AI/ML functions specifically for Confluent Flink. The ones bolded are the ones currently with quick starts located above in the .md files 

**AI_COMPLETE: Generate text completions.**  
AI_EMBEDDING: Create embeddings.  
**AI_FORECAST: Forecast trends.**  
AI_TOOL_INVOKE: Invoke MCP tools.  
**ML_DETECT_ANOMALIES: Detect anomalies in your data.**  
ML_EVALUATE: Evaluate the performance of an AI/ML model.  
ML_PREDICT: Run a remote AI/ML model for tasks like predicting outcomes, generating text, and classification.

There are also 12 Machine Learning preprocessing functions: 

ML_BUCKETIZE: Bucketizes numerical values into discrete bins based on split points.  
ML_CHARACTER_TEXT_SPLITTER: Splits text into chunks based on character count and separators.  
ML_FILE_FORMAT_TEXT_SPLITTER: Splits text into chunks based on specific file format patterns.  
ML_LABEL_ENCODER: Encodes categorical variables into numerical labels.  
ML_MAX_ABS_SCALER: Scales numerical values by their maximum absolute value.  
ML_MIN_MAX_SCALER: Scales numerical values to a specified range using min-max normalization.  
ML_NGRAMS: Generates n-grams from an array of strings.  
ML_NORMALIZER: Normalizes numerical values using p-norm normalization.  
ML_ONE_HOT_ENCODER: Encodes categorical variables into a binary vector representation.  
ML_RECURSIVE_TEXT_SPLITTER: Splits text into chunks using multiple separators recursively.  
ML_ROBUST_SCALER: Scales numerical values using statistics that are robust to outliers.  
ML_STANDARD_SCALER: Standardizes numerical values by removing the mean and scaling to unit variance.  

References:   
https://docs.confluent.io/cloud/current/ai/builtin-functions/overview.html  
https://docs.confluent.io/cloud/current/flink/reference/functions/ml-preprocessing-functions.html
