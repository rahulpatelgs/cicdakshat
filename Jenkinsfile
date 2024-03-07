pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/rahulpatelgs/cicdakshat', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:latest'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
               //withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
              //sh "echo $PASS | docker login -u $USER --password-stdin"
              //sh 'docker push rahulpatel123/$JOB_NAME:v1.$BUILD_ID'
                     //sh 'docker push rahulpatel123/$JOB_NAME:latest'
                }
            }
        }
        
        stage('Deploy to k8s'){
            steps{
                script{
                    def kubepodrm = 'sudo kubectl delete pod spring-boot-k8s-deployment || true'
                    def kubeCmd = 'sudo kubectl apply deploymentservice.yaml'
                    sshagent(['sshkeykube8s']) {
                        //sh "docker rm -f My-first-containe-1"
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 $JOB_NAME:v1.BUILD_ID"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.108.44.45 ${kubepodrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.108.44.45 ${kubeCmd}"
                   //kubernetesDeploy configs: 'deploymentservice.yaml', kubeConfig: [path: ''], 
                    //kubeconfigId: 'k8sconfigpwd', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''],
                    //textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
                    //kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
