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

               withSonarQubeEnv(installationName: 'sonar-install', credentialsId: 'sonar id') {

                        sh ''' /opt/maven/bin/mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                       -Dsonar.projectKey=deploy-1
                       -Dsonar.projectName='deploy' '''
                         } 
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
