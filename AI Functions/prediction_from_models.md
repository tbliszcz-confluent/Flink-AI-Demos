This example shows how we can use the ML_PREDICT() function to call a model that we have created inside Flink. 

In order to run this demo properly you will need an OpenAI Api key, which can be gotten here: https://platform.openai.com/api-keys . There needs to be a few dollars deposited on the account. 

Our first step is to create the connection to OpenAI. This uses CREATE CONNECTION along with the type, endpoint, and API key to create a connection to OpenAI. 

```
CREATE CONNECTION openai_connection
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com/v1/chat/completions',
    'api-key' = '<OpenAI_API_KEY>'
  );
```

We then create a model. This is the same model that was created in the Sentiment Analysis demo. We are querying an LLM to find out the sentiment of a piece of text. 
  
CREATE MODEL is used, along with showing the input/output types, and the config to be able to use the OpenAI connection. 

```
CREATE MODEL sentiment_analysis
  INPUT (input STRING)
  OUTPUT (sentiment STRING)
WITH(
    'provider' = 'openai',
    'openai.connection' = 'openai_connection',
    'openai.system_prompt' = 'Please give me the sentiment of the sentence as Positive, Neutral, or Negative',
    'task' = 'text_generation'
  );
```

Finally, we run a query to run text against our model. We are using 'I am happy today!' as the text and running a lateral table command to get the result of the ML_PREDICT() command. 

```
SELECT *
FROM (VALUES ('I am happy today!')) AS t(text),
LATERAL TABLE(ML_PREDICT(sentiment_analysis, t.text)) AS p(sentiment);
```

You should get a result similar to showing the inputted text ('I am happy today!') and Positive as the sentiment. 

More documentation on the function can be found here: https://docs.confluent.io/cloud/current/flink/reference/functions/model-inference-functions.html#ml-predict
