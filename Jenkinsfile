pipeline {
    agent any

    environment {
        REGISTRY = 'conslegrand312/ci-cd-ocp'
        IMAGE_NAME = 'ci-cd-ocp'
        TAG = 'latest'
        OPENSHIFT_API_SERVER = 'https://api.ocp.heritage.africa:6443'
        OPENSHIFT_PROJECT = 'test'
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/ConsLeGrand/ci-cd-ocp.git'
            }
        }
        stage('Build image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY}", 'registry-credentials-id') {
                        docker.image("${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }
        stage('Deploy to OpenShift') {
            steps {
                script {
                    openshift.withCluster("${OPENSHIFT_API_SERVER}") {
                        openshift.withProject("${OPENSHIFT_PROJECT}") {
                            openshift.apply(readYaml(file: 'deployment.yaml'))
                        }
                    }
                }
            }
        }
    }
}
