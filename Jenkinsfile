pipeline {
    agent none
    
    environment{
        IMAGE_NAME='gurudattaaws/box1:$BUILD_NUMBER'
        DEV_SERVER_IP='ec2-user@65.0.89.121'
        deploy_server='ec2-user@13.235.51.103'
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
           //  agent any
           agent any
            steps {
                
                sshagent(['build-server-key']) {
                withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                

                //sh 'mvn compile'


                sh "scp -o StrictHostKeyChecking=no server-script.sh ${DEV_SERVER_IP}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${DEV_SERVER_IP} bash /home/ec2-user/server-script.sh"
                sh "ssh ${DEV_SERVER_IP} sudo docker build -t ${IMAGE_NAME} /home/ec2-user/addressbook"
                sh "ssh ${DEV_SERVER_IP} sudo docker login -u $USERNAME -p $PASSWORD"
                sh "ssh ${DEV_SERVER_IP} sudo docker push ${IMAGE_NAME}"

                }
                }
                
            }
        }


        stage('deploy') {
           //  agent any
           agent any
            steps {
                
                sshagent(['build-server-key']) {
                withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                

                
                
                sh "scp -o StrictHostKeyChecking=no server-script.sh ${deploy_server}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${deploy_server} bash /home/ec2-user/server-script.sh"
                sh "ssh ${deploy_server} sudo docker login -u $USERNAME -p $PASSWORD"
                sh "ssh ${deploy_server} sudo docker run -itd -p 8080:8080 ${IMAGE_NAME}"
                }
                }
                
            }
        }
    }
}
