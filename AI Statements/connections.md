Creating a connection is the first step in being able to use your AI models. It allows you to allow the importation of models from various sources. 

For this example, and other examples, we will be using OpenAI as a demo. This connection will be needed to run other statements and functions inside of this repo.

The first step is to get an OpenAI key from https://platform.openai.com/api-keys. There must be money on the account to have the api key work. In actuality running things in this repo even several times will likely cost less than one cent. 

The next step is to run the command to create a connection. 

```
CREATE CONNECTION open_ai_connection
  WITH (
    'type' = 'openai',
    'endpoint' = 'https://api.openai.com',
    'api-key' = '<OpenAI_API_KEY>'
  );
```

After we have created the connection, we can run a SHOW connections statement to view all the other connections inside 

```
SHOW connections
```

If we want to dig further into the connection, we can get more information

```
DESCRIBE connection open_ai_connection
```

To delete the connection, we can use: 

```
DROP connection open_ai_connection
```

Official Documentation:  https://docs.confluent.io/cloud/current/flink/reference/statements/create-connection.html
