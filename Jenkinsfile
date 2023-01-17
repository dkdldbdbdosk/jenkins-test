podTemplate(yaml: '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: kubectl
    image: bitnami/kubectl:1.26.0
    command:
      - sleep
    args:
      - infinity
'''
) {
  node(POD_LABEL) {
    container('kubectl') {
      kubeconfig(serverUrl:'https://192.168.35.111:6443',credentialsId:'k8s-test-cluster-config') {
        sh 'kubectl version'
        sh 'ping 8.8.8.8'
      }
    }
  }
}
