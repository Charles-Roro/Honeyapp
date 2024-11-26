# Honeyapp
Using AWS and GCP to create and monitor a Fake app.

First step is to create all the folders and files we will need for the project via the command line.

![Screenshot 2024-11-22 at 11 29 35 AM](https://github.com/user-attachments/assets/8fc7b738-fa2a-4017-a570-32b890370a7f)

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
![Screenshot 2024-11-22 at 2 15 40 PM](https://github.com/user-attachments/assets/ead1e297-ab59-442d-a211-ccdde8d0741f)

Now lets check or IP address to see if the apache loads. Good the apache page is showing1
![Screenshot 2024-11-22 at 2 19 21 PM](https://github.com/user-attachments/assets/f0786f1f-e997-4cd7-b878-827d30033680)

Now we will deploy our website.yml
![Screenshot 2024-11-22 at 2 21 29 PM](https://github.com/user-attachments/assets/b88c7c37-b932-40da-84e6-c23eb6310f43)

Now lets check the url again.
![Screenshot 2024-11-22 at 2 32 47 PM](https://github.com/user-attachments/assets/13c47fb7-167b-4f29-bfe1-a585d1e9d0e8)


Now our website is up and running!

Next we will set up Wazuh via the Deployment Market place.
![Screenshot 2024-11-26 at 11 28 50 AM](https://github.com/user-attachments/assets/71e1aa70-8d2b-408c-99b3-db31453ffb33)
There are guided instructions be sure to follow them I'll add the link [Here](https://decyphertek.readthedocs.io/en/latest/products/gcp-wazuh-instructions/)
![Screenshot 2024-11-26 at 11 31 05 AM](https://github.com/user-attachments/assets/128def68-cec0-455b-aa8e-62fbd033fbad)
![Screenshot 2024-11-26 at 11 31 18 AM](https://github.com/user-attachments/assets/8795de9a-54b7-4e3c-b92d-96a6e9e10aba)
Now deployed in GCP
![Screenshot 2024-11-26 at 11 37 25 AM](https://github.com/user-attachments/assets/3fee06d2-8ee5-4830-9fa6-66b197deba8b)

Next we need to wait about 5-15 minutes for the services to start. 
Then we follow the instructions to set up our agent to put it in our GCP-Linux-Box. Be sure to add the ip address of your Wazuh server.
![Screenshot 2024-11-26 at 11 49 39 AM](https://github.com/user-attachments/assets/bfde8b38-b806-413f-8829-ce6731817643)
Next we ssh into our GCP-Linux-Box and run the commands given in Wazuh to install the agent.
![Screenshot 2024-11-26 at 12 01 12 PM](https://github.com/user-attachments/assets/c437c2c5-0831-4d6e-8617-5cc68af7a3ab)
Now let's check Wazuh if the agent is active. It is now up and running! Also connected via our internal GCP subnet!
![Screenshot 2024-11-26 at 12 02 58 PM](https://github.com/user-attachments/assets/f5b5f319-4a23-4b55-ae60-c437c9a80955)

Our last steps will involve going back to AWS and we will SSH into our instance and run a few NMap scans on our GCP-Linux-Box
![Screenshot 2024-11-26 at 12 22 02 PM](https://github.com/user-attachments/assets/2ecf8b35-acb8-4701-ae11-9b28bfae3e28)
![Screenshot 2024-11-26 at 12 37 56 PM](https://github.com/user-attachments/assets/280274e0-7243-43cd-b6a9-33883de03293)

Now we are all finished! You could always run more advanced scans and simulated attacks!

















