pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '076892551558.dkr.ecr.us-east-2.amazonaws.com/devops_repository'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/suzie-utrains/helloworld_pipeline.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-2:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
