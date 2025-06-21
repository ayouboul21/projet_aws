pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-user/ci-cd-static-site.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-static-site .'
            }
        }

        stage('Test Image Locally') {
            steps {
                sh '''
                    docker run -d -p 8080:80 --name test-container my-static-site
                    sleep 5
                    curl -f http://localhost:8080 || (echo "Test failed" && exit 1)
                    docker rm -f test-container
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo "$DOCKER_CREDENTIALS_PSW" | docker login -u "$DOCKER_CREDENTIALS_USR" --password-stdin
                    docker tag my-static-site $DOCKER_CREDENTIALS_USR/my-static-site:latest
                    docker push $DOCKER_CREDENTIALS_USR/my-static-site:latest
                '''
            }
        }

        stage('Deploy to AWS EC2 Review') {
            steps {
                sshagent(credentials: ['aws-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@REVIEW_EC2_IP '
                            docker pull $DOCKER_CREDENTIALS_USR/my-static-site:latest &&
                            docker rm -f site || true &&
                            docker run -d -p 80:80 --name site $DOCKER_CREDENTIALS_USR/my-static-site:latest
                        '
                    '''
                }
            }
        }

        stage('Deploy to AWS EC2 Staging') {
            steps {
                sshagent(credentials: ['aws-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@STAGING_EC2_IP '
                            docker pull $DOCKER_CREDENTIALS_USR/my-static-site:latest &&
                            docker rm -f site || true &&
                            docker run -d -p 80:80 --name site $DOCKER_CREDENTIALS_USR/my-static-site:latest
                        '
                    '''
                }
            }
        }
        stage('Deploy to AWS EC2 Production') {
            steps {
                sshagent(credentials: ['aws-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@PRODUCTION_EC2_IP '
                            docker pull $DOCKER_CREDENTIALS_USR/my-static-site:latest &&
                            docker rm -f site || true &&
                            docker run -d -p 80:80 --name site $DOCKER_CREDENTIALS_USR/my-static-site:latest
                        '
                    '''
                }
            }
        }
        stage('Cleanup') {
            steps {
                sh 'docker rmi my-static-site || true'
            }
        }
    }
}
