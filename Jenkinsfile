pipeline {
    
    agent any


    stages {

        stage("Git checkout") {
            steps {
                echo "Cloniing the Pythonn project source code from the GitHub repository"
                git branch: 'main', url: 'https://github.com/falakdesai/python-onepiece-app'
            }
        }
    }

}