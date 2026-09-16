pipeline{
    agent any
    stages{
        stage('checkout-code'){
            steps{
                git 'https://github.com/kundurthiravikiran2/java-maven-project-new.git'
            }
        }
        stage('compile-project'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('test-project'){
            steps{
                sh 'mvn test'
            }
        }
        stage('sonarqube scanning'){
            steps{
                withSonarQubeEnv ('sonarqube'){
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    
                }
            }
        }
        stage('generate-artifactory'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('uploading-artifactory'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: '.war']], credentialsId: 'nexus', groupId: 'in.krishna', nexusUrl: '54.166.3.79:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'hotstar', version: '8.3.3-SNAPSHOT'
            }
        }
        stage('deployement-project'){
            steps{
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tom', path: '', url: 'http://18.215.179.33:8080/')], contextPath: 'myapp', war: '**/*.war'
            }
        }
    }
}
