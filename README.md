###Note :-  Please  Go through Bipolar-assignment.pdf in repo for clear understanding.


1.Created Spring boot Application of Helloworld  :-

2.Created GITHUB REPO  :-

  a) created Github repo :- https://github.com/SaiRevanth-J/bipolar-test.git
  b) Appplication files are uploaded in the repo.
  c) In repo setting added Webhook to automate the jenkins job whenever there is new push to repo.

3.Launced Jenkins-Server for CI/CD pipeline,Monitoring and Test-server for appliction deployment  :-

  a) In AWS jenkins-server and test-server are  launched .

  b)  Test-server is configured with following commands.

      1.Sudo apt update -y
      2.Sudo apt install docker.io -y

  c) jenkins-server is configured with  following commands  to start the setup .

      1.Sudo apt update -y
      2.Sudo apt install git maven docker.io -y
      3.Sudo apt install openjdk-17-jdk -y
      4.sudo wget -O /usr/share/keyrings/jenkins-keyring.asc  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
      5.echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee  /etc/apt/sources.list.d/jenkins.list > /dev/null
      6.sudo apt-get update
      7.sudo apt-get install jenkins .

  d) Added Jenkins user to the sudeors file for sudo permissions.
    
  e) Maven is used for build application.
  f) Docker is used to containerize  the application .
  g) Selenium automated test case are written as shown below. 

        String message = driver.findElement(By.xpath("//*[contains(text(),'Hello World!')]")).getText();
		if(message.equals("Hello World!")) {
			System.out.println(" Test Script Executed Successfully");
		} else 
		{
			System.out.println("Script Failed");
		}



  h) Jenkins is Accessed at public ip of  jenkins-server http://54.145.138.148:8080.

  i) Smtp Server is Configured in mange jenkins > system to notify the job failure through email

  j) In Manage Jenkins > Credentials , credentials  are configured for dockerhub and test-server access.

  k) Created Jenkins pipeline Job with webhook trigger and declarative pipeline is taken from JenkinsFile
  
  l) Jenkinsfile is present in repo.

  m) Bipolar-test Pipeline Job-1 is failed intentionally to test the Email Notifications is working or not on job failure. 
  
  n) Email notification Received successfully on job failure.
  
  o) After Rectificatin of  Error in pipeline and after a new  push, webhook triggered the job and run the pipeline successfully.

  q) Application is deployed on test-server and accessed on 54.173.163.134:8081 

  r) Application is deployed on test-server (container name : sharp_mendeleev).


5. Configured Monitoring and logging  :-

 a) Deployed application container is monitored by using prometheus , grafana and cAdviser.

 b) First cAadviser was started  on test-server instance to monitor the running containers  with following command.

         sudo docker run -d --name=cadvisor -p 8080:8080 -v /:/rootfs:ro -v /var/run:/var/run:ro -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -v /dev/disk/:/dev/disk:ro --privileged --device=/dev/kmsg --restart=unless-stopped gcr.io/cadvisor/cadvisor

 c) cAdvisor capture the metrics of docker containers in Test-server.

 d) Cadvisor container is up and running on test-server    54.173.163.134:8080 .

 e) Created promethus.yml file in jenkins-server and configured to scrap the metrics of test-server docker containers as below from CAdvisor.

 f) In Jenkins-server Prometheus and grafana are launced as a docker container to stup  monitor and logging  with following commands 

       1. docker volume create prometheus-data
       2. docker run -d \
                -p 9090:9090 \
                -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
                -v prometheus-data:/prometheus \
                 prom/prometheus

       3.docker run -d --name=grafana -p 3000:3000 grafana/grafana
       

 g) Prometheus is accessed on jenkins-server 54.145.138.148:9090 .

 h) Grafana  is accessed on jenkins-server 54.145.138.148:3000 .
  
 i) Dashboard is created for Monitoring and logging of  deployed container 


