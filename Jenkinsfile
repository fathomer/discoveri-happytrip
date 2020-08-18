pipeline{
    agent any
    parameters{
        booleanParam(name: 'quality', description: 'Do you want code quality analysis? true/false', defaultValue : false)
        booleanParam(name: 'deploy', description: 'Do you want code deployment? true/false', defaultValue : false)
    }
    stages{
        stage('Build'){
            steps {
                echo "Build"
                if (${params.deploy} == true) {
                    withSonarQubeEnv('sonarqube-vm'){
                        powershell label: '', script: 'mvn package sonar:sonar'
                    }
                } else{
                     powershell label: '', script: 'mvn package'
                }
            }
        }
    }
    post{
        always{
            echo "Archive"
            archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            script{if (${params.deploy} == true) {
                echo "Deploying on Tomcat"
                deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8087/')], contextPath: 'happytrip', war: '**/*.war'    
            }}
            
        }
    }
}
