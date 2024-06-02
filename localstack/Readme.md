# Local Stack

This repository contains the docker compose files and instructions for starting and connecting with several localstack services.

## How to use

1. Clone the repository
2. Configure the services you want to use on the SERVICES variable in docker-compose.yaml
3. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)


## Connect with localstack (AWS CLI)

Add enpoint-url modifier to aws cli commands to indicate the endpoint API. Example:

```bash
aws --endpoint-url=http://localhost:4566 --region eu-west-1 s3 ls
```

Every instance of a container is assigned to a region. Localstack Community does not persists data :-(

Additionally you can configue the environment variable AWS_ENDPOINT pointing to the localstack API endpoint: 

```bash
export AWS_ENDPOINT_URL=http://localhost:4566
```

### Example: S3

Create a bucket

By now only can be created on us-east-1

```bash
aws --endpoint-url=http://localhost:4566 --region us-east-1 s3api create-bucket --bucket test-bucket
```

View buckets

```bash
aws --endpoint-url=http://localhost:4566 --region eu-west-1 s3 ls
```


### Example: SQS

View queues

```bash
aws --endpoint-url=http://localhost:4566 --region eu-west-1 sqs list-queues
```

Create queue

```bash
aws --endpoint-url=http://localhost:4566 --region eu-west-1 sqs create-queue --queue-name CaffeineHub
```

Response:

```json
{
    "QueueUrls": [
        "http://sqs.eu-west-1.localhost.localstack.cloud:4566/000000000000/CaffeineHub"
    ]
}
```



