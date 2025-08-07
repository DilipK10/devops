pipeline {
    agent any

    environment {
        PYTHON_VENV = "python-venv"
    }

    stages {
        stage('Clone') {
            steps {
                script {
                    echo 'Cloning repository from GitHub...'
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Setting up Python environment...'

                    // Check if python3-venv is installed, if not, skip it
                    sh '''
                    dpkg -l | grep -q python3-venv || echo "python3-venv is already installed"
                    '''
                    
                    // Create a virtual environment and activate it using bash
                    sh '''
                    python3 -m venv ${PYTHON_VENV}
                    bash -c "source ${PYTHON_VENV}/bin/activate && pip install --upgrade pip && pip install -r requirements.txt"
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh '''
                    bash -c "source ${PYTHON_VENV}/bin/activate && pytest --maxfail=1 --disable-warnings -q"
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    echo "Deployment commands go here"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh "rm -rf ${PYTHON_VENV}"
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
