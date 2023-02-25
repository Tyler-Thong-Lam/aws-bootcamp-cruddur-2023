# Week 1 — App Containerization

##Required Homework

###Adding the Notification feature to our app

- Adding the notification's actitivy to the service
- Adding the notification page in the frontend 

[Notification image](![Notification_1](https://user-images.githubusercontent.com/93460271/221334911-cebb0119-4712-4090-82d7-15ab28859124.png)

### Creating Dockerfile for Frontend and Backend

[Backend Dockerfile](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile)

[Frontend Dockerfile](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/frontend-react-js/Dockerfile)

### Using Docker to launch multiple apps
[Docker Compose](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)
[Docker_compose image](![docker](https://user-images.githubusercontent.com/93460271/221336016-eb897e8a-ad4c-49b7-8057-a259c18df187.png)

### Adding DynamoDB and Postgres

[Docker Compose](https://github.com/Tyler-Thong-Lam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)
[](![docker_postgres](https://user-images.githubusercontent.com/93460271/221336680-6d6de81d-10ec-4e23-88ce-9844a8c9977b.png)

- Testing DynamoDB
[DynamoDB Image](![dynamodb](https://user-images.githubusercontent.com/93460271/221336792-31c75183-f54f-4b95-ab83-c7f5b9b0c9d1.png)


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

- Testing Postgres
[Postgres Image](![postgress](https://user-images.githubusercontent.com/93460271/221336948-81f59971-4b5d-4252-bb36-e64e8bd94031.png)

 
 ## Homework Challenge
 
 ### Push and Tag image on DockerHub
 
-After creating account in DockerHub, then push the image
[Dockerhub Image](![dockerhub_code](https://user-images.githubusercontent.com/93460271/221344804-b5145983-8200-4daf-976f-ea20b24a8fa7.png)

[Proof of image on DockerHub](![dockerhub_image](https://user-images.githubusercontent.com/93460271/221344867-3cbe4a72-3c51-484a-826a-7f0a8d5d31c0.png)

 Installing Docker on local
 ** Insert Image**
 
 [Window install](https://docs.docker.com/desktop/install/windows-install/)
 
