pipeline{
    agent any
    tools{
        maven 'maven_3_8_6'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/GaborPinter/devops-automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t gaborpinter/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd-ok', variable: 'dockerhubpwdok')]) {
                    bat 'docker login -u gaborpinter -p ${dockerhubpwdok}'
                    
}
                    bat 'docker push gaborpinter/devops-integration'
                }
            }
        }
    }
}