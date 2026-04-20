pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t jmeter-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f jmeter-app || true'
                sh 'docker run -d --name jmeter-app -p 8081:80 jmeter-app'
            }
        }

        stage('Test JMeter') {
            steps {
                sh 'jmeter -n -t plan-jenkins.jmx -l results.jtl'
            }
        }
    }

    post {
        always {
            perfReport sourceDataFiles: 'results.jtl'
        }
    }
}
