pipeline{
    agent any
    stages{
        stage("Dockerizing the NodeJS app"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
                        sh "docker build -t ilo2003/testing:jenkins-exercise:1.0 ."
                        sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                        sh "docker push ilo2003/testing:jenkins-exercise:1.0"
                    }
                }
            }
        }
        stage ("Test"){
            steps{
                script{
                    echo "Testing ...."
                }
            }
        }
        stage ("Deploy"){
            steps{
                script{
                    echo "Deploying ....."
                }
            }
        }
    }
}