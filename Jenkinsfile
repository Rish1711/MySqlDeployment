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
                // Assuming the Dockerfile is stored in this Git repository
                git 'https://github.com/Rish1711/MySqlDeployment.git'
            }
        }
        
        stage('Build Docker Image on Remote Host') {
            steps {
                script {
                    // Using the Docker plugin to build the image on the remote Docker host
                    docker.build("my-mysql-image", ".", "--host=$DOCKER_HOST")
                }
            }
        }
        
        stage('Run MySQL Container on Remote Host') {
            steps {
                script {
                    // Using the Docker plugin to run the container on the remote Docker host
                    docker.image("my-mysql-image").run(
                        "-d --name my-mysql-container " +
                        "-e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} " +
                        "-e MYSQL_DATABASE=${MYSQL_DATABASE} " +
                        "-e MYSQL_USER=${MYSQL_USER} " +
                        "-e MYSQL_PASSWORD=${MYSQL_PASSWORD} " +
                        "-p 3306:3306"
                    )
                }
            }
        }
    }

    post {
        always {
            script {
                // Show running containers on the remote host as a confirmation
                sh "docker -H $DOCKER_HOST ps"
            }
        }
    }
}
