pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker build -t gieyudha/react-app-dicoding .'
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push gieyudha/react-app-dicoding'
                }
            }
        }
        stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'docker run --name react-app -p 3000:3000 -d gieyudha/react-app-dicoding:latest'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.1.81.35 ${dockerCmd}"
                    }
                }
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
    }
}
