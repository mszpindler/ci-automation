pipeline {
    agent any
    stages {
        stage('Stage 1') {
            echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            sh "curl ipinfo.io/ip"
        }
    }
}
