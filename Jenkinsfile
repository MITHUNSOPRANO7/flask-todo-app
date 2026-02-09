pipeline {
    agent any
    
    environment {
        EC2_HOST = '16.171.142.62'
        EC2_USER = 'ubuntu'
        PROJECT_DIR = '/home/ubuntu/flask-todo-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MITHUNSOPRANO7/flask-todo-app.git'
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                script {
                    sh """
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no docker-compose.yml ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no Dockerfile ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no requirements.txt ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no app.py ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no init.sql ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        scp -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no -r templates ${EC2_USER}@${EC2_HOST}:${PROJECT_DIR}/
                        
                        ssh -i /var/jenkins_home/flask-todo-key.pem -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                            cd ${PROJECT_DIR}
                            docker-compose down
                            docker-compose up -d --build
                            docker ps
                        '
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Deployment to EC2 successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
