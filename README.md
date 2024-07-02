# retell-custom-llm-python-demo

This is a sample demo repo to show how to have your own LLM plugged into Retell.

This repo currently uses `OpenAI` endpoint. Feel free to contribute to make
this demo more realistic.

This repos is intended to be used together with Simli Avatar API - found here "[Link](https://github.com/simliai/simli-retell-frontend-reactjs-demo/tree/historicalCharacters)". 

It also uses unstructured to process unstructered data and feed it into Datastax rag database, which in turn is what is used to ensure that the interactions with historical characters are grounded in truth. 

We have a database that is public here - (url) 

If you want to create your own please follow the docs on datastax and unstrucutered to ingest more historical context and books.

## Steps to run in localhost

1. First install dependencies

```bash
pip3 install -r requirements.txt
```

2. Fill out the API keys in `.env`

3. In another bash, use ngrok to expose this port to public network

```bash
ngrok http 8080
```

4. Start the websocket server

```bash
uvicorn app.server:app --reload --port=8080
```

You should see a fowarding address like
`https://dc14-2601-645-c57f-8670-9986-5662-2c9a-adbd.ngrok-free.app`, and you
are going to take the hostname `dc14-2601-645-c57f-8670-9986-5662-2c9a-adbd.ngrok-free.app`, prepend it with `wss://`, postpend with
`/llm-websocket` (the route setup to handle LLM websocket connection in the code) to create the url to use in the [dashboard](https://beta.retellai.com/dashboard) to create a new agent. Now
the agent you created should connect with your localhost.

The custom LLM URL would look like
`wss://dc14-2601-645-c57f-8670-9986-5662-2c9a-adbd.ngrok-free.app/llm-websocket`


5. Run Rag database queryer api:

```cd queryer```
```python main.py```

This will start a fastapi that interfaces with datastax astradb, which is where we get the rag data to enrich the agent interactions.


## Run in prod

To run in prod, you probably want to customize your LLM solution, host the code
in a cloud, and use that IP to create agent.
