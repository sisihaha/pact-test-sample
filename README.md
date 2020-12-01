# Sample Project for Contract Testing via Pact

This project is a simple demonstration showing Contract Testing via Pact.

## Set up Pact Broker

Step 1 - Start PostgreSQL container

```
docker run --name pactbroker-db -e POSTGRES_PASSWORD=ThePostgresPassword -e POSTGRES_USER=admin -e PGDATA=/var/lib/postgresql/data/pgdata -v /var/lib/postgresql/data:/var/lib/postgresql/data -d postgres
```

Step 2 - Connect to container and execute psql and run SQL configuration scripts

```
docker exec -it pactbroker-db psql -U admin

CREATE USER pactbrokeruser WITH PASSWORD 'TheUserPassword';
CREATE DATABASE pactbroker WITH OWNER pactbrokeruser;
GRANT ALL PRIVILEGES ON DATABASE pactbroker TO pactbrokeruser;
```

Step 3 - Start PactBroker container

```
docker run --name pactbroker --link pactbroker-db:postgres -e PACT_BROKER_DATABASE_USERNAME=pactbrokeruser -e PACT_BROKER_DATABASE_PASSWORD=TheUserPassword -e PACT_BROKER_DATABASE_HOST=postgres -e PACT_BROKER_DATABASE_NAME=pactbroker -d -p 80:80 dius/pact-broker
```

## Publish Pact file from consumer to Pact Broker

Execute gradle task ```test``` in consumer project, and execute gradle task ```pactPublish```

## Verify provider via Pact file in Pact Broker and publish result

Execute gradle task ```pactVerify_Our_Provider``` in springboot-provider project with the following argument:
```-Ppact.verifier.publishResults=true```

## Reference

[Running Postgresql via Docker](https://github.com/DiUS/pact_broker-docker/blob/master/POSTGRESQL.md)

[Dockerised Pact Broker](https://github.com/DiUS/pact_broker-docker)

[Example JVM project for the Pact workshop](https://github.com/DiUS/pact-workshop-jvm)

[Pact](https://docs.pact.io/)