pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'Maven3'
    }

    environment {
        SCANNER_HOME = tool 'SonarQube Scanner'
    }


    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', 
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://DevOpsCrafter@github.com/DevOpsCrafter/JenkinsSonarqubeDocker.git', 
                        credentialsId: 'github'
                    ]]
                ])
            }
        }
    }
   

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // 'SonarQube' matches the name in Manage Jenkins → Configure System
                    sh '''${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=portfolio-website \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=squ_27b6550f9430cc6cfbcb42b7f7744658d2ac0ca8'''
                }
            }
        }




        stage('Dependency Check') {
            steps {
                dependencyCheck additionalArguments: "--project 'Portfolio' --scan .",
                                odcInstallation: 'DependencyCheck'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('portfolio:latest')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-hub-credentials']) {
                        sh 'docker tag portfolio:latest acraterdevops/ScoreMe:latest'
                        sh 'docker push acraterdevops/ScoreMe:latest'
                    }
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deployment to staging environment'
            }
        }
    }


