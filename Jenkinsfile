pipeline {
    agent any
    stages {
        stage('GitCheckout') {
            steps {
                git 'https://github.com/SriramVeldandi/DevOps.git'
            }
        }
        /*
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
                    sh 'docker run -itd -p 81:80 --name=myimagecontainer sriram2421/myimage'
                }
            }
        }
        */
        stage('K8S Deploy') {
            steps {
                script {
                    sh 'sudo kubectl apply -f deployment.yaml'
                    sh 'sudo kubectl apply -f service.yaml'
                }
            }
        }
    }
}
