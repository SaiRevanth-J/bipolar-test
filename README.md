BIPOLAR FACTORY

DevOps - Assignment


Submitted by - J. Sai Revanth kumar











Created Spring boot Application of Helloworld  :-



Created GITHUB REPO  :-

Created Github repo :- https://github.com/SaiRevanth-J/bipolar-test.git
Appplication files are uploaded in the repo.
In repo setting added Webhook to automate the jenkins job whenever there is new push to repo.















Launced Jenkins-Server for CI/CD pipeline,Monitoring and Test-server for appliction deployment  :-

In AWS jenkins-server   and test-server are  launched .

In Test-server is configured with following commands.

1.Sudo apt update -y
2.Sudo apt install docker.io -y

In jenkins-server is configured with  following commads  to start the setup .

1.Sudo apt update -y
2.Sudo apt install git maven docker.io -y
3.Sudo apt install openjdk-17-jdk -y
4.sudo wget -O /usr/share/keyrings/jenkins-keyring.asc  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
5.echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee  /etc/apt/sources.list.d/jenkins.list > /dev/null
6.sudo apt-get update
7.sudo apt-get install jenkins .

Added Jenkins user to the sudeors file as shown below.
 



Maven is used for build application.
Docker is used to containerize  the application .
Selenium automated test case are written as shown below.





Jenkins is Accessed public ip of  jenkins-server http://54.145.138.148:8080 as shown below.




Smtp Server is Configured in mange jenkins > system to notify the job failure through email



In Manage Jenkins > Credentials , credentials  are configured for dockerhub and test-server access as shown below.

 





Created Jenkins pipeline Job with webhook trigger and declarative pipeline is taken from JenkinsFile.





Jenkinsfile pipeline is as shown below.














Bipolar-test Pipeline Job-1 is failed intentionally to test the Email Notifications is working or not on job failure. 


.



Email notification Received successfully on job failure.as shown below




After Rectificatin of  Error in pipeline and after a new  push, webhook triggered the job and run the pipeline successfully as below.












 Pipeline is executed successfully.



Application is deployed on test-server and accessed on 54.173.163.134:8081 





Application is deployed on test-server as below (container name : sharp_mendeleev).









Configured Monitoring and logging  :-

Deployed application container is monitored by using prometheus , grafana and cAdviser.

First cAadviser was started  on test-server instance to monitor the running containers  with following command.

sudo docker run -d --name=cadvisor -p 8080:8080 -v /:/rootfs:ro -v /var/run:/var/run:ro -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -v /dev/disk/:/dev/disk:ro --privileged --device=/dev/kmsg --restart=unless-stopped gcr.io/cadvisor/cadvisor

cAdvisor capture the metrics of docker containers.

Cadvisor container is up and running on test-server   public ip with  port 8080 as shown below.




Created promethus.yml file in jenkins-server and configured to scrap the metrics of test-server docker containers as below from CAdvisor.


     








In Jenkins-server Prometheus and grafana are launced as a docker container to stup  monitor and logging  with following commands 

1. docker volume create prometheus-data
2. docker run -d \
    -p 9090:9090 \
    -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v prometheus-data:/prometheus \
    prom/prometheus

3.docker run -d --name=grafana -p 3000:3000 grafana/grafana
       

Prometheus is accessed on jenkins-server public ip with  port 9090 as shown below.




Grafana  is accessed on jenkins-server public ip with port 3000 as shown below.





Monitoring and logging Dashboard for deployed container is setup as below.


