# Week 0 — Billing and Architecture

## Required Homework

### Install AWS CLI

Installing the AWS CLI by following the instruction

 
   ```
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
    
   ```
   
I installed the AWS CLI via Gitpod by folowwing the instruction from [AWS Documentation Page](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

I am able to use Gitpod or Github Codespaces; however, I have quite a lot of problem with Gitpod due to my mistakes.  

When learning how to generate Credentials, Budget and Billing Alarm via CLI, I did not commit my ```gitpod.yml```  after the instalation through the Gitpod.  After completing the whole exercises through Gitpod, I was unable to commit the code to my Github repository (the process of commiting keep loading-See picture)
   
  ![Gitpod Commit Issue](![gitpod comit](https://user-images.githubusercontent.com/93460271/219847154-3e45694f-750e-4793-8fa4-c9db514acb7c.png)
   
To fix that, I go back and watch the tutorial and re-do the whole thing from the beginning; that is the moment that I figure out that  I made mistake.  Lesson learned!!!

From the video, I learned so much more information about the AWS CLI. I have never used the CLI before; therefore, this is a really good chance for me to know and learn about AWS.

### Creating Admin user

For creating Admin user by manually using AWS console, I have no issue with this because I have learned and read about the feature before.

### Creating Billing alarm

![Billing Alert](![Billing_Alert](https://user-images.githubusercontent.com/93460271/219847256-2b162281-0793-450e-974c-220a0d0aceed.png)

I would like to try to create Billing Alert using the AWS CLI through Gitpod.  I learned how to create it through Gitpod, and it is pretty awsome for me to know how to use the CLI. 
### Creating Budgets Alert

![Budget Alert](![Budget_Alert](https://user-images.githubusercontent.com/93460271/219847325-b3264c55-94fb-4e7c-97cb-58675231b915.png)

### Using CloudShell

This is my first time I have used the CloudShell, I will definitely go to AWS document to read more and do some fun work about this feature

![Using CloudShell](![CloudShell_updated](https://user-images.githubusercontent.com/93460271/219874711-bd432a46-1d88-49e2-9f5e-3a82ecbed6b7.png)

### Recreate the Conceptual Design in Licud chart

Before the bootcamp started, I was wondering that how can I draw the icons of the AWS.  I saw a lot of people that they drew the chart with pretty icons, but I have no idea how to draw the icons.  Then, when attending the bootcamp, I just learned that they use tools to draw those icons.  I am excited to play around on the Lucid.app
![Proof of Conceptual Design](![Conceptual Diagram](https://user-images.githubusercontent.com/93460271/219847072-e440688d-6931-4468-a4da-fa0c7d821281.png)

[Lucid charts Shart Link_1](https://lucid.app/lucidchart/f8231132-794b-40e0-b079-91ffeb0e872f/edit?viewport_loc=-261%2C-76%2C2994%2C1495%2C0_0&invitationId=inv_a44879f7-9130-4512-9b2d-f449b2db1e4f)

### Recreate Logical Design in Lucid chart

Before the bootcamp started, I was wondering that how can I draw the icons of the AWS.  I saw a lot of people that they drew the chart with pretty icons, but I have no idea how to draw the icons.  Then, when attending the bootcamp, I just learned that they use tools to draw those icons.  I am excited to play around on the Lucid.app

![Proof of Logical Diagram](![Logical Diagram](https://user-images.githubusercontent.com/93460271/219848482-ba3f1af8-2183-49aa-9dbc-21323599b6fa.png)

[Lucid charts Share Link_2](https://lucid.app/lucidchart/4cd7cc66-04b2-46a8-a8b9-168fbd504a1a/edit?viewport_loc=-372%2C-29%2C2994%2C1495%2CQIkxwmBpUK~h&invitationId=inv_e9954ac4-9ab9-4f8f-87a1-a387d3879541)

### Spend and Security Considerations

I gain more knowledge about the Spend and Security Considerations, especially the Security Considerations.  Before watching Ashish's video, I just only use my Root User to access my account because I think my account is safe with the enabled MFA.  From now, I will use the IAM Account to access my account to be more secured.  

For the Spend Considerations, I never know that AWS will start charging me money if I create more that two bugets in the account; I used to think that I can create as much budgets as I can inside the AWS account.  One more knowledge to gain in Week 0.  

### Homework Challenges 

I will add API Gateway between Frontend and Backend application so we can monitor, manage, and secure the the APIs that comunicate between Frontend and Backend Application

![Adding API Gateway](![Adding API Gateway](https://user-images.githubusercontent.com/93460271/219874401-5c24b8b1-1985-4824-b04f-a97db41659f8.png)

For this, I did not do much about the design due to lack of information (I am a beginer but no excuse!!!), but I will continue learning and try my best to be involved in the challenge as much as I can
