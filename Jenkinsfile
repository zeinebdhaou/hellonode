pipeline {
  agent none
  triggers {
    pollSCM('*/1 * * * *')
  }
  stages {
    //Build container image
    stage('Build') {
      agent {
        kubernetes {
          label 'jenkinsrun'
          defaultContainer 'dind'
          yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: dind
    image: docker:18.05-dind
    securityContext:
      privileged: true
    volumeMounts:
      - name: dind-storage
        mountPath: /var/lib/docker
  volumes:
    - name: dind-storage
      emptyDir: {}
"""
        } // kubernetes
      } // agent
      steps {
        container('dind') {
          script {
              def app = docker.build("app:${env.BUILD_ID}")
          } //script
        } //container
      } //steps
    } //stage(build)
  } //stages
} //pipeline
