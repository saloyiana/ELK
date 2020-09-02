pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
  name: elk
spec:
  containers:
  - name: dnd
    image: docker:latest
    command: 
    - cat
    tty: true
    volumeMounts: 
    - mountPath: /var/run/docker.sock
      name: docker-sock
  - name: kubectl
    image: bryandollery/terraform-packer-aws-alpine
    command:
    - cat
    tty: true
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock  
      type: Socket
"""
    }
  }
  environment {
    TOKEN=credentials('5d7560ab-096d-44cb-bcc5-34c620a38a51')
}
  stages {
      stage("Deploy") {
          steps {
              container('kubectl') {
                  sh '''
                    #kubectl  --token=$TOKEN create rolebinding elk --clusterrole=admin --serviceaccount=jenkins:default --namespace=elk
                    #kubectl  --token=$TOKEN create clusterrolebinding elk --clusterrole cluster-admin --serviceaccount=jenkins:jenkins -n elk
                    . elk.sh  
                    kubectl --token=$TOKEN -n elk get all
                     
                  '''
              }
          }
      }
  }
}
