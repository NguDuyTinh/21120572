pipeline {
    agent any
    tools {
        nodejs 'nodejs'
        jdk 'OpenJDK8'
        maven 'Maven3'
    }
    stages {
        stage('Maven Build') {
            steps {
                //sh 'mvn clean install'
                echo 'Maven Build'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub-repo', passwordVariable: 'Admin@123', usernameVariable: '21120572') {
                        sh 'docker build -t wude18/docker-hub-repo:ta123 .'
                        sh 'docker push wude18/docker-hub-repo:ta123'
                    }
                }
            }
        }
    }
}
