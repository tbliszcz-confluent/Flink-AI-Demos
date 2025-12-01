Embeddings in AI are dense, low-dimensional vector representations of high-dimensional data (like words, images, users, or items) that capture their semantic meaning and relationships. 

In more plain terms -- When you run a piece of data such as an image through an embedding model, you get a short list of numbers that act like a summary of what the picture shows. The numbers then let AI systems do things such as find similar pictures and compare it to other pictures. 

We will use a OpenAI embeddings model to get embeddings from text we send to it. However you can use other embedding models that are available. 

Like other quickstarts, you will need to get your own OpenAI API key. You must also put a couple of dollars on it to be able to pull from the OpenAI API. An OpenAI API Key can be created at: https://platform.openai.com/api-keys

1. Create a connection to OpenAI if you do not currently have one set up. The important thing to note here is that we are creating a connecton to hit the OpenAI Embeddings endpoint:

```
CREATE CONNECTION openai_embeddings
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com/v1/embeddings',
    'api-key' = '<OpenAI_API_KEY>'
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

4. Create the table we will input data to.

```
CREATE TABLE strings (id int, embedding_strings string)
```

5. Input data that we will use to get the embeddings of -- enter additional strings if you would like: 

```
INSERT INTO strings (id, embedding_strings) VALUES 
(1, 'The purple elephant danced under the neon moon.'),
(2, 'Whispering winds carried secrets through the ancient forest.'),
(3, 'Quantum cats juggled flaming pianos at midnight.'),
(4, 'Velvet shadows painted dreams on forgotten walls.'),
(5, 'Cosmic jellyfish sang operas to the wandering stars.')
```

6. View the table with the string and the embedding results:

```
SELECT * FROM strings, LATERAL TABLE(AI_EMBEDDING('openai_embed', embedding_strings))
```

You should see a lot of numbers. These are what embeddings look like once data is transformed. 


Related Documentation: 
https://docs.confluent.io/cloud/current/flink/reference/functions/model-inference-functions.html#ai-embedding
