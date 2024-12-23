![image](https://github.com/user-attachments/assets/3b02d2e1-65e6-4c7b-926f-e2c37c2eb2cd)

In this project we will host the static website content in this repositary on Ec2 Instancce via Jenkinss and create the CI/CD pipeline so whenever there is any change in the code via the Developer, Jenkins will build and deploy that code automatically on the webserver. Follow the below steps to setup it.

We will use the same Ec2 Instance for hosting and running the Jenkins tool as this prject does not require multiple instances.

Prerequisites:
Jenkins is installed.
Install imp plugins like SSH Plugin and github

Step 1: Create a New Jenkins Freestyle Job
Go to Jenkins Dashboard.
Click on "New Item".
Enter a name for the project, e.g., Static-Website-Deployment.
Select Freestyle project and click OK.


Step 2: Configure the Job to Poll SCM
Source Code Management:

Under Source Code Management, choose Git.
Enter the URL of your GitHub repository.
Select your credentials if needed.
Branch Specifier: Usually, */main for the main branch or */master if you use the master branch.


Build Triggers:

Enable Poll SCM.
Set a schedule to poll for changes. For example:
Copy code
H/5 * * * *  
This means Jenkins will check for changes in the repository every 5 minutes.



Step 3: Configure Build Steps with Shell Commands
Scroll down to the Build section and click Add build step.

Select "Execute shell".

In the shell command field, enter the following commands to copy the website's files to your EC2 instance using SSH:

Example shell commands that you can use.

scp -i /path/to/your/private-key.pem -r ./ * ec2-user@<EC2-PUBLIC-IP>:/var/www/html/ (This is to copy the artifacts from jenkins to webserever folder)

ssh -i /path/to/your/private-key.pem ec2-user@<EC2-PUBLIC-IP> "sudo systemctl restart nginx" (This is to restart nginx so it shows all the changes)

Mark the job completed and run the job. For testing, make sure that port 80 is open then run http://public-ip-of-Ec2/80 on your computer to see the website running. Do any changes in the code and see if webiste reflect your changes.

