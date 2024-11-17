pipeline {
    agent {
        kubernetes {
            label 'jenkins-agent'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: example
spec:
  containers:
  - name: jnlp
    image: jenkins/inbound-agent
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_AGENT_NAME)']
  - name: build
    image: maven:3.8.5-jdk-11
    command:
    - cat
    tty: true
"""
        }
    }
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/example.git'
            }
        }
        stage('Build') {
            steps {
                container('build') {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Test') {
            steps {
                container('build') {
                    sh 'mvn test'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f kubernetes/deployment.yaml'
            }
        }
    }
}

