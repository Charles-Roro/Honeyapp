# Honeyapp
Using AWS, GCP, and Docker to create and monitor a Fake app.

First step is to create all the folders and files we will need for the project via the command line.
![Screenshot 2024-11-20 at 3 26 40 PM](https://github.com/user-attachments/assets/ad868eab-8c70-472b-8c51-31efeead19cf)

We will be using Graylog for this project. I'll add the link to the website for the Open version "[here](https://github.com/Graylog2/docker-compose/blob/main/open-core/docker-compose.yml)" Be sure to follow the 
guided instructions so there are now issues.


Now setting up the AWS terraform file. 2 IMPORTANT things to remember here are we do NOT want to use the default VPC in AWS and 2 when creating VMs in terraform with AWS you NEED to pay attention to the 
"AMI ID" for the OS you are using each region I.E 'us-east-1' and/or 'us-east-2' have different "AMI ID's" even if the OS is the same. I'll post a picture below.
![Screenshot 2024-11-20 at 3 49 45 PM](https://github.com/user-attachments/assets/801f099e-4eb9-4aff-ba39-8c3eea54fc15)

Copy that from your respective region and add it to the terraform file
![Screenshot 2024-11-20 at 5 38 13 PM](https://github.com/user-attachments/assets/8ebe758f-99a3-464b-9ef4-7b2fb55c97fa)

Next we will have snyk do an IaC test on this first part of are terraform code. (No major issues)
![Screenshot 2024-11-20 at 4 12 36 PM](https://github.com/user-attachments/assets/97186ede-4cf4-4d42-bbe3-6705888fa7e7)



Next we will move on to the GCP Windows server that will host our make believe "app"
![Screenshot 2024-11-20 at 4 34 48 PM](https://github.com/user-attachments/assets/01087ed5-9ed8-40d4-8d70-54ed4146d093)
Snyk scan shows 1 major issues. In these case we can ignore since this is for personal use and will be spun down. As well as shielded vm we want to scan this later and see the results.
![Screenshot 2024-11-20 at 4 36 50 PM](https://github.com/user-attachments/assets/01c7aa07-f076-41c2-a60d-bf23b74e076d)

Now for the website we will use AI to help create the html for the simple website(I'm not a webdev forgot all that from highschool haha)
![Screenshot 2024-11-20 at 5 15 53 PM](https://github.com/user-attachments/assets/d31ba0f5-f268-4239-908c-a645809e6c29)
![Screenshot 2024-11-20 at 5 16 16 PM](https://github.com/user-attachments/assets/56d81418-340e-4073-8647-d71c355ed2f0)
![Screenshot 2024-11-20 at 5 17 03 PM](https://github.com/user-attachments/assets/4f06b4e5-bd40-4eba-bd7a-898980559429)

Now we will run "terraform init" then plan then apply to spin up in our AWS and GCP consols
![Screenshot 2024-11-20 at 5 56 55 PM](https://github.com/user-attachments/assets/2f3dd50f-71ce-453b-b18a-0ed5457cdb5c)
"Terraform plan" looks good and no issues
![Screenshot 2024-11-20 at 6 01 26 PM](https://github.com/user-attachments/assets/13d16830-e2fa-4f23-8812-260b3bb7a99f)
Up and running!
![Screenshot 2024-11-20 at 6 19 57 PM](https://github.com/user-attachments/assets/9391211c-0b56-4453-8d68-1dc463f63df0)














