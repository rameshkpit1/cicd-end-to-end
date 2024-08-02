pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_CREDENTIALS_ID = 'docker-cred'
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

        stage('Login to Docker') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push rameshm1/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "cicd-end-to-end"
            GIT_USER_NAME = "rameshkpit1"
        }
        steps {
            withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    git config user.email "ram.btp68@gmail.com"
                    git config user.name "Ramesh Babu M"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    #REPOSITORY_NAME="rameshm1/ultimate-cicd"
                    sed -i "s|image: .*|image: rameshm1/django:${BUILD_NUMBER}|" deploy/deploy.yaml
                    git add deploy/deploy.yaml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
    }
  }
        
    }
