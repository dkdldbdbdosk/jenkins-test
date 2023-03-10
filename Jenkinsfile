podTemplate(yaml: '''
              apiVersion: v1
              kind: Pod
              spec:
                volumes:
                - name: docker-socket
                  emptyDir: {}
                containers:
                - name: docker
                  image: docker
                  readinessProbe:
                    exec:
                      command: [sh, -c, "ls -S /var/run/docker.sock"]
                  command:
                  - sleep
                  args:
                  - 99d
                  volumeMounts:
                  - name: docker-socket
                    mountPath: /var/run
                - name: docker-daemon
                  image: docker:dind
                  securityContext:
                    privileged: true
                  volumeMounts:
                  - name: docker-socket
                    mountPath: /var/run
                - name: kubectl
                  image: bitnami/kubectl:1.26.0
                  command: 
                  - sleep
                  args: 
                  - 99d
''') {
  node(POD_LABEL) {
    container('docker') {
            stage('Clone Repository'){
        checkout scm
    }
    stage('Build image') {
        app = docker.build("ihp001/jenkins-nginx-test")
    }
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    }
    container('kubectl') {
      stage('test') {
        kubeconfig(serverUrl:'url',credentialsId:'id') {
          sh 'kubectl version'
        }
      }
    }
  }
}
