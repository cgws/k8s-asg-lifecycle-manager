@Library('ciinabox@docker_build') _
pipeline {
  environment {
    AWS_REGION = 'ap-southeast-2'
    ECR_REPO = '362995399210.dkr.ecr.ap-southeast-2.amazonaws.com'
    OPS_ACCOUNT_ID = '362995399210'
  }
  agent none
  stages {
    stage('Build helm package') {
      agent {
        docker {
          image '362995399210.dkr.ecr.ap-southeast-2.amazonaws.com/catch/helm-builder:latest'
        }
      }
      steps {
        script {
          withCredentials([
            [
              $class: 'UsernamePasswordMultiBinding',
              credentialsId: 'catch-github-token',
              usernameVariable: 'GITHUB_USER',
              passwordVariable: 'GITHUB_TOKEN'
            ],
            [
              $class: 'AmazonWebServicesCredentialsBinding',
              credentialsId: "tf-ops",
            ]
          ]) {
            sh '''
               cd helm
               helm package asg-lifecycle-manager
               helm s3 push --force asg-lifecycle-manager*.tgz cgws-helm-stable
               '''
          }
        }
      }
    }
  }
}
