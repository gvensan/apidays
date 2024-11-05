
This repository contains how to use the solace-ai-connector for 
- A chat style interaction with LLM for an intelligent ticket classification
- A OpenAI RAG (Retrieval Augmented Generation) example using ChromaDB for getting recommendation for a patient's status based on the ingested vital data

## Setup

1. Clone the solace-ai-connector repo
```
git clone git@github.com:SolaceDev/solace-ai-connector.git
```
2. Clone this repostory inside the cloned repo `solace-ai-connector` 
```
cd solace-ai-connector
git clone git@github.com:gvensan/apidays.git
```
3. Inside the `apidays` folder, update the `setcloudbroker.env` and `setcloudopenai.env` with appropriate values for expected environment variables.

## Running Support Ticket Classification Application

**Start connector**
```
solace-ai-connector apidays_ticket_classification.yaml
```

**Start a receiver using *stm***

stm receive -t <USER_NAME>/> --config apidays-broker.json

**Send a new ticket event using *stm***

stm send -t <USER_NAME>/customer/tickets/new -f samples/ticket.json --config apidays-broker.json

>**Note:** <USER_NAME> correspond to the environment variable SOLACE_USER_NAME set in the `setcloudbroker.env` file.

**Observe the results on the receiver**
You should be able to see events on topics:
- New Ticket
- Updated ticket with sentiment and ticket classification 
- Updated ticket with priority 
- Updated ticket with escalation

## Running RAG Application - Patient Monitoring 

**Start connector**
```
solace-ai-connector apidays_patient_monitoring_rag.yaml
```

**Start a receiver using *stm***

```
stm receive -t <USER_NAME>/> --config apidays-broker.json
```

**Ingest Patient data using *stm***

```
stm send -t <USER_NAME>/patient/rag/data -f samples/patientsdata.json --config apidays-broker.json
```

**Send Patient query using *stm***

```
stm send -t <USER_NAME>/patient/rag/query -f samples/patientsquery.json --config apidays-broker.json
```


>**Note:** <USER_NAME> correspond to the environment variable SOLACE_USER_NAME set in the `setcloudbroker.env` file.

**Observe the results on the receiver**
You should be able to see events on topics:
- RAG Query and
- RAG Response
