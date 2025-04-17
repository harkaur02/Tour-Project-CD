pipeline {
    agent any
    environment {
        DOCKERTAG = "${BUILD_NUMBER}"
        GIT_USER_NAME ="harkaur02"
        GIT_REPO_NAME = "https://github.com/harkaur02/Tour-Project.git"
    }
    stages {
        stage ('Update Deployment Manifest file') {
            steps {
                script {
                    //sed -i 's+thethymca/next-node-js-app:*+thethymca/next-node-js-app:'"$DOCKERTAG"'+g'
                    //catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(
                            credentialsId: 'git-credentials',
                            usernameVariable: 'GIT_USERNAME',
                            passwordVariable: 'GIT_PASSWORD'
                        )]) {
                            sh """
                                git config user.email "harkaur02@gmail.com"
                                git config user.name "harkaur02"

                                echo "deployment.yaml file before update"
                                cat k8s/deployment.yaml

                                sed -i 's+thethymca/next-node-js-app:[^ ]*+thethymca/next-node-js-app:"$DOCKERTAG"+g' k8s/deployment.yaml

                                echo "deployment.yaml file after update....."
                                cat k8s/deployment.yaml

                                git add k8s/deployment.yaml
                                git commit -m "updated image tag to current $BUILD_NUMBER"

                                git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/$GIT_USERNAME/$GIT_REPO_NAME HEAD:master
                            """
                        }
                    //}
                }
            }
        }
    }
}