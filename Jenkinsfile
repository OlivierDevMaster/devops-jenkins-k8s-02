pipeline {
    agent {
        kubernetes {
            // this label will be the prefix of the generated pod's name
            label 'jenkins-agent-my-app'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: ci
spec:
  containers:
  - name: python
    image: python:3.11
    command:
    - cat
    tty: true
  - name: docker
    image: docker
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Test python') {
            steps {
                container('python') {
                    sh "pip install -r requirements.txt"
                    sh "python test.py"
                }
            }
        }

        stage('Build image') {
            steps {
                container('docker') {
                    sh "docker build -t 192.168.1.137:4000/pythontest:latest ."
                    sh "docker push 192.168.1.137:4000/pythontest:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                container('docker') {
                    sh "kubectl apply -f kubernetes/deployment.yaml"
                    sh "kubectl apply -f kubernetes/service.yaml"
                }
            }
        }
    }
}
