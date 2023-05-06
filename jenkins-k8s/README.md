# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD and Kubernetes


This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to target environment deployment, using popular tools like SonarQube, Argo CD and Kubernetes.

*Setup EC2 instance in AWS*

* Log in to the AWS account, click on the Services menu at the top right corner of the page, a dropdown menu will follow, Select Compute and a submenu will pop up, select EC2 > Proceed to Launch instances. > Once we’re on the instances page, The first step is choosing the Amazon Machine Image (AMI) > Amazon Linux 2 AMI > and proceed with the configuration and provision the instance.

*Installing Jenkins on an EC2 Instance*

* SSH into the already created EC2 Instance

Then install Java using:
```
$ which amazon-linux-extras
```
```
$ amazon-linux-extras
```
```
$ sudo amazon-linux-extras enable java-openjdk11
```
```
$ sudo amazon-linux-extras install java-openjdk11
```

* Add the Jenkins the repository using the command below:
```
$ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

* Add the key file from Jenkins CI to enable the Jenkins installation from the package:
```
$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key 
```
(This command might change over the time. Please check Jenkins release page to get the updated command)

```
$ sudo amazon-linux-extras install epel
```
* Install Jenkins:

```
$ sudo yum install jenkins
```

* To enable the Jenkins service to start at boot, Start Jenkins as a service and check status:
```
$ sudo systemctl enable jenkins
```
```
$ sudo systemctl start jenkins
```
```
$ sudo systemctl status jenkins
```

* Configuring Jenkins:

http://<your_server_public_DNS_address>:8080

This will open the Jenkins configuration page which will get you started.

Type the following command to display the initial password on your Terminal:
```
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste the password copied from your terminal into the field provided on the Jenkins configuration page and click continue.

* Install plugins:

-- Docker Pipeline plugin
-- Maven Integration plugin
-- Pipeline plugin


*Docker Slave Configuration*

Run the below command to Install Docker
```
$ sudo yum update
```
```
$ sudo yum install docker
```

Grant Jenkins user and ec2-user user permission to docker deamon.
```
$ sudo usermod -a -G docker jenkins
```
```
$ sudo usermod -a -G docker ec2-user
```
```
$ sudo systemctl restart docker
```

Once you are done with the above steps, please restart Jenkins.
```
$ sudo systemctl restart jenkins
```

Enable docker during system startup
```
$ sudo systemctl enable docker
```

The docker agent configuration is now completed.

*Install ArgoCD - Install on Kubernetes*

* Go to https://operatorhub.io/

* Find ArgoCD and Select

* Click on Install

* Login into your kubernetes cluster and run the following commands

* Install Operator Lifecycle Manager (OLM), a tool to help manage the Operators running on your cluster.
  ```
  $ curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.24.0/install.sh | bash -s v0.24.0
  ```
  
* Install the operator by running the following command:
  ```
  $ kubectl create -f https://operatorhub.io/install/argocd-operator.yaml
  ```
  
  Note : This Operator will be installed in the "operators" namespace and will be usable from all namespaces in the cluster.
  
* After install, watch your operator come up using next command.
  ```
  $ kubectl get csv -n operators
  ```
  
Once installation of operator is done, lets deploy a basic ArgoCD cluster in default namespace. For this, do the following.

* Create a yaml file - argocd-basic.yaml using the following content

```
apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: techielins-argocd
  labels:
    app: basic
spec: {}

```

* Create resources using following command

```
$ kubectl apply -f argocd-basic.yaml
```
* Confirm resources were created in the default namespace using the following command
```
$ kubectl get all
```

* To get access the ArgoCD UI, edit the techielins-argocd-repo-server service and change _ClusterIP_ to _NodePort_ using kubectl edit svc command
```
$ kubectl edit svc techielins-argocd-server
```

* Confirm, it is changed to NodePort using the following command
```
$ kubectl get svc techielins-argocd-server
```
Output will be like as follows

```
$ kubectl get svc techielins-argocd-server
NAME                       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
techielins-argocd-server   NodePort   10.100.252.54   <none>        80:31177/TCP,443:30313/TCP   16m
```
* In order to access the ArgoCD UI, use the port forward. Following command is required only if your cluster running on AWS EC2 instance using minikube
```
$ kubectl port-forward --address 0.0.0.0 svc/techielins-argocd-server 31177:80 > /dev/null &
```
* Getting username and password of ArgoCD UI

Username is admin

To retrive the password for admin, use the folliwng command

```
$ kubectl get secret argocd-secret -o jsonpath='{.data.admin\.password}' | base64 -d
```
Note : It is observed that argocd-cluster secret overrides the argocd-secret, so you may use the following command as well in case if you're not able to login using the password shown in the first command.

```
$ kubectl get secret techielins-argocd-cluster -o jsonpath='{.data.admin\.password}' | base64 -d
```


