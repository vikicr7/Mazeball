pipeline {
    agent { label 'dep' }
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/vikicr7/Mazeball.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t mazeimage /home/vijay/workspace/game-project'
                sh 'sudo docker tag mazeimage vikicr7/mazeimage:latest'
                sh 'sudo docker tag mazeimage vikicr7/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Trivy Vulnerability Scan') {
            steps {
                script {
                    sh 'sudo trivy image vikicr7/mazeimage:latest'
                    sh 'sudo trivy image vikicr7/mazeimage:${BUILD_NUMBER}'
                }
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push vikicr7/mazeimage:latest'
                sh 'sudo docker image push vikicr7/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /home/vijay/workspace/game-project/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
