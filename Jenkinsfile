pipeline {
    agent {
        node {
            label "node_01"
        }
    }
    stages {
        stage('Clone repository') {
            steps {
                git url: 'git@github.com:AleksandMur/word01.git',
                credentialsId: "github-ssh"
            }
        }
        stage('Test') {
            steps {
                sh """
                kubectl apply -f pv.yaml
                kubectl apply -f pvc.yaml
                kubectl apply -f deploy.yaml
                kubectl apply -f ingres.yaml
                """
            }
        }
    }    
    post {
        success {
                slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
            failure {
                slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        always {
            deleteDir()
        }
    }
}
