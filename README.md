This repository provides ready-to-run demos and quick-start examples for the built-in AI/ML features available in Confluent Cloud for Flink (e.g., integrations with LLMs, embeddings, etc.).

The goal is to let you try these capabilities in just a few minutes without needing to read extensive documentation.

Prerequisites to run the demos:

* A running Confluent Cloud cluster
* A Flink Compute Pool in the same region
* (For some demos) an OpenAI API key

Once those are set up, you can copy-paste and run the examples directly in the SQL Workspace of your Flink Compute Pool.

The bolded entries currently have examples in the repo. 

There are currently several commands that allow you to work with AI inside of Flink: 

**CREATE_CONNECTION** -- Connect to various external model hosting platforms (Includes model providers as well as hyperscaler model hosting platforms)  
**CREATE_MODEL** -- Allows you to create a model within Flink using a connection to an external model hosting platform. 

There are currently 7 AI/ML functions specifically for Confluent Flink:

**AI_COMPLETE:** Generate text completions.  
**AI_EMBEDDING:** Create embeddings.  
**AI_FORECAST:** Forecast trends.  
AI_TOOL_INVOKE: Invoke MCP tools.  
AI_RUN_AGENT: Execute a streaming agent.  
**ML_DETECT_ANOMALIES:** Detect anomalies in your data.  
ML_EVALUATE: Evaluate the performance of an AI/ML model.  
**ML_PREDICT:** Run a remote AI/ML model for tasks like predicting outcomes, generating text, and classification.  

Contact: Taylor Bliszcz @ tbliszcz@confluent.io

<!--

How to get a stats model running in sagemaker and calling it from a stream. (Pull from huggingface, reccomendation type model)


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

-->
References:   
https://docs.confluent.io/cloud/current/ai/builtin-functions/overview.html  
https://docs.confluent.io/cloud/current/flink/reference/functions/ml-preprocessing-functions.html
