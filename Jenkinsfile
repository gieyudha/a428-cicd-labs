node {
    env.CI = 'true'
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') { 
            sh 'npm install'
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        stage('Build Image') {
            withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh 'docker build -t gieyudha/react-app-dicoding .'
                sh "echo $PASS | docker login -u $USER --password-stdin"
                sh 'docker push gieyudha/react-app-dicoding'
            }
        }
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
        }
        stage('Deploy') {
           sh './jenkins/scripts/deliver.sh'
           script {
                    def dockerCmd = 'docker run  -p 3000:3000 -d gieyudha/react-app-dicoding:latest'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.1.81.35 ${dockerCmd}"
                    }
            }
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
            sleep(time: 1, unit: "MINUTES")
            sh './jenkins/scripts/kill.sh' 
        }
    }
}
