pipeline {
    agent any

    tools {
        jdk 'jdk17'        // Use JDK version 11 or configure as per your setup
        maven 'Maven3'     // Maven tool configuration in Jenkins
    }

    environment {
        SCANNER_HOME = tool 'SonarQube' // SonarQube Scanner tool name in Jenkins
        DOCKER_CREDENTIALS = 'docker-hub-credentials' // Replace with your Docker Hub credentials ID
        DOCKER_IMAGE_NAME = 'score' // Replace with your Docker image name
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/DevOpsCrafter/JenkinsSonarqubeDocker.git'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=JenkinsSonarQubeDocker \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=squ_27b6550f9430cc6cfbcb42b7f7744658d2ac0ca8
                    """
                }
            }
        }

        // stage('Build with Maven') {
        //     steps {
        //         sh 'mvn clean package'
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: env.DOCKER_CREDENTIALS]) {
                        sh """
                        docker tag ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG} your-dockerhub-repo/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}
                        docker push your-dockerhub-repo/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}
                        """
                    }
                }
            }
        }

        stage('Deploy to Environment') {
            steps {
                echo 'Deployment steps to staging or production environment go here.'
            }
        }
    }

    post {
        always {
            cleanWs() // Cleanup workspace
        }
    }
}

