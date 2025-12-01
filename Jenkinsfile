pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/your-app:latest"
        INVENTORY_FILE = "/etc/ansible/hosts"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-app-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to EC2 using Ansible') {
            steps {
                sh '''
                    ansible-playbook -i $INVENTORY_FILE deploy.yml
                '''
            }
        }
    }
}
