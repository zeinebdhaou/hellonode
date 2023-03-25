pipeline {
  agent {
    kubernetes {
      label 'hellonode'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: maven
    image: maven:latest
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: "/root/.m2"
        name: m2
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: m2
      persistentVolumeClaim:
        claimName: m2
"""
}
   }
  stages {
   stage ('Clone repository'){
    steps {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }
  }
  stage('Build imag') {
    steps {
        container('docker') {
          sh """
             docker build -t hellonode .
          """
           echo 'Build Image Completed' 
        }
      }
    }
  }
}
