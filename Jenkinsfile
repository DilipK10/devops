pipeline {
    agent any

    environment {
        // Environment variables
        GITHUB_CREDENTIALS = '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'  // Replace with your GitHub credentials ID in Jenkins
        BRANCH = 'main'  // Change to your default branch
        PYTHON_VIRTUALENV = 'python-venv'  // Virtual environment name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub repository
                git branch: "${BRANCH}", credentialsId: "${GITHUB_CREDENTIALS}", url: 'https://github.com/DilipK10/devops.git'
            }
        }

       stage('Setup Python Environment') {
            steps {
                script {
                    // Check if python3-venv is installed
                    sh '''
                    if ! dpkg -l | grep -q python3-venv; then
                        echo "python3-venv not found, installing..."
                        sudo apt-get update && sudo apt-get install -y python3-venv
                    fi

                    # Create a Python virtual environment
                    python3 -m venv ${PYTHON_VIRTUALENV}
                    '''
                    // Now use bash to activate the virtualenv and install dependencies
                    sh '''
                    bash -c "source ${PYTHON_VIRTUALENV}/bin/activate && pip install --upgrade pip && pip install -r requirements.txt"
                    '''
                }
            }
        }

        stage('Run Python Script') {
            steps {
                script {
                    sh '''
                    bash -c "source ${PYTHON_VIRTUALENV}/bin/activate && python your_script.py"
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    bash -c "source ${PYTHON_VIRTUALENV}/bin/activate && pytest --maxfail=1 --disable-warnings -q"
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
                    echo 'Deploying the application to production...'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'rm -rf ${PYTHON_VIRTUALENV}'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
