pipeline {
    agent any
    environment {
              APP_NAME = "register-app-pipeline"
              IMAGE_TAG = 1.0.0
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Print Workspace") {
            steps {
                sh '' echo params.Updated_Version_Id ''
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
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

         stage("New Deploymnet File") {
            steps {
                sh " cat deployment.yaml " 
            }
        }

        // stage("Push the changed deployment file to Git") {
        //     steps {
        //         sh """
        //            git config --global user.name "Ashfaque-9x"
        //            git config --global user.email "ashfaque.s510@gmail.com"
        //            git add deployment.yaml
        //            git commit -m "Updated Deployment Manifest"
        //         """
        //         withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
        //           sh "git push https://github.com/Ashfaque-9x/gitops-register-app main"
        //         }
        //     }
        // }
      
    }
}