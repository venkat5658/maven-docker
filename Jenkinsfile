pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/venkat5658/maven-docker.git']]])
                  sh 'mvn clean install'
            }
        }
         stage('stop container') {
            steps{
                sh 'docker rm $(sudo docker stop $(sudo docker ps -a | grep "venkat5658/kubernetes" | cut -d " " -f 1))'
             }
        }        
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t venkat5658/kubernetes .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u venkat5658 -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push venkat5658/kubernetes'
                }
            }
        }
        stage('run image') {
            steps{          
                sh 'docker run  -d -t --name kubernetes -p 8081:80 venkat5658/kubernetes:latest'  
                }
       }
    }    
}
