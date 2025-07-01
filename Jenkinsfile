pipeline {
  agent any
  parameters {
    string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to build')
  } 
  stages {
    stage('git scm update') {
      steps {
        // git url: 'https://github.com/kshelp/099_django', branch: 'master'
        git url: 'https://github.com/kshelp/099_django', branch: "${params.BRANCH}"
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        docker build --no-cache -t 192.168.127.131:32000/react-app .
        docker push 192.168.127.131:32000/react-app
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl delete -f react-svc.yaml
        kubectl delete -f react-deploy.yaml
        kubectl apply -f react-deploy.yaml
        kubectl apply -f react-svc.yaml
        '''
      }
      
    }
  }
}