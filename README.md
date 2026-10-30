pipeline {
    agent any
    stages {
        stage ("pull") 
        {
            steps {
                git 'https://github.com/Mohit-Verma555/jenkins-code.git' 
            }
        }
        stage ("Build") 
        {
            steps {
                sh '/opt/maven/bin/mvn clean package' 
            }
        }
        stage ("test") 
        {
            steps {
                sh ''' /opt/maven/bin/mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
  -Dsonar.projectKey=deploy \
  -Dsonar.projectName='deploy' \
  -Dsonar.host.url=http://3.24.216.90:9000 \
  -Dsonar.token=sqp_ac3115a79322c646d50f39ff99184b091b1bad05 ''' 
            }
        }
        stage ("deploy") 
        {
            steps {
                echo 'deployment successfull' 
            }
        }
        
    }
}
