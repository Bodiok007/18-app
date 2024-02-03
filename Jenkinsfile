pipeline {
    agent any
    
    environment {
        // Set the NodeJS installation defined in Jenkins
        NODEJS_HOME = tool 'NodeJS'
        PATH = "${NODEJS_HOME}/bin:${PATH}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'CleanBeforeCheckout']],
                    userRemoteConfigs: [[url: 'https://github.com/Bodiok007/18-app.git']]
                ])
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                npm install
                npm run build
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                npm test
                '''
            }
        }
    }
    
    post {
        always {
            emailext (
                subject: "Jenkins Pipeline - ${currentBuild.result}",
                body: "Pipeline ${currentBuild.result}: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                attachLog: true,
            )
        }
    }
}
