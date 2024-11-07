pipeline {
    agent any
    
    environment {
        MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD')
        MYSQL_DATABASE = 'Plutushub'
        MYSQL_USER = 'user_plutus'
        MYSQL_PASSWORD = credentials('MYSQL_USER_PASSWORD')
        DOCKER_HOST = "tcp://13.235.100.70:2375"
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the main branch explicitly
                git branch: 'main', url: 'https://github.com/Rish1711/MySqlDeployment.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image for MySQL
                    sh 'docker -H $DOCKER_HOST build -t my-mysql-image .'
                }
            }
        }
        
        stage('Run MySQL Container') {
            steps {
                script {
                    // Remove any existing container with the same name
                    sh 'docker -H $DOCKER_HOST rm -f my-mysql-container || true'

                    // Run a new MySQL container with the custom image
                    sh """
                        docker -H $DOCKER_HOST run -d \
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
                // Show running containers as a confirmation
                sh 'docker -H $DOCKER_HOST ps'
            }
        }
    }
}
