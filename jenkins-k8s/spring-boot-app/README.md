# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml at the root directory of the repository.


## Build the application locally and access it using your browser

Checkout the repo and move to the directory

```
git clone https://github.com/techielins/CICDPipeline.git

cd jenkins-k8s/sprint-boot-app
```

Execute the Maven targets to generate the artifacts using the command

```
mvn clean package
```

The above maven target stroes the artifacts to the `target` directory.


### Execute locally (Java 11 needed) and access the application on http://localhost:8080

```
java -jar target/spring-boot-web.jar
```


### Configure a Sonar Server locally

```
apt install unzip
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

You can access the `SonarQube Server` on `http://<ip-address>:9000` 


