node {
    stage('Build') {
       nodejs(nodeJSInstallationName: 'node') {
          sh 'npm install'
       }
    }
    stage('Test') {
       nodejs(nodeJSInstallationName: 'node') {
          sh './jenkins/scripts/test.sh'
       }
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
       script {
          def dockerCmd = 'docker run --name react-app -p 3000:3000 -d gieyudha/react-app-dicoding:latest'
          sshagent(['ec2-server-key']) {
                   sh "ssh -o StrictHostKeyChecking=no ubuntu@3.1.81.35 ${dockerCmd}"
           }
       }
       sleep(time: 1, unit: "MINUTES")
   }
}
