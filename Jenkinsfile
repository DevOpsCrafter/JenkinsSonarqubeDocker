tools {
    jdk 'jdk17'
}

environment {
    SCANNER_HOME = tool 'sonar-scanner'
}

stages {
    stage('Git Checkout') {
        steps {
            git credentialsId: 'github', url: 'https://github.com/praguee/containerizing-portfolio.git'
        }
    }

    stage('SonarQube Analysis') {
        steps {
            sh '''$SCANNER_HOME/bin/sonar-scanner \
                  -Dsonar.projectKey=portfolio-website \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=<your-sonar-token>'''
        }
    }

    stage('OWASP Dependency Check') {
        steps {
            dependencyCheck additionalArguments: "--project 'Portfolio' --scan . --nvdApiKey <your-nvd-api-key>",
                            odcInstallation: 'DependencyCheck'
        }
    }

    stage('Build & Push Docker Image') {
        steps {
            script {
                withDockerRegistry(credentialsId: 'Dockerhub') {
                    sh "docker build -t portfolio:latest ."
                    sh "docker tag portfolio:latest prrague/portfolio:latest"
                    sh "docker push prrague/portfolio:latest"
                }
            }
        }
    }

    stage('Trigger CD Pipeline') {
        steps {
            build job: 'CD pipeline', wait: true
        }
    }
}
