pipeline {
    agent any
    tools {
        maven 'Maven' 
    }
    stages {
        stage('initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage('Check-git-Secrets')
        {
            sh 'docker run cloudkats/trufflehog https://github.com/ek2020/webapp.git'
        }
        stage('Build') {
            steps {
                sh  'mvn clean package' 
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.208.105.127:/home/ubuntu/prod/apache-tomcat-8.5.55/webapps/webapp.war'
                }
        }
        }
    }
}
