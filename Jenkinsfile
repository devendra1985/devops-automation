pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/devendra1985/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t taledevendra/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerpwd')]) {
                   sh 'docker login -u taledevendra -p ${dockerpwd}'

}
                   sh 'docker push taledevendra/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: '', credentialsId: 'k8cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.59.105:8443')
                {
                  sh 'kubectl apply -f deploymentservice.yaml'
                 }
            }
        }
    }
}
