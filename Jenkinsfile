pipeline {
    agent {
        node{
            label 'labels1'
        }
    }

    stages {
        stage('github') {
            steps {
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/Dheerajthuniki/amazona-app.git']])
            }
        }
        stage('Docker build') {
           steps {
               sh 'sudo docker build -t amazona:latest .'
               sh 'sudo docker tag amazona:latest dheerajthuniki/amazona-app'
           }
           
       }
       stage('docker push'){
           steps{
              withCredentials([usernamePassword(credentialsId: 'Dockerhub', passwordVariable: 'dockerpwd', usernameVariable: 'dockeruser')]) {
 
                 sh 'sudo docker login -u ${dockeruser} -p ${dockerpwd}'
                 sh 'sudo docker push dheerajthuniki/amazona-app'
}
           }
       }
       stage('docker container run'){
           steps{
               sshagent(['webserver']) {
                   sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.111.246.113 sudo docker run -itd -p 3008:3000 dheerajthuniki/amazona-app:latest '
                  
               }
               
           }
       }
    }
    
}
    
