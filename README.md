# developer-edition-docker-compose
Docker Compose configuration for rapidly getting started with running a bot using the Rasa Pro Developer Edition

Put the `docker-compose.yml` file in the root of your bot directory. This deployment assumes the following:
- That within the bot directory, the model you want to use is already trained and is the latest archive available in the `models` directory.
- That you have custom actions in a file called `actions.py` in the root of your bot directory.

Ensure your `endpoints.yaml` file contains the following:
```
action_endpoint:
  url: "http://action_server:5055/webhook"

tracker_store:
    type: SQL
    dialect: "postgresql" 
    url: "tracker_store:5432"  
    db: "rasa"  
    username: "rasa" # Ensure this matches POSTGRES_USER in the docker compose file.
    password: "rasa" # Ensure this matches POSTGRES_PASSWORD in the docker compose file.
```

Ensure you have a `.env` file in the root of your bot directory with the following contents:
```
RASA_PRO_LICENSE=<your Rasa Pro license goes here>
OPENAI_API_KEY=<your OpenAI API key goes here>
```

Deploy the bot using `sudo docker compose up`. 
You should be able to send it a message by sending a POST request: 

```
curl -XPOST http://<host>:5005/webhooks/rest/webhook -d '{"sender": "test", "message": "Hi"}'
```

You can configure your assistant to respond on other channels by 
modifying the `credentials.yml` file, see [here](https://rasa.com/docs/rasa-pro/connectors/messaging-and-voice-channels)
