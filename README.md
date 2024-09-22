# Realtime Streaming Election System

This repository contains the code for a realtime election voting system. The system is built using Python, Kafka, Spark Streaming, Postgres and Streamlit. The system is built using Docker Compose to easily spin up the required services in Docker containers.

## System Components

- **main.py**: This Python script handles creating the necessary tables in PostgreSQL (candidates, voters, and votes). It also sets up the Kafka topic, replicates the votes table into the Kafka topic, and contains logic to consume votes from Kafka and produce data to the voters_topic.
- **voting.py**: This script consumes votes from the Kafka topic (voters_topic), generates voting data, and produces it back to Kafka under the votes_topic.
- **spark-streaming.py**: This script processes votes from the votes_topic on Kafka, enriches the data by interacting with PostgreSQL, aggregates the votes, and outputs the data to specific Kafka topics.
- **streamlit-app.py**: This script consumes aggregated voting data from Kafka and PostgreSQL, and displays it in real-time using the Streamlit web application framework.

## Setting up the System

The system can be easily deployed using Docker Compose to set up Zookeeper, Kafka, and PostgreSQL in Docker containers.

### Prerequisites

- Python 3.9 or above
- Docker Compose
- Docker

### Steps to Run

1. Clone this repository.
2. Navigate to the root containing the Docker Compose file.
3. Run the following command:

```bash
docker-compose up -d
```

This command will start Zookeeper, Kafka and Postgres containers in detached mode (`-d` flag). Kafka will be accessible at `localhost:9092` and Postgres at `localhost:5432`.

##### Additional Configuration

If you need to modify Zookeeper configurations or change the exposed port, you can update the `docker-compose.yml` file according to your requirements.

### Running the App

1. Install the required Python packages using the following command:

```bash
pip install -r requirements.txt
```

2. Creating the required tables on Postgres and generating voter information on Kafka topic:

```bash
python main.py
```

3. Consuming the voter information from Kafka topic, generating voting data and producing data to Kafka topic:

```bash
python voting.py
```

4. Consuming the voting data from Kafka topic, enriching the data from Postgres and producing data to specific topics on Kafka:

```bash
python spark-streaming.py
```

5. Running the Streamlit app:

```bash
streamlit run streamlit-app.py
```
