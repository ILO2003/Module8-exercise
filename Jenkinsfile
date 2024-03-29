pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage("Incrementing version") {
            steps {
                script {
                    dir("app") {
                        echo "Incrementing version: minor"
                        sh "npm version minor"
                        def packageJson = readJSON file: 'package.json'
                        def version = packageJson.version

                        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                    }
                }
            }
        }
        stage("Testing app") {
            steps {
                script {
                    echo "Testing ...."
                    dir("app") {
                        sh "npm install"
                        sh "npm run test"
                    }
                }
            }
        }
        stage("Dockerizing the NodeJS app and pushing") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh "docker build -t ilo2003/testing:${IMAGE_NAME} ."
                        sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                        sh "docker push ilo2003/testing:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage("Commit version update in Github") {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId:"github-token", keyFileVariable: 'key')]) {
                        withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                            sh 'git config --global user.email "jenkins@example.com"'
                            sh 'git config --global user.name "jenkins"'
                            sh "git remote set-url origin https://@github.com/ILO2003/Module8.git"
                            sh "git remote set-url origin https://${TOKEN}@github.com/ILO2003/Module8.git"
                            sh 'git add .'
                            sh 'git commit -m "CI: version bump"'
                            sh 'git push origin HEAD:jenkins-jobs'
                        }
                    }
                }
            }
        }
    }
}
