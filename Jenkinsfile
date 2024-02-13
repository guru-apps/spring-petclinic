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
    image: maven:3-openjdk-17-slim
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
                    withCredentials([usernamePassword(credentialsId: 'nexus-admin-cred', passwordVariable: 'userPassword', usernameVariable: 'userName')]) {
                        sh "mvn -version"
                        sh "mvn -Duser=$userName -Dpassword=$userPassword -Dnexusdns=${env.NEXUSDNS} clean install deploy -Dmaven.test.skip=true -s settings.xml"
                        sh "ls -lart target/spring-petclinic*/WEB-INF/classes/org/springframework/samples"
                    }
                }
            }
        }
    }
}

