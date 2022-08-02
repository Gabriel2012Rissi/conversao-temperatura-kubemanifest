pipeline {
    agent any
    stages {
        stage('Update Git') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh "git config user.email ${GIT_EMAIL}"
                        sh "git config user.name ${GIT_NAME}"

                        sh "cat deployment.yaml"
                        sh "sed -i 's+image:.*+image: ${DOCKER_IMAGE}+' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Feito por Jenkins através do Job conversao-temperatura-manifest-update: ${env.BUILD_NUMBER}'"
                        // Para interpolação de variáveis sensíveis deve-se usar aspas simples
                        sh 'git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/$GIT_USERNAME/conversao-temperatura-kubemanifest.git HEAD:main'
                    }
                }
            }
        }
    }
}
