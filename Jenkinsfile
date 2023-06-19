pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
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
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                sh "docker exec -it quirky_ardinghelli bash"
                sh "rm -rf /var/www/jenkins-react-app"
                sh "exit"
                sh "docker cp -r ${WORKSPACE}/build/ quirky_ardinghelli:/var/www/jenkins-react-app/"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
