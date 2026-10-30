pipeline {
    agent any
    stages {
        stage ("pull") 
        {
            steps {
                git 'https://github.com/Mohit-Verma555/sonar.git' 
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
                       -Dsonar.projectKey=deploy-1 \
                       -Dsonar.projectName='deploy' \
                       -Dsonar.host.url=http://3.24.216.90:9000 \
                       -Dsonar.token=sqp_738343d71535fc0f740a583d180121f92352a9ad ''' 
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
