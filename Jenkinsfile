pipeline {
    agent any
    
    environment {
        MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD') // Jenkins credentials for MySQL root password
        MYSQL_DATABASE = 'Plutushub'
        MYSQL_USER = 'user_plutus'
        MYSQL_PASSWORD = credentials('MYSQL_USER_PASSWORD') // Jenkins credentials for MySQL user password
        DOCKER_HOST = "tcp://65.0.80.186:2375" // Set to your Docker server's IP
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository containing the Dockerfile
                git 'https://github.com/Rish1711/MySqlDeployment.git'
            }
        }
        
        
        stage('Build Docker Image on Remote Server') {
            steps {
                script {
                    // Builds Docker image on the remote Docker server
                    sh 'docker -H ${DOCKER_HOST} build -t my-mysql-image .'
                }
            }
        }
        
        stage('Run MySQL Container on Remote Server') {
            steps {
                script {
                    // Remove any existing container with the same name on the remote Docker server
                    sh 'docker -H ${DOCKER_HOST} rm -f my-mysql-container || true'

                    // Runs a new MySQL container on the remote Docker server with the custom image
                    sh """
                        docker -H ${DOCKER_HOST} run -d \
                        --name my-mysql-container \
                        -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} \
                        -e MYSQL_DATABASE=${MYSQL_DATABASE} \
                        -e MYSQL_USER=${MYSQL_USER} \
                        -e MYSQL_PASSWORD=${MYSQL_PASSWORD} \
                        -p 3306:3306 \
                        my-mysql-image
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                // List running Docker containers on the remote server as confirmation
                sh 'docker -H ${DOCKER_HOST} ps'
            }
        }
        cleanup {
            // Optional cleanup if desired on the remote Docker server
            // sh 'docker -H ${DOCKER_HOST} stop my-mysql-container'
            // sh 'docker -H ${DOCKER_HOST} rm my-mysql-container'
        }
    }
}