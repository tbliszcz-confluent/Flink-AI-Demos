Once we have a model, we want to be able to test its accuracy. ML_EVALUATE() returns different metrics depending on what type of model we are trying to evaluate. 

The funtion works with Classification, Clustering, Embedding, Regression, and Text Generation models. In this example we will use a text generation sentiment analysis model that was used elsewhere in this repo. 

What is returned is a little different depending on the model type. As we are running this against what we consider a text generation model, we will get: Mean BLEU (bilingual evaluation understudy), Mean ROUGE (Recall-Oriented Understudy for Gisting Evaluation), and Mean Semantic Similarity (Check out the documentation for more information).

As we are routing the text to OpenAI for inference, the accuracy and results will be determined by the LLM. For more information on anything, please check out the documentation link at the end of this page. 

As earlier, we want to create a connetion to OpenAI: 

```
CREATE CONNECTION openai_completions
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com/v1/chat/completions',
    'api-key' = '<OpenAI_API_KEY>'
  );
```
 
Next we want to create our model: 

```
CREATE MODEL sentiment_analysis
  INPUT (input STRING)
  OUTPUT (sentiment STRING)
WITH(
    'provider' = 'openai',
    'openai.connection' = 'openai_completions',
    'openai.system_prompt' = 'Please give me the sentiment of the sentence as Positive, Neutral, or Negative',
    'task' = 'text_generation'
  );
```

Creating the table where information to run ML_EVALUATE is stored. Label is the input string (to find the sentiment of) and res is what the result should be. 

```
CREATE TABLE eval_data (
    label STRING, 
    res STRING
);
```

Insert the data into eval_data. This is where we insert the labal (string to get sentiment of) and the res (what the output should be)

```

INSERT INTO eval_data (label, res) VALUES
  ('I love the new features in this app, they''re amazing!', 'Positive'),
  ('The customer service was rude and completely unhelpful.', 'Negative'),
  ('The product works as described, nothing special.', 'Neutral'),
  ('Delivery was so fast, I''m thrilled!', 'Positive'),
  ('This software crashes every single time I use it.', 'Negative'),
  ('The weather is pleasant for a walk today.', 'Positive'),
  ('I''m so disappointed with this item''s poor quality.', 'Negative'),
  ('The movie was a fantastic experience, highly recommend!', 'Positive'),
  ('My internet is painfully slow right now.', 'Negative'),
  ('The restaurant has nice decor but average food.', 'Neutral'),
  ('I''m so excited for the concert this weekend!', 'Positive'),
  ('The assembly instructions were confusing and unclear.', 'Negative'),
  ('This book is a masterpiece, couldn''t stop reading!', 'Positive'),
  ('The hotel room was dirty and uncomfortable.', 'Negative'),
  ('The support team responded quickly, very helpful.', 'Positive'),
  ('The new app update made everything worse.', 'Negative'),
  ('The amusement park was so much fun!', 'Positive'),
  ('The product broke after just a few days.', 'Negative'),
  ('The presentation was clear and informative.', 'Neutral'),
  ('I''m frustrated with the constant shipping delays.', 'Negative');

```

Finally, we want to run the ML_EVALUATE() funtion to get the results: 

```
SELECT ML_EVALUATE(`sentiment_analysis`, label, res) FROM `eval_data`;
```

We should get something similar to {MB=0.8125, MR=0.825, MSS=0.8125}. This is updated in real time 

Resources: 
https://docs.confluent.io/cloud/current/flink/reference/functions/model-inference-functions.html#ml-evaluate
