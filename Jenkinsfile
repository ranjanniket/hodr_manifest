pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        GIT_REPO_NAME = "hodr_manifest"
        GIT_USER_NAME = "ranjanniket"
        NEW_BUILD_NUMBER = "${BUILD_NUMBER}"
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/ranjanniket/hodr_manifest.git'
            }
        }
        stage('Update Deployment File') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([string(credentialsId: 'ranjanniket', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh '''
                        git config user.email "niketranjn50@gmail.com"
                        git config user.name "Niket Ranjan"
                        sed -i "s/niket50\\/hodr:.*/niket50\\/hodr:${env.BUILD_NUMBER}/" Kubernetes/hodr.yaml
                        git add .
                        sh "git commit -m 'By Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/hodr_manifest.git HEAD:main" 
                    '''
                }
            }
            }
        }
        
       stage('Update GIT') {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        def gitUrl = "git@github.com:ranjanniket/hodr_manifest.git"
                        sshagent(credentials: ['niket-github']) {
                            sh "git config user.email 'niketranjn50@gmail.com'"
                            sh "git config user.name 'ranjanniket'"

                            sh "cat Kubernetes/hodr.yaml"
                            sh "sed -i 's/niket50\\/hodr:.*/niket50\\/hodr:${env.BUILD_NUMBER}/' Kubernetes/hodr.yaml"
                            sh "cat Kubernetes/hodr.yaml"
                            sh "git add ."
                            sh "git commit -m 'By Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push ${gitUrl} HEAD:main"
                        }
                    }
                }
            }


        // stage('Update Deployment File') {
        //     steps {
        //         withCredentials([string(credentialsId: 'ranjanniket', variable: 'GITHUB_TOKEN')]) {
        //             sh '''
        //                 git config user.email "niketranjn50@gmail.com"
        //                 git config user.name "Niket Ranjan"
        //                 sed -i "s/niket50\\/hodr:.*/niket50\\/hodr:${NEW_BUILD_NUMBER}/" Kubernetes/hodr.yaml
        //                 git add .
        //                 git commit -m "Update image tag to ${NEW_BUILD_NUMBER}"
        //                 git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
        //             '''
        //         }
        //     }
        // }
    }
}



