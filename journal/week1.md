# Week 1 — App Containerization

## Adding the Notification feature to our app

- Adding the notification's actitivy to the service
- Adding the notification page in the frontend 

[Notification image](!![Notification_1](https://user-images.githubusercontent.com/93460271/221334911-cebb0119-4712-4090-82d7-15ab28859124.png)

## Creating Dcokerfile for Frontend and Backend
## Using Docker to launch multiple apps
## Adding DynamoDB and Postgres
- Testing DynamoDB

```
aws dynamodb create-table \
    --endpoint-url http://localhost:8000 \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --table-class STANDARD
    ```
 
