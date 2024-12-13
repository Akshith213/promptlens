# PromptQL with Pagila Database

This repository demonstrates how to set up and use **PromptQL** with the **Pagila** database, a PostgreSQL sample database. The guide walks you through the process of setting up the environment, integrating PromptQL, and querying the Pagila database interactively.

## Prerequisites

Before you begin, ensure you have the following:

- **Hasura Account**: [Sign up or log in](https://hasura.io/).
- **Docker**: Installed on your machine. [Download Docker](https://www.docker.com/get-started).
- **DDN CLI**: Install by running the following command:

  ```sh
  curl -L https://graphql-engine-cdn.hasura.io/ddn/cli/v4/get.sh | bash
  ```


## Setup Instructions

### 1. Validate Installation

Verify that the DDN CLI is correctly installed by running:

```sh
ddn doctor
```


### 2. Start a New PromptQL Project

Initialize a new project and navigate into the project directory:

```sh
ddn supergraph init my-assistant --with-promptql
cd my-assistant
```

### 3. Add Your API Key

Add your Anthropic API key to the `.env` file:

```sh
echo 'ANTHROPIC_API_KEY=your-anthropic-api-key' >> .env
```

### 4. Add the PostgreSQL Connector

Set up a PostgreSQL connector:

```sh
ddn connector init mypostgres -i
```

When prompted:

- Select **hasura/postgres**.
- Skip other fields by pressing **Enter**.

The CLI will provide instructions for running a local PostgreSQL instance. Follow these steps:

#### Run the Provided Docker Compose Command

```sh
docker compose -f app/connector/mypostgres/compose.postgres-adminer.yaml up -d
```

### Setting Up the Pagila Database

#### 1. Download Pagila Schema and Data

Run the following commands to download the schema and data:

```sh
curl -o ./app/connector/mypostgres/pagila-schema.sql https://raw.githubusercontent.com/devrimgunduz/pagila/master/pagila-schema.sql
curl -o ./app/connector/mypostgres/pagila-data.sql https://raw.githubusercontent.com/devrimgunduz/pagila/master/pagila-data.sql
```

### 2. Import the Schema and Data

Load the schema and data into the database:

```sh
cat app/connector/mypostgres/pagila-schema.sql | docker exec -i mypostgres-postgres-1 psql -U user -d dev
cat app/connector/mypostgres/pagila-data.sql | docker exec -i mypostgres-postgres-1 psql -U user -d dev
```

## Integrating with PromptQL

### 1. Introspect the Data Source

Analyze the database schema:

```sh
ddn connector introspect mypostgres
```

### 2. Add Resources

Generate models, commands, and relationships for the database:

```sh
ddn model add mypostgres '*'
ddn command add mypostgres '*'
ddn relationship add mypostgres '*'
```

### 3. Build the Supergraph

Build the supergraph locally:

```sh
ddn supergraph build local
```

### 4. Create a DDN Project

Initialize a new project:

```sh
ddn project init
```

### 5. Start the Supergraph

Run the supergraph locally:

```sh
ddn run docker-start
```

## Querying the Database

### Access the DDN Console

Open the DDN Console:

```sh
ddn console --local
```

In your browser, navigate to the URL provided by the CLI to start querying the Pagila database interactively.

### Sample Queries

#### Find Top Rented Films

```text
Show the top 5 rented films.
```


