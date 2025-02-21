pipeline {
    agent any

    environment {
        IMAGE_NAME = "vardhan1125/WB-Using-BS-Essentials"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/YashVMishra/WB-Using-BS-Essentials.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh "docker push $IMAGE_NAME"
                }
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    sh '''
                    git config --global user.email "yashvardhanmishra01@gmail.com"
                    git config --global user.name "YashVMishra"
                    git clone --branch gh-pages https://github.com/YashVMishra/WB-Using-BS-Essentials.git deploy_repo
                    cd deploy_repo
                    rm -rf *
                    cp -r ../* .
                    git add .
                    git commit -m "Automated update"
                    git push origin gh-pages
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
