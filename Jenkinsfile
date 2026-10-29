
pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat 'python -m pip install -r requirements.txt'

                bat 'start /B "FlaskApp" python app.py'

                bat 'ping 127.0.0.1 -n 5 > nul'

                bat 'python -m pytest -v'
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
                bat 'docker login -u induduvva'
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
