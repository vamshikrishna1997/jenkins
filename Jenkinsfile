pipeline {
    agent any
    stages{
        stage('checking_out') {
            steps {
                git '$GIT_URL'
                 sh 'pwd'
            }
        }
            
        stage ('tom_install') {
            steps { 
              sshagent(['ssh_ath']){
                  
                
                 sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/tom/tomcat.sh $USER:/home/ubuntu'
                   sh 'ssh $USER sudo chmod 777 /home/ubuntu/tomcat.sh' 
                    sh 'ssh $USER sudo sh tomcat.sh'
              }
            }
        }
         
        stage('mvn_build') {
         steps {
            sh "mvn clean package"
         }
     }
     stage('app_deploy') {
          steps { 
              sshagent(['ssh_ath'])  {
              
             sh """
                     scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/tom_deploy/webapp/target/webapp.war $USER:/home/ubuntu
                        ssh -o StrictHostKeyChecking=no $USER 'sudo cp -r /home/ubuntu/webapp.war $APP_DIR'
         """     
               sh 'curl $WEB_APP'
            
          }
      }
     }
           
     }
  }
