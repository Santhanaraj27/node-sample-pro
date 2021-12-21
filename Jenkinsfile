pipeline {
    agent any
    stages {
      stage("transaction Docker variables") {
        steps {
        script {
        env.ECRREPOURI = "390911387803.dkr.ecr.us-east-2.amazonaws.com/my-docker-image"
        env.DOCKERPUSHURL = "https://390911387803.dkr.ecr.us-east-2.amazonaws.com/my-docker-image"
        env.TAG = "${env.BRANCH_NAME}" + "${BUILD_NUMBER}"
        echo "Tag : ${env.TAG}"
        env.IMAGE = "${env.ECRREPOURI}" + ":" + "${env.TAG}"
                }
            }
        }
      stage("Docker build") {
                steps {
                    sh """ 
                        pwd
                        cat Dockerfile
                        docker build -t ${env.ECRREPOURI}:${env.TAG} .
                    """
                }
            }
      stage("Docker push to AWS ECR") {
            steps{
                script{
                     sh("eval \$(aws2 ecr get-login --no-include-email)")
                     docker.withRegistry("${env.DOCKERPUSHURL}", "ecr.us-east-2:aws-creds") {
                     docker.image("${env.IMAGE}").push()
                    }
                }
            }
        }
     }
}
