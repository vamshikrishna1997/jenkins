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
              sshagent(['78e22912-29d3-4eb4-a914-daafcc25ee59']){
                  
                
                 sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/tom/tomcat.sh $USER:/home/centos'
                   sh 'ssh $USER sudo chmod 777 /home/centos/tomcat.sh' 
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
              sshagent(['78e22912-29d3-4eb4-a914-daafcc25ee59'])  {
              
             sh """
                     scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/tom_deploy/webapp/target/webapp.war $USER:/home/centos
                        ssh -o StrictHostKeyChecking=no $USER 'sudo cp -r /home/centos/webapp.war $APP_DIR'
         """     
               sh 'curl $WEB_APP'
            
          }
      }
     }
           
     }
  }
