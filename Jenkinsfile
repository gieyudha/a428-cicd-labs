pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3007:3000'
        }
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
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}

// node {
//     docker.image('node:16-buster-slim').inside('-p 3007:3007') {
//         stage('Build') { 
//             sh 'npm install' 
//         }
//         stage('Test') {
//             sh './jenkins/scripts/test.sh'
//         }
//     }
// }