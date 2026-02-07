pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'flask-todo-app'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }
        
        stage('Build Docker Images') {
            steps {
                echo 'Building Docker images...'
                sh 'docker-compose build'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'docker-compose up -d'
                echo 'Waiting for MySQL to be ready...'
                sh '''
                    for i in 1 2 3 4 5 6 7 8 9 10; do
                        docker exec mysql_db mysqladmin ping -h localhost -u root -prootpassword --silent && break
                        echo "Waiting for MySQL... attempt $i"
                        sleep 3
                    done
                '''
                echo 'Waiting for Flask app to be ready...'
                sh 'sleep 5'
                sh 'docker exec flask_app curl -f http://localhost:5000 || exit 1'
                echo 'Tests passed!'
            }
        }
        
        stage('Push to Registry') {
            steps {
                echo 'Ready for deployment'
                // We'll add Docker Hub push later for AWS
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'docker-compose down || true'
                sh 'docker-compose up -d'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Cleaning up...'
            sh 'docker-compose down || true'
        }
    }
}
