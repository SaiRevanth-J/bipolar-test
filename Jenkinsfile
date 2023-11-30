pipeline {
    agent any

    tools {
        maven "maven"
    }

    stages {
        stage('Git checkout') {
            steps {
              
                   git 'https://github.com/SaiRevanth-J/bipolar-test.git'
            
                }
            }
        stage('maven build') {
              steps {
              
                     sh "mvn install package"
                }
        }
        
         
          stage('Docker build image') {
              steps {
                  
                  sh'sudo docker system prune -af '
                  sh 'sudo docker build -t revanthkumar9/bipolar:${BUILD_NUMBER}.0 .'
              
                }
            }
                
        stage('Docker login and push') {
              steps {
                   withCredentials([string(credentialsId: 'docpass', variable: 'docpasswd')]) {
                  sh 'sudo docker login -u revanthkumar9 -p ${docpasswd} '
                  sh 'sudo docker push revanthkumar9/bipolar:${BUILD_NUMBER}.0 '
                  }
                }
        }    


        stage('Deploy') {
              steps {
                   withCredentials([string(credentialsId: 'docpass', variable: 'docpasswd')]) {
                  sh 'sudo docker login -u revanthkumar9 -p ${docpasswd} '
                  sh 'sudo docker push revanthkumar9/bipolar:${BUILD_NUMBER}.0 '
                  }
                }
        }   
                
        stage('waitng to start the app') {
              steps {
                  
                  sh ' sleep 40'
                           
                }
            }
       
        /*stage('Selenium test') {
              steps {
                  
                  sh 'sudo java -jar seleniumbank.jar'
                  sh"echo 'application is logged in succussfully done' "
                           
                }
            }*/

        
    }
}
