pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'javahome2', url: 'https://github.com/prasannachl/VCIT_AST2.git'
            }
        }
         stage('Build') {
            steps {
                dir("/var/lib/jenkins/workspace/tomcatdeploy") {
                sh 'mvn -f POM.xml clean package'
                sh "mv target/*.war target/myweb.war"
                }    
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.9.186:/home/ec2-user/apache-tomcat-9.0.74/webapps/
                    
                    ssh ec2-user@172.31.9.186 /home/ec2-user/apache-tomcat-9.0.74/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.9.186 /home/ec2-user/apache-tomcat-9.0.74/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
