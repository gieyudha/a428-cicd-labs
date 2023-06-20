node {
    env.CI = 'true'
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
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
            //sh "rm -rf /var/www/jenkins-react-app"
           // sh "cp -r ${WORKSPACE}/build/ /var/www/jenkins-react-app/"
            sh './jenkins/scripts/deliver.sh' 
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
            sleep(time: 1, unit: "MINUTES")
            sh './jenkins/scripts/kill.sh' 
        }
    }
}
