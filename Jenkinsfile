pipeline {
    agent any
    options {
        timestamps()
    }
    stages {
        stage ('Production') {
            when {
                branch 'main'
            }
            steps {
                withCredentials([
                    string(credentialsId: 'PRODUCTION_HOST', variable: 'HOST'),
                    string(credentialsId: 'PRODUCTION_PORT', variable: 'PORT'),
                ]) {
                    sshagent (credentials: ['PRODUCTION_AUTH']) {
                        sh 'make deploy'
                    }
                }
            }
        }
    }
    post {
        failure {
            emailext (
                subject: "FAIL Job ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                body: "Check console output at: ${env.BUILD_URL}/console",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}