pipeline {
    agent any  // This defines that the pipeline can run on any available agent

    stages {
        stage('Build') {
            steps {
                // Use a build automation tool, e.g., setup virtual environment and install requirements
                script {
                    echo 'Building the application...'
                    sh 'python3 -m venv venv'
                    sh 'venv/Scripts/activate && pip install -r requirements.txt'  // For Windows
                    // sh 'source venv/bin/activate && pip install -r requirements.txt'  // For Linux/Mac
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                // Run unit tests using pytest
                script {
                    echo 'Running unit tests...'
                    sh 'venv/Scripts/activate && pytest tests'  // For Windows
                    // sh 'source venv/bin/activate && pytest tests'  // For Linux/Mac
                }
            }
        }

        stage('Code Analysis') {
            steps {
                // Integrate a code analysis tool, e.g., Flake8
                script {
                    echo 'Running code analysis...'
                    sh 'venv/Scripts/activate && flake8 app'  // For Windows
                    // sh 'source venv/bin/activate && flake8 app'  // For Linux/Mac
                }
            }
        }

        stage('Security Scan') {
            steps {
                // Perform a security scan using Bandit
                script {
                    echo 'Running security scan...'
                    sh 'venv/Scripts/activate && bandit -r app'  // For Windows
                    // sh 'source venv/bin/activate && bandit -r app'  // For Linux/Mac
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                // Deploy the application to a staging server (you can replace this with actual deployment commands)
                script {
                    echo 'Deploying to staging...'
                    // Example: Deploy using SSH or any other method
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on the staging environment
                script {
                    echo 'Running integration tests on staging...'
                    sh 'venv/Scripts/activate && pytest tests/integration_tests'  // For Windows
                    // sh 'source venv/bin/activate && pytest tests/integration_tests'  // For Linux/Mac
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                // Deploy the application to the production server
                script {
                    echo 'Deploying to production...'
                    // Example: Deploy using SSH or any other method
                }
            }
        }
    }

    post {
        success {
            mail to: 'anushri20sept@gmail.com',
                 subject: "Jenkins: Build ${currentBuild.fullDisplayName} - Success",
                 body: "The build was successful!\nCheck the details at ${env.BUILD_URL}"
        }
        failure {
            mail to: 'anushri20sept@gmail.com',
                 subject: "Jenkins: Build ${currentBuild.fullDisplayName} - Failed",
                 body: "The build has failed.\nCheck the details at ${env.BUILD_URL}"
        }
    }
}
