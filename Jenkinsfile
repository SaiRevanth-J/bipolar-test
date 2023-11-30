pipeline {
    agent any


    stages {
        stage('Git checkout') {
            steps {
              
                   git 'https://github.com/SaiRevanth-J/bipolar-test.git'
            
                }
            }
        stage('maven build') {
              steps {
              
                     sh "mvnnn install package"
                }
        }
        
         
          stage('Docker build image') {
              steps {
                  
                  sh' sudo docker system prune -af '
                  sh ' sudo docker build -t revanthkumar9/bipolar:${BUILD_NUMBER}.0 .'
              
                }
            }
                
        stage('Docker login and push') {
              steps {
                   withCredentials([string(credentialsId: 'docpass', variable: 'docpasswd')]) {
                  sh ' sudo docker login -u revanthkumar9 -p ${docpasswd} '
                  sh ' sudo docker push revanthkumar9/bipolar:${BUILD_NUMBER}.0 '
                  }
                }
        }    
        
        stage('App deploy on test-server ') {
              steps {
                  withCredentials([sshUserPrivateKey(credentialsId: 'test-server', keyFileVariable: 'sshkey', passphraseVariable: 'passkey', usernameVariable: 'ubuntu')]) {
                      
                      sh 'ssh -o StrictHostKeyChecking=no -i ${sshkey} ${ubuntu}@172.31.35.204 sudo docker system prune -af ' 
                       sh 'ssh -o StrictHostKeyChecking=no -i ${sshkey} ${ubuntu}@172.31.35.204 sudo docker run -dt -p 8081:8080 revanthkumar9/bipolar:${BUILD_NUMBER}.0 '
}
                }
        }    

        stage('waitng to start the app') {
              steps {
                  
                  sh ' sleep 4'
                           
                }
            }
       
        stage('Selenium test') {
              steps {
                  
                  sh 'sudo java -jar demotest.jar'
                  sh"echo 'application testing  done' "
                           
                }
            }

        
    }
    post {
        
        failure {
            echo 'sending email notification from jenkins'
            
            step([$class: 'Mailer',
      notifyEveryUnstableBuild: true,
      recipients: emailextrecipients([[$class: 'CulpritsRecipientProvider'],
                                      [$class: 'RequesterRecipientProvider']])])

            
       }
    }  
}