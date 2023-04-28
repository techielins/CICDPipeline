# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD and Kubernetes


This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to target environment deployment, using popular tools like SonarQube, Argo CD and Kubernetes.

*Setup EC2 instance in AWS*

* Log in to the AWS account, click on the Services menu at the top right corner of the page, a dropdown menu will follow, Select Compute and a submenu will pop up, select EC2 > Proceed to Launch instances. > Once we’re on the instances page, The first step is choosing the Amazon Machine Image (AMI) > and proceed with the configuration and provision the instance.

*Installing Jenkins on an EC2 Instance*

* SSH into the already created EC2 Instance

Then install Java using:

$ which amazon-linux-extras

$ amazon-linux-extras

$ sudo amazon-linux-extras enable java-openjdk11

$ sudo yum clean metadata && sudo yum install java-openjdk11

* Add the Jenkins the repository using the command below:

$ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

* Add the key file from Jenkins CI to enable the Jenkins installation from the package:

$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

$ sudo amazon-linux-extras install epel

* Install Jenkins:

$ sudo yum install jenkins

* To enable the Jenkins service to start at boot, Start Jenkins as a service and check status:

$ sudo systemctl enable jenkins

$ sudo systemctl start jenkins

$ sudo systemctl status jenkins

* Configuring Jenkins:

http://<your_server_public_DNS_address>:8080

This will open the Jenkins configuration page which will get you started.

Type the following command to display the initial password on your Terminal:

$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Paste the password copied from your terminal into the field provided on the Jenkins configuration page and click continue.

* Install plugins:

-- Docker Pipeline plugin

*Docker Slave Configuration*

Run the below command to Install Docker

$ sudo yum update

$ sudo yum install docker.io

Grant Jenkins user and ec2-user user permission to docker deamon.

$ sudo usermod -aG docker jenkins

$ sudo usermod -aG docker ec2-user 

$ sudo systemctl restart docker

Once you are done with the above steps, please restart Jenkins.

$ sudo systemctl restart jenkins

The docker agent configuration is now completed.


