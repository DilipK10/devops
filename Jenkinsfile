pipeline {
    agent any

    environment {
        // Environment variables
        GITHUB_CREDENTIALS = 'github-credentials-id'  // Replace with your GitHub credentials ID in Jenkins
        BRANCH = 'main'  // Change to your default branch
        PYTHON_VIRTUALENV = 'python-venv'  // Virtual environment name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub repository
                git branch: "${BRANCH}", credentialsId: "${GITHUB_CREDENTIALS}", url: 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Install Python and create a virtual environment
                    sh '''
                    python3 -m venv ${PYTHON_VIRTUALENV}
                    source ${PYTHON_VIRTUALENV}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Run Python Script') {
            steps {
                script {
                    // Run a Python script (make sure it's in the repository)
                    sh '''
                    source ${PYTHON_VIRTUALENV}/bin/activate
                    python your_script.py
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests after the script (e.g., unit tests or integration tests)
                    sh '''
                    source ${PYTHON_VIRTUALENV}/bin/activate
                    pytest --maxfail=1 --disable-warnings -q
                    '''
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                script {
                    // Deploy the application (you can replace this with any deployment logic)
                    echo 'Deploying the application to production...'
                    // You can add deployment commands here (e.g., SSH, Docker, etc.)
                }
            }
        }
    }

    post {
        always {
            // Clean up actions like removing temporary files or environments
            echo 'Cleaning up...'
            sh 'rm -rf ${PYTHON_VIRTUALENV}'
        }
        success {
            // Actions to take if the pipeline succeeds
            echo 'Pipeline completed successfully!'
        }
        failure {
            // Actions to take if the pipeline fails
            echo 'Pipeline failed!'
        }
    }
}
