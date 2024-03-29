pipeline{
    agent any
    stages{
        stage("Dockerizing the NodeJS app"){
            steps{
                script{
                        sh "docker build -t jenkins-exercise:1.0 ."
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
