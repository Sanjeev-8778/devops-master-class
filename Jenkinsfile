pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/your-image-name"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Cloning Repository...'
                checkout scm
            }
        }

        stage('Build Node.js App') {
            when {
                expression { fileExists('node-app/package.json') }
            }
            steps {
                dir('node-app') {
                    echo '📦 Installing Node.js dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Run Python App') {
            when {
                expression { fileExists('python-app/requirements.txt') }
            }
            steps {
                dir('python-app') {
                    echo '🐍 Installing Python requirements...'
                    sh 'pip install -r requirements.txt'
                    echo '🚀 Running Python app...'
                    sh 'python app.py &'
                }
            }
        }

        stage('Run JavaScript App') {
            when {
                expression { fileExists('js-app/index.html') }
            }
            steps {
                dir('js-app') {
                    echo '💻 Static JS app detected.'
                }
            }
        }

        stage('Docker Build') {
            steps {
                echo "🐳 Building Docker image $IMAGE_NAME:$IMAGE_TAG..."
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Docker Push') {
            steps {
                echo "📤 Pushing Docker image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI pipeline completed successfully!'
        }
        failure {
            echo '❌ CI pipeline failed!'
        }
    }
}
