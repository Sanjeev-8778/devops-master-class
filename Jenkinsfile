pipeline {
    agent any

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
                    echo '🧪 Running Node.js app...'
                    sh 'node index.js'
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
                    sh 'python app.py'
                }
            }
        }

        stage('Run JavaScript App') {
            when {
                expression { fileExists('js-app/index.html') }
            }
            steps {
                dir('js-app') {
                    echo '💻 Static JS app detected (index.html found).'
                    echo 'No build needed for static HTML/JS.'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
