pipeline {
    agent none
    
    environment{
        IMAGE_NAME='gurudattaaws/box1:$BUILD_NUMBER'
        DEV_SERVER_IP='ec2-user@3.108.228.203'
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
            
            //post{
                //always{
                   // junit 'targrt/surefire-reports/*.xml'
               // }
            //}
        }
        
        stage('package') {
           //  agent any
           agent any
            steps {
                
                sshagent(['build-server-key']) {
                withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'docker-password', usernameVariable: 'docker-username')]) {
                

                sh 'mvn compile'
                sh "scp -o StrictHostKeyChecking=no server-script.sh ${DEV_SERVER_IP}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${DEV_SERVER_IP} bash /home/ec2-user/server-script.sh"
                sh "ssh ${DEV_SERVER_IP} sudo docker build -t ${IMAGE_NAME} /home/ec2-user/addressbook"
                sh "ssh ${DEV_SERVER_IP} sudo docker login -u ${docker-username} -p ${docker-password}"
                sh "ssh ${DEV_SERVER_IP} sudo docker push ${IMAGE_NAME}"
                }
                }
                
            }
        }
    }
}
