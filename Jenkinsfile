pipeline {
    agent any
    environment {
              APP_NAME = "loginregisterapp"
    }
    parameters {
    string(name: 'release_version', )
  }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
               steps {
                   git changelog: false, poll: false, url: 'https://github.com/vedantguptha/k8-manifest-files.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${params.release_version}/g' deployment.yaml
                """
            }
        }

         stage("New Deploymnet File") {
            steps {
                sh " cat deployment.yaml " 
            }
        }

        stage("Commit Changes") {
            steps {
                sh """
                   git config --global user.name "vedantguptha"
                   git config --global user.email "vedantguptha.devops@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
            }
        }
        stage("Push") {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'git-hub-id', gitToolName: 'git-tool')]) {
                    sh "git push https://github.com/vedantguptha/k8-manifest-files.git master"
                }
            }
        }
    }
}