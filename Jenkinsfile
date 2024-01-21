pipeline {
    agent any

    stages {
        stage('compile') {
            steps {
                echo 'compile the code'
            }
        }
        
        stage('unit test') {
            steps {
                echo 'test the code'
            }
        }
        
        stage('build') {
            steps {
                echo 'buld the code'
            }
        }
        
        stage('deploy') {
            steps {
                echo 'deploy the code'
                echo 'extra step added to test'
            }
        }
        
    }
}
