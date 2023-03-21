
currentBuild.displayName = "Maven-Project-#"+currentBuild.number

pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven-3.9/bin:$PATH"
        }
  
    stages{
      
        stage("Git Checkout"){
            steps{
                git credentialsId: 'github', url: 'https://github.com/tirush1245/hello-world.git'
            }
        }
      
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
//                 sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/webapp.war  ec2-user@1.0.0.99:/opt/tomcat9/webapps/
                    
                    ssh ec2-user@1.0.0.99 /opt/tomcat9/bin/shutdown.sh
                    
                    ssh ec2-user@1.0.0.99 /opt/tomcat9/bin/startup.sh
                
                """
               }
            
            }
        }
  
    }
}
