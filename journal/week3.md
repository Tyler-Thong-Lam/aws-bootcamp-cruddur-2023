# Week 3 — Decentralized Authentication

## Authenticate using Amazon Cognito

### Create Cognito

AWS Cognito: Providing authentication, authorization, and user management for the web and mobiles applications

[What is Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)


I create the Amazon Cognito by following the instructions on the project bootcamp video and the document 

[Creating AWS Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-user-pool.html)


Step 1: 

[Create Cognito](![create cognito](https://user-images.githubusercontent.com/93460271/224466465-624604c8-ebaa-4a36-a19b-f7b42d92546b.png)

Step 2: 

[Create Password Security and MFA](![PW policy and MFA](https://user-images.githubusercontent.com/93460271/224466501-7a5b5fef-4fe3-4cfa-9b38-38a90994411e.png)

In this step, we won't set up the MFA for the Cognito cause it will cost money

Step 3: 

Verifying the email and adding attributes 

[Verifying the email and adding attributes](![use email to verify and add attributes](https://user-images.githubusercontent.com/93460271/224466632-f918dc86-40fd-4b8c-9266-feaeb4173ddf.png)

[Prefered Attributes](![attributes](https://user-images.githubusercontent.com/93460271/224466811-365496de-3f79-4d21-b1d0-99a593931079.png)

Step 4: 
Configure Email

[Configuring email for Cognito](![configure email](https://user-images.githubusercontent.com/93460271/224466776-2c4d8201-c27a-41ab-92be-41923b672be8.png)

Step 5: 

Initializing app client

We need to pick a name and add it to the ``` App client name ```

[Initialize app client](![initial app client](https://user-images.githubusercontent.com/93460271/224466842-6b8f6a5a-d907-467b-94f7-f517c5b8cccd.png)

Step 6:  
Confirming all the set-up for Amazon Cognito

After reviewing the setting and completing, we will have the Cognito account 

[Example](![AWS Cognito email confirm](https://user-images.githubusercontent.com/93460271/224466945-7ad23fc3-e9ab-4089-b841-0354282981c0.png)

## Implementing Signin Page, Signup Page, Confirnmation Page

We experience error when implementing the code in the Signin Page
 
[Signin Error](![Signin Error](https://user-images.githubusercontent.com/93460271/224468626-98f0c7ad-2f4b-4d85-b666-f92f6b6afc17.png)

However, We are able to fix the problem then we export better result 

[After fixxing the error](![UI after signin](https://user-images.githubusercontent.com/93460271/224468730-716ce730-b0b1-418f-afd2-664d98de6062.png)


## Implement Recovery Page

[](![Cruddur reset code](https://user-images.githubusercontent.com/93460271/224468819-4be50642-5a52-4730-a222-e9262f134416.png)

We receive the verification email

[Verification mail](![Verification Code](https://user-images.githubusercontent.com/93460271/224469110-72b5423a-c425-4068-931e-0aec1ab8a618.png)




