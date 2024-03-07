pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                git url: 'https://github.com/rahulpatelgs/cicdakshat', branch: 'master'
                sh 'mvn clean install'
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    sh "docker build -t $JOB_NAME:v1.$BUILD_ID ."
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:latest"
                    sh "docker images"
                }
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push rahulpatel123/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker push rahulpatel123/$JOB_NAME:latest"
                }
            }
        }
        stage('Deploy to k8s') {
            steps {
                script {
                    def kubeCmd = 'kubectl apply -f deploymentservice.yaml'
                    withCredentials(['sshkeykube8s']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.108.44.45 ${kubeCmd}"
                    }
                }
            }
        }
    }
}
