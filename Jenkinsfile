pipeline {
    agent any  

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Creating virtual environment...'
                    sh 'python3 -m venv venv'  // Change to python3
                    echo 'Activating virtual environment and installing dependencies...'
                    // For Linux
                    sh 'source venv/bin/activate && pip install -r requirements.txt'
                    // For Windows (if needed)
                    // sh 'venv\\Scripts\\activate && pip install -r requirements.txt' 
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // For Linux
                    sh 'source venv/bin/activate && pytest tests'
                    // For Windows (if needed)
                    // sh 'venv\\Scripts\\activate && pytest tests'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
