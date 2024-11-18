pipeline {
    agent any
    environment {
        APP_NAME = "testsysadmin8//nginx-game"
        GIT_USER = "git-ed-hub"
        GIT_TOKEN = 'github'
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
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                sh "git push https://${GIT_USER}:${github}@github.com/${GIT_USER}/flappybird-deployment.git main"
            }
        }
    }
}
