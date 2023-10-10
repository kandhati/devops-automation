pipeline {
    agent any
    tools{
        maven 'MyMaven'
    }
    environment{
        // Generate a unique tag no
        IMAGE_TAG = "devops-integration:${BUILD_NUMBER}.0"
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kandhati/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    //sh 'docker build -t reddyk123/devops-integration:1.0 .'
                    sh 'docker build -t reddyk123/${IMAGE_TAG} .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                       withCredentials([string(credentialsId: 'DockerHub', variable: 'dockerhubpwd')]) {
                           sh 'docker login -u reddyk123 -p ${dockerhubpwd}'
                           sh 'docker push reddyk123/${IMAGE_TAG}'
                    }
                }
            }
        }  
    }
}
