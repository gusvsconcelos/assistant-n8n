# n8n Self-Hosted Virtual Assistant (MVP)

This project is an **MVP of a fully self-hosted virtual assistant**, built with **n8n** and powered by **LLaMA 3.2:3b**, running locally via **Ollama** inside Docker containers.

The main goal is to provide a solid foundation for integrating an AI assistant into real services (such as WhatsApp, Telegram, etc.), while maintaining full control over infrastructure and data.

## Requirements

Before getting started, make sure you have the following installed:

* **Docker**
* **Docker Compose**

## Starting the environment

From the project root, run:

```bash
docker compose up -d
```

This command will:

* Build and start the **n8n** container
* Start the **PostgreSQL** container
* Start the **Ollama** container

## Downloading the Ollama model

Once all containers are running, pull the language model:

```bash
docker compose exec ollama ollama pull llama3.2:3b
```

This may take a few minutes depending on your internet connection.

## Accessing n8n

After everything is up, open your browser and go to:

```
http://localhost:5678
```

### Account setup

n8n will ask you to create an account.
You can use any name, email (even a fake one), and password.

If, for any reason, it asks you to log in directly, use:

* **Username:** `admin`
* **Password:** `admin`

## Connecting PostgreSQL to n8n

I recommend configuring the database before creating any workflows.

1. Go to **Credentials**

2. Click **Create Credential**

3. Select **Postgres**

4. Fill in the following values:

   * **Host:** `postgres`
   * **Database:** `n8n_data`
   * **User:** `n8n`
   * **Password:** `n8n`

5. Save and test the connection
   If it returns success, you are ready to proceed.

## Importing the workflows

Inside the `workflows/` folder, there are **three JSON files**.

For each one:

1. Create a new workflow in n8n
2. Use **Import from file**
3. Import the corresponding JSON file

## Initializing the database

After importing all workflows, you **must** run the following workflow:

```
setup_database
```

This workflow creates the required database structures used by the assistant.

## Interacting with the assistant

You can interact with the assistant via **curl** using the exposed n8n webhook:

```bash
curl -X POST http://localhost:5678/webhook-test/assistent \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello!", "userId": "user001"}' | jq '.'
```

* `message`: the user input message
* `userId`: a unique user identifier (used for context handling)

## Next steps

* Add a simple **frontend UI**
* Integrate with real services such as:

  * WhatsApp
  * Telegram
  * External APIs

The focus of this project is to serve as a foundation for **real-world, self-hosted AI assistants**, with full control over data, costs, and infrastructure.
