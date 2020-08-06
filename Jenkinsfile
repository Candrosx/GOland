pipeline {
    agent {
        label 'master'
    }
    triggers { pollSCM('* * * * *')}
    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    environment {
        IMAGE       = 'candrosx/goland:tests'
        SELECTOR    = 'demostest'
        OUTERPORT   = 9999
        INNERPORT   = 8888
        K8SNAME     = 'demostest'
    }
    stages {
        stage('Delete old') {
            steps {
                sh("kubectl delete all --selector run=$SELECTOR")
                sh("docker image rm $IMAGE -f")
            }
        }
        stage('Create docker image') {
            steps {
                dir('docker') {
                    sh("docker image build -t $IMAGE .")
                }
            }
        }
        stage('Kubectl apply') {
            steps {
                dir('yaml') {
                    sh("kubectl apply -f deployments.yaml")
                    sh("kubectl apply -f services.yaml")
                    sh("kubectl create -f limitrange.yaml")
                    sh("kubectl apply -f quota.yaml")
                }
            }
        }
    }
}
