pipeline {
    agent none

    environment{
        IMAGE_NAME='gurudattaaws/box1:$BUILD_NUMBER'
        DEV_SERVER_IP='ec2-user@3.110.223.15'
       // APP_NAME='java-mvn-app'
    }

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

                withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'password', usernameVariable: 'username')]){
    
                
                sh "scp -o StrictHostKeyChecking=no server-script.sh ${DEV_SERVER_IP}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${DEV_SERVER_IP} bash ~ec2-user/server-script.sh"

                sh "ssh ${DEV_SERVER_IP} sudo docker build -t  ${IMAGE_NAME} /home/ec2-user/addressbook"
                    sh "ssh ${DEV_SERVER_IP} sudo docker login -u $username -p $password"
                    sh "ssh ${DEV_SERVER_IP} sudo docker push ${IMAGE_NAME}"
                }
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
