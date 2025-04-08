pipeline {
    agent any

    environment {
        TOMCAT_SERVER = "ubuntu@54.173.54.129"
        WAR_FILE = "TrainBook-1.0.0-SNAPSHOT.war"
        TOMCAT_WEBAPPS = "/home/ubuntu/tomcat/tomcat1/webapps/"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/JeeruPrasanth96/HelloWorld.git'
            }
        }

        stage('Build WAR File') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat-ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/${WAR_FILE} ${TOMCAT_SERVER}:${TOMCAT_WEBAPPS}
                        ssh -o StrictHostKeyChecking=no ${TOMCAT_SERVER} << EOF
                            cd /home/ubuntu/tomcat/tomcat1/bin
                            ./shutdown.sh
                            ./startup.sh
                        EOF
                    '''
                }
            }
        }
    }
}
