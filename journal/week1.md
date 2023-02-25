# Week 1 — App Containerization

##Required Homework

###Adding the Notification feature to our app

- Adding the notification's actitivy to the service
- Adding the notification page in the frontend 

[Notification image](!![Notification_1](https://user-images.githubusercontent.com/93460271/221334911-cebb0119-4712-4090-82d7-15ab28859124.png)

### Creating Dockerfile for Frontend and Backend

[Backend Dockerfile](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile)
[Frontend Dockerfile](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/frontend-react-js/Dockerfile)

### Using Docker to launch multiple apps
[Docker Compose](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)
[Dockerfile](![docker](https://user-images.githubusercontent.com/93460271/221336016-eb897e8a-ad4c-49b7-8057-a259c18df187.png)

### Adding DynamoDB and Postgres

**Insert image**
- Testing DynamoDB
- 
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
 
 ## Homework Challenge
 
