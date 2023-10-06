pipeline {
    agent any
    tools{
        //maven 'maven_3_5_0'
        maven 'MyMaven'
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
                    sh 'docker build -t reddyk123/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                       withCredentials([string(credentialsId: 'DockerHub', variable: 'dockerhubpwd')]) {
                           sh 'docker login -u reddyk123 -p ${dockerhubpwd}'
                           sh 'docker push reddyk123/devops-integration'
                    }
                }
            }
        }
       // stage('Deploy to k8s'){
           // steps{
              //  script{
               //     kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
              //  }
           // }
        //}
    }
}
