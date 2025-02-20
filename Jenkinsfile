pipeline {
    agent {
        docker { 
            image 'cypress/base:22.13.1'
            args '-u root --network cjslack_skynet'
        }
    }

    environment {
        SLACK_CHANNEL = 'jenkins-notification'
        SLACK_TOKEN = 'slack-token'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                sh 'npx cypress install --force'
            }
        }

        stage('Run E2E Tests') {
            steps {
                sh 'npx cypress run'
            }
        }
    }

    post {
        success {
            slackSend color: "#6AA84F", channel: env.SLACK_CHANNEL, 
                      message: "✅ Os testes do sistema ${env.JOB_NAME} foram executados com sucesso! 🚀\nBuild: ${env.BUILD_NUMBER}\n🔗 Acesse: ${env.BUILD_URL}",
                      tokenCredentialId: env.SLACK_TOKEN
        }
        failure {
            slackSend color: "#D9534F", channel: env.SLACK_CHANNEL, 
                      message: "❌ Falha nos testes do sistema ${env.JOB_NAME}! ⚠️\nBuild: ${env.BUILD_NUMBER}\n🔗 Acesse: ${env.BUILD_URL} para mais detalhes.",
                      tokenCredentialId: env.SLACK_TOKEN
        }
    }
}
