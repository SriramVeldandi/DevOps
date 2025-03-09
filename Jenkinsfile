pipeline {
    agent any
    stages {
        stage('GitCheckout') {
            steps {
                git 'https://github.com/SriramVeldandi/DevOps.git'
            }
        }
        stage('Docker Remove Container') {
            steps {
                script {
                    sh 'docker rm -f myimagecontainer'
                }
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId:'DockerLogin', usernameVariable: 'DockerUsername', passwordVariable: 'DockerPassword')]){
                    script {
                        sh 'docker rmi sriram2421/myimage'
                        sh 'docker build -t sriram2421/myimage .'
                        sh 'docker login -u ${DockerUsername} -p ${DockerPassword}'
                        sh 'docker push sriram2421/myimage'
                    }
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    sh 'docker run -itd -p 80:80 --name=myimagecontainer sriram2421/myimage'
                }
            }
        }
        stage('K8S Deploy') {
            steps {
                script {
                    echo 'hello'
                    //sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl set image deployment my-app-deployment my-app-container=sriram2421/myimage --record'
                    sh 'kubectl rollout restart deployment my-app-deployment'
                    sh 'kubectl rollout status deployment/html-app-deployment'
                    sh 'kubectl apply -f service.yaml'
                    sh 'kubectl get services'
                }
            }
        }
    }
}
