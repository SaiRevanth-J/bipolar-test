###Note :-  Please  Go through Bipolar-assignment.pdf in repo for clear understanding.


1.Created Spring boot Application of Helloworld  :-

2.Created GITHUB REPO  :-

  a) created Github repo :- https://github.com/SaiRevanth-J/bipolar-test.git .

  b) Appplication files are uploaded in the repo.

  c) In repo setting added Webhook to automate the jenkins job whenever there is new push to repo.

3.Launced Jenkins-Server for CI/CD pipeline,Monitoring and Test-server for application deployment  :-

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
      6.sudo apt-get update -y
      7.sudo apt-get install jenkins 

  d) Added Jenkins user to the sudeors file for sudo permissions.
    
  e) Maven is used for build application.

  f) Docker is used to containerize  the application .

  g) To execute selenium test downloaded google-chrome and chrome-driver in jenkins-server from the below link.
    
      1. sudo wget https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/119.0.6045.105/linux64/chrome-linux64.zip
      2. sudo https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/119.0.6045.105/linux64/chromedriver-linux64.zip

  h) both are unzipped and saved on required path.

  i) Selenium automated test case are written as shown below  . 

        package testing.com;

         import java.io.IOException;

         import org.openqa.selenium.By;
         import org.openqa.selenium.WebDriver;
         import org.openqa.selenium.chrome.ChromeDriver;
         import org.openqa.selenium.chrome.ChromeOptions;


         public class testdemo {
	 public static void main( String[] args )throws InterruptedException, IOException {
	
		System.setProperty("webdriver.chrome.driver", "/var/lib/jenkins/workspace/new/chromedriver-linux64/chromedriver"); 
		

    	        ChromeOptions chromeOptions = new ChromeOptions();
    	        chromeOptions.addArguments("--remote-allow-origins=*");
		chromeOptions.addArguments("start-maximized");
		chromeOptions.addArguments("--headless");
		chromeOptions.addArguments("--no-sandbox");
		chromeOptions.addArguments("--disable-dev-shm-usage");
		chromeOptions.addArguments("--ignore-ssl-errors=yes");
		chromeOptions.addArguments("--ignore-certificate-errors");
	
    	        chromeOptions.setBinary("/var/lib/jenkins/workspace/new/chrome-linux64/chrome");
		
		WebDriver driver = new ChromeDriver (chromeOptions);
	
		Thread.sleep(3000);
		driver.get("http://54.173.163.134:8081");
		driver.manage().window().maximize();
		Thread.sleep(3000);
		
		String message = driver.findElement(By.xpath("//*[contains(text(),'Hello World!')]")).getText();
		if(message.equals("Hello World!")) {
			System.out.println(" Test Script Executed Successfully");
		} else 
		{
			System.out.println("Script Failed");
		}
		
		driver.quit();
		
	     }
        }



  j) Jenkins is Accessed at public ip of  jenkins-server http://54.145.138.148:8080.

  k) Smtp Server is Configured in mange jenkins > system to notify the job failure through email

  l) In Manage Jenkins > Credentials , credentials  are configured for dockerhub and test-server access.

  m) Created Jenkins pipeline Job with webhook trigger and declarative pipeline is taken from JenkinsFile
  
  n) Jenkinsfile is present in repo.

  o) Bipolar-test Pipeline Job-1 is failed intentionally to test the Email Notifications is working or not on job failure. 
  
  p) Email notification Received successfully on job failure.
  
  q) After Rectificatin of  Error in pipeline and after a new  push, webhook triggered the job and run the pipeline successfully.

  r) Application is deployed on test-server and accessed on 54.173.163.134:8081 

  s) Application is deployed on test-server (container name : sharp_mendeleev).
  
5. Configured Monitoring and logging  :-

 a) Deployed application container is monitored by using prometheus , grafana and cAdviser.

 b) First cAadviser was started  on test-server instance to monitor the running containers  with following command.

         sudo docker run -d --name=cadvisor -p 8080:8080 -v /:/rootfs:ro -v /var/run:/var/run:ro -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -v /dev/disk/:/dev/disk:ro --privileged --device=/dev/kmsg --restart=unless-stopped gcr.io/cadvisor/cadvisor

 c) cAdvisor capture the metrics of docker containers in Test-server.

 d) Cadvisor container is up and running on test-server at  54.173.163.134:8080 .

 e) Created prometheus.yml file in jenkins-server and configured to scrap the metrics of test-server docker containers from CAdvisor as below .

             scrape_configs:
             - job_nam: Docker_containers
               scrape_interval: 5s
               static_configs:
               - targets:
                 - 54.173.163.134.8080

 f) In Jenkins-server Prometheus and grafana are launced as a docker container to setup  monitor and logging  with following commands 

       1. docker volume create prometheus-data
       2. docker run -d \
                -p 9090:9090 \
                -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
                -v prometheus-data:/prometheus \
                 prom/prometheus

       3.docker run -d --name=grafana -p 3000:3000 grafana/grafana
       

 g) Prometheus is accessed on jenkins-server 54.145.138.148:9090 .

 h) Grafana  is accessed on jenkins-server 54.145.138.148:3000 .
  
 i) Dashboard is created for Monitoring and logging of  deployed container. 


