pipeline{
    agent any
    
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    
    stages{
        stage('Git-checkout'){
            steps{
                git branch: 'main' , url: 'https://github.com/jagadish82/ciprojects.git'
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Package'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build and tag'){
            steps{
                sh 'docker build -t jagadish69/ciproject .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name c8 -p 9008:8080 jagadish69/ciproject
                '''
            }
        }
        stage('docker login to dockerhub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
                sh '''echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'''
            }

            }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push jagadish69/ciproject'
            }
        }
        
    }
}
