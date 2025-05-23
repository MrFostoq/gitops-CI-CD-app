pipeline {
    agent { label "Jenkins-agent" }
    environment {
              APP_NAME = "reg-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'githup', url: 'https://github.com/MrFostoq/gitops-CI-CD-app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's|image: fostoq/${APP_NAME}:.*|image: fostoq/${APP_NAME}:${IMAGE_TAG}|' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "MrFostoq"
                   git config --global user.email "abod18mon@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'githubwrite', gitToolName: 'Default')]) {
                  sh "git push https://github.com/MrFostoq/gitops-CI-CD-app.git main"
                }
            }
        }
      
    }
}