pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "jx-live"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'helm init --stable-repo-url http://mirror.azure.cn/kubernetes/charts/'
            sh 'unset TILLER_NAMESPACE && jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'unset TILLER_NAMESPACE && jx step helm apply'
          }
        }
      }
    }
  }
}
