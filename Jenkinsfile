pipeline {
    agent any
    
    environment {
        MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD') // Use Jenkins credentials for password
        MYSQL_DATABASE = 'Plutushub'
        MYSQL_USER = 'user_plutus'
        MYSQL_PASSWORD = credentials('MYSQL_USER_PASSWORD')
        DOCKER_HOST = "tcp://65.0.80.186:2375" // Remote Docker server address
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the Git repository from the main branch
                git branch: 'main', url: 'https://github.com/Rish1711/MySqlDeployment.git'
            }
        }
        
        stage('Build Docker Image on Remote Host') {
            steps {
                script {
                    // Build Docker image on the remote Docker server
                    sh 'docker -H $DOCKER_HOST build -t my-mysql-image .'
                }
            }
        }
        
        stage('Run MySQL Container on Remote Host') {
            steps {
                script {
                    // Remove any existing container with the same name on the remote Docker server
                    sh 'docker -H $DOCKER_HOST rm -f my-mysql-container || true'

                    // Run a new MySQL container with the custom image on the remote Docker server
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
                // Show running containers on the remote host as a confirmation
                sh 'docker -H $DOCKER_HOST ps'
            }
        }
    }
}
