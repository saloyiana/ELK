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
    TOKEN=credentials('cd33ce09-42b5-481e-b714-15f1c1bd19e1')
}
  stages {
      stage("Deploy") {
          steps {
              container('kubectl') {
                  sh '''
                    cd elk/
                    . elk.sh  
                    kubectl --token=$TOKEN -n elk get all
                     
                  '''
              }
          }
      }
  }
}
