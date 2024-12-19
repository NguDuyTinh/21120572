pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = "1"
    }
    tools {
        nodejs 'nodejs'
    }
    parameters {
        choice(name:'VERSION', choices:['1.0', '1.1', '1.2'], description:'Choose the version of the project')

        booleanParam(name :'executeTests', description:'Execute the tests', defaultValue:false)
    }
    
    stages {
        stage('Setup Buildx') {
            steps {
                sh '''
                mkdir -p ~/.docker/cli-plugins
                curl -LO https://github.com/docker/buildx/releases/latest/download/buildx-v0.11.2.linux-amd64
                mv buildx-v0.11.2.linux-amd64 ~/.docker/cli-plugins/docker-buildx
                chmod +x ~/.docker/cli-plugins/docker-buildx
                '''
            }
        }
        stage('Build') {
            steps {
                //sh 'npm install'
                // sh 'npm run build'
                sh 'npm --version'
                echo 'Build'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo 'Test'

            }
        }
        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'Admin@123', usernameVariable: '21120572')]) {
                    sh '''
                    DOCKER_BUILDKIT=1 docker build -t DevOps/sample-react-app .
                    '''
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push DevOps/sample-react-app'
                }
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run  -p 3000:3000 -d jennykibiri/sample-react-app:latest'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.92.144.96 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
