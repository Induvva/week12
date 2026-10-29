
pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\duvva\\AppData\\Local\\Programs\\Python\\Python313\\python.exe'
    }

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat '"%PYTHON%" -m pip install -r requirements.txt'

                bat 'start /B "FlaskApp" "%PYTHON%" app.py'

                bat 'ping 127.0.0.1 -n 5 > nul'

                bat '"%PYTHON%" -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"

                bat 'docker build -t week12-app:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging in to Docker Hub"

                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                     bat 'echo %DOCKER_TOKEN%| docker login -u "%DOCKER_USER%" --password-stdin'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub"

                bat 'docker tag week12-app:v1 induduvva/week12-app:v1'

                bat 'docker push induduvva/week12-app:v1'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes"

                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
