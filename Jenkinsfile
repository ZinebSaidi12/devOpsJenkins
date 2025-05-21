pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // ID configuré dans Jenkins → Credentials
    }

    stages {
        stage('Build') {
            steps {
                echo '🔨 Building the application...'
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Test') {
            steps {
                echo '✅ Running tests...'
                // Exemple de test : python -m unittest discover
                sh 'echo "No tests implemented yet."'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📤 Pushing the Docker image to Docker Hub...'
                sh '''
                    echo "${DOCKER_HUB_CREDENTIALS_PSW}" | docker login -u "${DOCKER_HUB_CREDENTIALS_USR}" --password-stdin
                    docker tag my-python-app ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest
                    docker push ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying the application to remote server...'
                sh '''
                    ssh user@remote-server "docker pull ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest"
                    ssh user@remote-server "docker stop my-python-app || true"
                    ssh user@remote-server "docker rm my-python-app || true"
                    ssh user@remote-server "docker run -d -p 5000:5000 --name my-python-app ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest"
                '''
            }
        }
    }
}
