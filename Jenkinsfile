pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
              git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/rameshkpit1/cicd-end-to-end.git'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t rameshm1/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    echo ${{ secrets.DOCKERHUB_TOKEN }} | docker login -u ${{ secrets.DOCKERHUB_USERNAME }} --passwd-stdin docker.io
                    docker push rameshm1/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
    }
}
