node {
    docker.image('node:16-buster-slim').inside('-p 3007:3007') {
        stage('Build') { 
            sh 'npm install' 
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
        }
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh' 
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
            sh './jenkins/scripts/kill.sh' 
            sleep(time: 1, unit: "MINUTES")
        }
    }
}