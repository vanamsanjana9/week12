
pipeline {
    agent any

    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat 'pip install -r requirements.txt'

                bat 'start /B python app.py'

                bat 'timeout /t 5'

                bat 'pytest tests\\test_registrationapp.py --maxfail=1 --disable-warnings'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat 'docker build -t seleniumdemapp:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                bat 'docker login -u vanamsanjana9'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                bat 'docker tag seleniumdemapp:v1 bhavani765/sample:seleniumtestimage'
                bat 'docker push vanamsanjana9/sample:seleniumtestimage'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml --validate=false'
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
