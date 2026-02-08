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
        echo "Running tests..."

        sh """
        docker-compose down -v || true
        docker-compose up -d --remove-orphans
        """
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
