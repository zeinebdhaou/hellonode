pipeline {
    agent {
        kubernetes {
            yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          volumes:
            - name: build-cache
              persistentVolumeClaim: 
                claimName: build-cache
          serviceAccountName: jenkins
          containers:
         - name: docker
            image: myreg/docker:1
            volumeMounts:
            - name: build-cache
              mountPath: /var/lib/docker
              subPath: docker
            command:
            - cat
            tty: true
            securityContext:
              privileged: true
       '''
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
