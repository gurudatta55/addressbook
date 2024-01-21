pipeline {
    agent none

    stages {
        stage('compile') {
            agent any
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('unit test') {
            agent any
            steps {
                sh 'mvn test'
            }
        }
        
        stage('packahe') {
            agent any
            steps {
                sh 'mvn package'
            }
        }
        
        stage('deploy') {
            agent any
            steps {
                echo 'deploy the code'
                echo 'extra step added to test'
            }
        }
        
    }
}
