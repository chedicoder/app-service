pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "app:v1"
        USERNAME = "chedi1"
        EMAIL_ADDRESS = "chadighribi2@gmail.com"
    }
    stages {
      
        stage('Clone & Switch to project branch') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        
        stage('Infrastructure Provisioning') {
            steps {
                script {
                    sh '''
                        cd terraform
                        ./deploytf.sh
                    '''
                }
            }
        }

        stage('Deploy Application to azure app service') {
            steps {
                script {
                    sh 'az webapp deployment source config --name flask-app-chedi --resource-group flask-app-rg --repo-url https://github.com/chedicoder/Terraform-Repo.git --branch main --manual-integration'
                }
            }
        }
    }

    post {
        failure {
            mail to: "${env.EMAIL_ADDRESS}",
                 cc: 'ccedpeople@gmail.com',
                 subject: "FAILED: Build ${env.JOB_NAME}",
                 body: "Build failed ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}.\n\nView the log at:\n${env.BUILD_URL}\n\nBlue Ocean:\n${env.RUN_DISPLAY_URL}"
        }

        success {
            mail to: "${env.EMAIL_ADDRESS}",
                 cc: 'ccedpeople@gmail.com',
                 subject: "SUCCESSFUL: Build ${env.JOB_NAME}",
                 body: "Build Successful ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}\n\nView the log at:\n${env.BUILD_URL}\n\nBlue Ocean:\n${env.RUN_DISPLAY_URL}"
        }

        aborted {
            mail to: "${env.EMAIL_ADDRESS}",
                 cc: 'ccedpeople@gmail.com',
                 subject: "ABORTED: Build ${env.JOB_NAME}",
                 body: "Build was aborted ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}\n\nView the log at:\n${env.BUILD_URL}\n\nBlue Ocean:\n${env.RUN_DISPLAY_URL}"
        }
    }
}
