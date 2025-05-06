pipeline {
    agent any

    tools {
        jdk 'jdk11'
    }

    stages {
        stage('Clone-Repo') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                // Copy WAR to remote server
                sh '''
                sshpass -p "vijaykumar" scp target/gamutkart.war vijay@172.17.0.2:/jobs/apache-tomcat-9.0.104/webapps
                '''

                // Start Tomcat using sudo
                sh '''
sshpass -p "vijaykumar" ssh -o StrictHostKeyChecking=no vijay@172.17.0.2 "/jobs/apache-tomcat-9.0.104/bin/startup.sh"
'''

            }
        }
    }
}

