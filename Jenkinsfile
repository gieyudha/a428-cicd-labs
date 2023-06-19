node {
    // add nothing
    env.CI = 'true'
    docker.image('node:16-buster-slim').inside('-p 3008:3008') {
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
            sh "sudo rm -rf /var/www/jenkins-react-app"
            sh "sudo cp -r ${WORKSPACE}/build/ /var/www/jenkins-react-app/"
            sh './jenkins/scripts/deliver.sh' 
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
            sh './jenkins/scripts/kill.sh' 
            sleep(time: 1, unit: "MINUTES")
        }
    }
}
