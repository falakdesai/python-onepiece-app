pipeline {
    
    agent any


    stages {

        stage("Git checkout") {
            steps {
                echo "Cloniing the Pythonn project source code from the GitHub repository"
                git branch: 'main', url: 'https://github.com/falakdesai/python-onepiece-app'
            }
        }
        
        stage("python build") {
            steps {
                echo "Building the Python application"
                sh '''
                rm -rf venv # Remove existing virtual environment if any
                python -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage("Python test") {
            steps {
                echo "Running tests for the Python application"
                sh '''
                . venv/bin/activate
                pytest app/tests/test_app.py
                '''
            }
        }

        stage("sonarQube scan") {
            environment { SONARQUBE_SCANNER_HOME = tool 'sonar-scanner' }
            steps {
                echo "Running SonarQube scan for code quality analysis"
                withSonarQubeEnv('sonarqube-local') {
                    sh '''
                    . venv/bin/activate
                    $SONARQUBE_SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=Jenkins \
                    -Dsonar.sources=. \
                    '''
                }
            }
        }

        stage("publish sonarQube quality gate") {
            environment { SONARQUBE_SCANNER_HOME = tool 'sonar-scanner' }
            steps {
                echo "Re-running SonarQube scan for code quality analysis"
                withSonarQubeEnv('sonarqube-local') {
                    sh '''
                    . venv/bin/activate
                    $SONARQUBE_SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=Jenkins \
                    -Dsonar.sources=. \
                    -Dsonarqualitygate.wait=false
                    '''
                }
            }
        }



    }

}