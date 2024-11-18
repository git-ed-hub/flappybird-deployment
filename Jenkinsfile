pipeline {
    agent any
    environment {
        APP_NAME = "nginx-game-pipeline"
        GIT_USER = "git-ed-hub"
        GIT_TOKEN = 'github'
        GIT_EMAIL = 'email'
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/git-ed-hub/flappybird-deployment.git'
               }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name ${GIT_USER}
                   git config --global user.email ${GIT_EMAIL}
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/git-ed-hub/flappybird-deployment.git main"
            }
        }
    }
}
