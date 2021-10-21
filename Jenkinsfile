pipeline {
    agent any
    stages {
        stage('Git checkout') {
        steps {
            git branch: 'main', url: 'https://github.com/Lokesh3011/jenkins-kubernetes-example.git'
          }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build . -t nodeapp'
                sh 'docker images'
            }
        }

        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u username -p ${dockerhubpwd}'
                 }  
                 sh 'docker push username/nodejsapp-1.0:latest'
                }
            }
        }

         stage('Deploy to K8s') {
            steps {
                kubernetesDeploy configs: '*.yaml', kubeConfig: [path: ''], kubeconfigId: 'K8s', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
}
}
        }
    }
