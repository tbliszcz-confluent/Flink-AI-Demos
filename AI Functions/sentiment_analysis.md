This Quickstart allows you to run sentiment analysis on a subset of phrases inside of hosted Flink in Confluent Cloud. There are 20 sentences that when the script is ran allows you to see if the sentence has a positive, negative, or neutral sentiment. 

The demo should be able to be ran within any cluster and a Flink compute pool. If you enter the code directly as it is below, with the exception of the API key. It uses the AI_COMPLETE() function to get results.

AI Model Inference and Machine Learning Functions in Confluent Cloud for Apache Flink | Confluent Documentation 

NOTE: You will need to have your own OpenAPI API key to run. You can get one at https://platform.openai.com/api-keys. There must be a couple of dollars on the API key. 

 

1. Create the table (topic) that we will enter the phrases into. The tables in Flink are represented as topics in the selected Kafka cluster: 

```
CREATE TABLE phrases (
    sentence_id INT,
    sentence STRING
);
```

2. Create the phrases that we will use to get the sentiment of from an OpenAI model. If you want to change or add your own phrases, feel free to add your own in the (sentence_id, ‘sentence’) format,

```
INSERT INTO phrases (sentence_id, sentence) VALUES
  (1, 'I love the new features in this app, they''re amazing!'),
  (2, 'The customer service was rude and completely unhelpful.'),
  (3, 'The product works as described, nothing special.'),
  (4, 'Delivery was so fast, I''m thrilled!'),
  (5, 'This software crashes every single time I use it.'),
  (6, 'The weather is pleasant for a walk today.'),
  (7, 'I''m so disappointed with this item''s poor quality.'),
  (8, 'The movie was a fantastic experience, highly recommend!'),
  (9, 'My internet is painfully slow right now.'),
  (10, 'The restaurant has nice decor but average food.'),
  (11, 'I''m so excited for the concert this weekend!'),
  (12, 'The assembly instructions were confusing and unclear.'),
  (13, 'This book is a masterpiece, couldn''t stop reading!'),
  (14, 'The hotel room was dirty and uncomfortable.'),
  (15, 'The support team responded quickly, very helpful.'),
  (16, 'The new app update made everything worse.'),
  (17, 'The amusement park was so much fun!'),
  (18, 'The product broke after just a few days.'),
  (19, 'The presentation was clear and informative.'),
  (20, 'I''m frustrated with the constant shipping delays.');
```

3. Create the sentiment table that will be used to insert the output from the OpenAI model into: 

```
CREATE TABLE sentiment (
  sentence_id INT,
  sentiment STRING
);
```

4. Create the connection to OpenAI. This creates a connection object that will allow Flink to communicate with OpenAI.

```
CREATE CONNECTION openai_connection
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com/v1/chat/completions',
    'api-key' = '<OpenAI_API_KEY_GOES_HERE>'
  );
```

5. Then create the sentiment_analysis model with of Flink. In this model we give the connection information as well as the task and system prompt we want to use it for. 

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

6. From here we use an INSERT INTO statement to call the AI_Complete function along with the model and sentences we have to get the results. 

```
INSERT INTO sentiment
  SELECT
    p.sentence_id,
    a.sentiment
  FROM phrases AS p,
       LATERAL TABLE(AI_COMPLETE('sentiment_analysis', p.sentence)) AS a(sentiment);
```

7. At this point we are able to select * from the table (topic) and see the results that OpenAI gave us back. 


```
SELECT * FROM sentiment
```
