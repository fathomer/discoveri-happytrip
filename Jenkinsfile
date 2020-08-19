pipeline{
    agent any
    parameters{
        booleanParam(name: 'quality', description: 'Do you want code quality analysis? true/false', defaultValue : false)
        booleanParam(name: 'deploy', description: 'Do you want code deployment? true/false', defaultValue : false)
    }
    stages{
        stage('Build with Sonar'){
            when {
                expression { params.quality}
            }
            steps {
                    echo "Build with Sonar"
                    withSonarQubeEnv('sonarqube-vm'){
                    powershell label: '', script: 'mvn package sonar:sonar'
                    }
            }
        }
        stage('Build'){
            when {
                expression {params.quality == false}
            }        
            steps {   
                    echo "Build without Sonar"
                    powershell label: '', script: 'mvn package'
                  }
        }
        stage('Deploy'){
            when {
                expression {params.deploy}
            }        
            steps {   
                echo "Deploying on Tomcat"
                deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8087/')], contextPath: 'happytrip', war: '**/*.war'
            }
        }
        }
    post{
        always{
            echo "Archive"
            archiveArtifacts artifacts: '**/*.war', followSymlinks: false
       
        }
        }
    }
