pipeline {
    agent {
        label 'built-in-node'
    }


    environment {
        KUBE_CONFIG = "/var/lib/jenkins/.kube/config"  // Adjust if needed
        REPO_URL = "https://github.com/SriramVeldandi/DevOps.git"
        BRANCH = "master"
        DOCKER_USER = 'sriram2421'
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh "rm -rf DevOps && git clone -b ${BRANCH} ${REPO_URL} DevOps"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    docker rmi sriram2421/html-app:latest
                    docker build -t sriram2421/html-app:latest DevOps
                    echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USER --password-stdin
                    docker push sriram2421/html-app:latest
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl set image deployment/html-app-deployment html-app=sriram2421/html-app:latest --record"
                    sh "kubectl rollout restart deployment html-app-deployment"
                    sh "kubectl rollout status deployment/html-app-deployment"
                    sh "kubectl get svc html-app-service"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Access your app at http://<node-ip>:<node-port>"
        }
        failure {
            echo "Deployment failed! Check logs for errors."
        }
    }
}
