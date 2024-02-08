pipeline {
    agent {
        kubernetes {
            label 'spring-pt-build'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
"""
        }
    }
    stages {

        stage('Build') {
            steps {
                container('maven') {
                        sh "mvn -version"
                        sh "mvn compile"
                }
            }
        }
    }
}

