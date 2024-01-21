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
            //post {
               // always {
                //    junit 'target/surefire-report/*.xml'
               // }
           // }
        }
        
        stage('package') {
            agent {label 'slave1'}
            steps {
                sh 'mvn package'
            }
        }
        
        stage('deploy') {
            agent any
            steps {
                echo 'deploy the code'
            }
        }
        
    }
}
