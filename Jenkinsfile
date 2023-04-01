    
podTemplate(yaml: '''
kind: Pod
metadata:
  name: kaniko
  namespace: ali
spec:
  containers:
  - name: shell
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: IfNotPresent
    env:
     - name: container
       value: "docker"
    command:
     - /busybox/cat
    tty: true
    volumeMounts:
      - name: docker-config
        mountPath: /kaniko/.docker
  volumes:
    - name: docker-config
      configMap:
        name: docker-config
'''){
  node(POD_LABEL) {
     stage('Build') {
      container('shell') {
        stage('Build a Maven project') {
          sh '/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=hellonode'
        }
      }
    }
}
