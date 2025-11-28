Embeddings in AI are dense, low-dimensional vector representations of high-dimensional data (like words, images, users, or items) that capture their semantic meaning and relationships. 

In more plain terms -- When you run a piece of data such as an image through an embedding model, you get a short list of numbers that act like a summary of what the picture shows. The numbers then let AI systems do things such as find similar pictures and compare it to other pictures. 

In this quickstart for consistency we will use a OpenAI embeddings model to get embeddings from text we send to it. However you can use other embedding models that are available. 

Like other quickstarts, you will need to get your own OpenAI API key. You must also put a couple of dollars on it to be able to pull from the OpenAI API. An OpenAI API Key can be created at: 

1. Create a connection to OpenAI if you do not currently have one set up. The important thing to note here is that we are creating a connecton to hit the OpenAI Embeddings endpoint:

```
CREATE CONNECTION openai_connection
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com/v1/embeddings',
    'api-key' = <OpenAI_API_KEY>'
  );
```

2. Create an embedding model. We set the connection to the connection created specifically for embeddings above as well as setting the task to embedding. The INPUT and OUTPUT properties are required: 

```
CREATE MODEL openai_embed
INPUT (text STRING)
OUTPUT (response ARRAY<FLOAT>)
WITH (
  'provider' = 'openai',
  'openai.connection' = 'openai_embeddings',
  'task' = 'embedding'
);
```

3. We can preview what embeddings will look like with the command below. The first string input is the model name (referenced above). The second string input is the string we are getting embeddings from and can be changed.:

```
SELECT * FROM AI_EMBEDDING('openai_embed', 'This is my text')
```

5. Create the table we will input data to.

```
CREATE TABLE embedding_data
VALUES
```

6. Input Data

```
INSERT INTO embedding_data VALUES

8. View results 


Related Documentation: 
https://docs.confluent.io/cloud/current/flink/reference/functions/model-inference-functions.html#ai-embedding
