# Honeyapp
Using AWS, GCP, and Docker to create and monitor a Fake app.

First step is to create all the folders and files we will need for the project via the command line.

![Screenshot 2024-11-22 at 11 29 35 AM](https://github.com/user-attachments/assets/8fc7b738-fa2a-4017-a570-32b890370a7f)


We will be using Graylog for this project. I'll add the link to the website for the Open version "[here](https://github.com/Graylog2/docker-compose/blob/main/open-core/docker-compose.yml)" Be sure to follow the guided instructions so there are no issues. They provide the need yaml to get graylog started.
![Screenshot 2024-11-22 at 11 30 46 AM](https://github.com/user-attachments/assets/7ae5e413-f79c-40bf-b0f0-b80fb56016e5)
Next we will run the command "docker-compose up -d" in the folder containing our docker-compose.yml and .env files
![Screenshot 2024-11-22 at 11 50 25 AM](https://github.com/user-attachments/assets/a5e0b224-077a-463e-826b-f0b46806d140)
![Screenshot 2024-11-22 at 11 50 51 AM](https://github.com/user-attachments/assets/5e6adb93-a018-4e8a-a001-9c73117830d9)

Now lets check our localhost port 9000 to see if it's up and running(If you do this in the cloud whatever ip address you have assigned to the VM you would use that followed by ":9000")
![Screenshot 2024-11-22 at 11 52 21 AM](https://github.com/user-attachments/assets/d45487a8-6104-48db-8c1a-41c0b590392b)

Now Graylog is up and running we will login.
![Screenshot 2024-11-22 at 11 55 18 AM](https://github.com/user-attachments/assets/0f6af39c-1958-4cdb-8167-4fb6db736f96)

Now we just follow the instructions for basic set up. Now Graylog is up!
![Screenshot 2024-11-22 at 12 00 20 PM](https://github.com/user-attachments/assets/8ba3bed4-1ee7-4862-ba56-4d91a2682050)


Now setting up the AWS terraform file. 2 IMPORTANT things to remember here are we do NOT want to use the default VPC in AWS and 2 when creating VMs in terraform with AWS you NEED to pay attention to the 
"AMI ID" for the OS you are using each region I.E 'us-east-1' and/or 'us-east-2' have different "AMI ID's" even if the OS is the same. I'll post a picture below.(The Highlighted section). We need to make sure we have port 22 open in our Security group so we can use ansible to automate the installation of the plugins we need.
![Screenshot 2024-11-22 at 11 33 03 AM](https://github.com/user-attachments/assets/b599479b-04a1-42b6-8868-c586a715c7a9)


Copy that from your respective region and add it to the terraform file
![Screenshot 2024-11-22 at 11 31 25 AM](https://github.com/user-attachments/assets/234e5835-ab98-4b03-b072-bb082bbaf75d)


Next we will move on to the GCP Linux server that will host our make believe "app." We have to make sure in GCP to add a certain tag to the VM "http-server" as well as have port 80 open in or VPC so we can access the site. Like the AWS EC2 AM we need to have port 22 open so we can have ansible connect and automate the installation of the plugins we need.
![Screenshot 2024-11-22 at 12 04 07 PM](https://github.com/user-attachments/assets/7b999151-4314-49b3-88d6-8ad91dd2d440)
![Screenshot 2024-11-22 at 12 08 18 PM](https://github.com/user-attachments/assets/fbfe6b87-84e1-47b8-a3cb-9a3c7f896d48)

Next we will have snyk do an IaC test on this first part of our terraform code. (No major issues are 2 High issues are because we are allowing SSH from the whole internet. Normally you would want to restrict it to certain IPS of your/works network)
![Screenshot 2024-11-22 at 11 34 44 AM](https://github.com/user-attachments/assets/2efb75d0-d0ab-4742-a307-a2bd61a649b4)



Now for the website we will use AI to help create the html for the simple website(I'm not a webdev forgot all that from highschool haha)
![Screenshot 2024-11-22 at 12 11 24 PM](https://github.com/user-attachments/assets/0ab934ab-8612-496b-a280-efd21789120e)
![Screenshot 2024-11-22 at 12 11 48 PM](https://github.com/user-attachments/assets/dd2b99e9-b068-429b-901d-fa572e25d8ed)
![Screenshot 2024-11-22 at 12 11 34 PM](https://github.com/user-attachments/assets/009c4a4e-83e5-4b5d-9dd2-88e8764220e7)
![Screenshot 2024-11-22 at 12 12 08 PM](https://github.com/user-attachments/assets/eb18f109-02ff-4eb6-b85f-13d54ed4c44f)

Now we will run "terraform init," then plan, and then apply to spin up in our AWS and GCP consols!
![Screenshot 2024-11-22 at 12 14 34 PM](https://github.com/user-attachments/assets/cbf44d6b-82ff-4d1e-9332-1418d37af2f0)


"Terraform plan" looks good and no issues

![Screenshot 2024-11-22 at 12 15 10 PM](https://github.com/user-attachments/assets/9c909c3f-ccd5-4b4b-b917-a4b01db322bf)

Next we will run the following command "terraform apply -auto-approve -var-file=terraform.auto.tfvars" This will do a few things. "Apply" will tell terraform to build anything that is in our files that has not been created yet. "-auto-approve" will automatically confirm that we want to do these 'changes.' "-var-file=<name of your var file> lets terraform know we have variables that we are using in this set up.

![Screenshot 2024-11-22 at 12 24 20 PM](https://github.com/user-attachments/assets/90ceb5b8-c454-4236-b104-4bf08f0a8e42)

Up and running!
![Screenshot 2024-11-22 at 12 25 00 PM](https://github.com/user-attachments/assets/442c69ae-a398-48ac-a398-d9e91b7081a7)
![Screenshot 2024-11-22 at 12 25 18 PM](https://github.com/user-attachments/assets/10d4a255-cb2d-4319-9167-dacce3d67a80)


Now that are VM's are running in GCP and AWS we will now use ansible to install and update the needed tools. First lets add NMAP to our AWS EC2
![Screenshot 2024-11-22 at 12 30 18 PM](https://github.com/user-attachments/assets/0eb9f978-f45f-4aa2-b0d3-6ac006e28050)
Looks like we were able to connect and install NMAP lets check in the AWS EC2 instance!
![Screenshot 2024-11-22 at 12 32 36 PM](https://github.com/user-attachments/assets/d4a6b78c-4d67-4135-a7e9-0bcfd5218f06)
![Screenshot 2024-11-22 at 12 34 16 PM](https://github.com/user-attachments/assets/46cee935-0473-485f-9116-eb295ee3b2a4)

Now lets run our GCP ansible playbook!
















