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
            steps {
            sh 're trufflehog||true'
            sh 'docker run gesellix/trufflehog --json https://github.com/ek2020/webapp.git > trufflehog'
            sh 'cat  trufflehog'
            }
        }
        stage('Source Composition Analysis') {
            steps {
                sh 'rm owasp* || true'
                sh 'wget "https://raw.githubusercontent.com/ek2020/webapp/master/owasp-dependency-check.sh" '
                sh 'chmod +x owasp-dependency-check.sh'
                sh 'bash owasp-dependency-check.sh'
                sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
            }
        }
        stage('Build') {
            steps {
                sh  'mvn clean package' 
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.145.31.12:/home/ubuntu/prod/apache-tomcat-8.5.55/webapps/webapp.war'
                }
        }
        }
    }
}
