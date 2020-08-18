pipeline{
    agent any
    stages{
        stage('Build'){
            steps {
                echo "Build"
                 withSonarQubeEnv('sonarqube-vm'){
                    powershell label: '', script: 'mvn package sonar:sonar'
                }
            }
        }
    }
    post{
        always{
            echo "Archive"
            archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            echo "Deploying on Tomcat"
            deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8087/')], contextPath: 'happytrip', war: '**/*.war'
        }
    }
}
