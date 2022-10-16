pipeline {
  agent {
    label "slave"
  }
  
  parameters {
    string(name: 'BUILD_NUMBER')
  }

  stages {
    stage("Checkout SCM") {
      steps {
        checkout scm
      }
    }

    stage("CI Stage") {
      steps {
        sh vote_image="703172697697.dkr.ecr.us-east-1.amazonaws.com/vote-app:vote-v$BUILD_NUMBER"
        sh result_image="703172697697.dkr.ecr.us-east-1.amazonaws.com/vote-app:result-v$BUILD_NUMBER"
        sh worker_image="703172697697.dkr.ecr.us-east-1.amazonaws.com/vote-app:worker-v$BUILD_NUMBER"

        sh sudo docker build -t $vote_image ./vote
        sh sudo docker build -t $result_image ./result
        sh sudo docker build -t $worker_image ./worker

        sh sudo aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 703172697697.dkr.ecr.us-east-1.amazonaws.com
        sh sudo docker push $vote_image
        sh sudo docker push $result_image
        sh sudo docker push $worker_image
      }
    }
  }
}
