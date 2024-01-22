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
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('package') {
            //agent {label 'slave1'}
            agent any
            // on ssh2 with ssh plugin
            steps {
                sshagent(['build-server-key']) {
                
            sh "scp -o StrictHostKeyChecking=no server-script.sh ec2-user@172.31.5.43:/home/ec2-user"
            sh "ssh -o StrictHostKeyChecking=no server-script.sh ec2-user@172.31.5.43 ~ec2-user/server-script.sh"
    
                }
                
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
