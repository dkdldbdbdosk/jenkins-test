pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
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
          securityContext: 
            runAsUser: 0
        '''
    }
  }
  stages {
      stage('Build-Docker-Image') {
      steps {
        container('docker') {
          sh 'docker build -t ihp001/jenkins-test-nginx:1.0 ./'
        }
      }
    }
  }
}
