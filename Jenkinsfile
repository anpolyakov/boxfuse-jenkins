pipeline {
    agent {
        docker {
            image 'warious/jenkins-agent:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    environment {
        registry = 'warious/tomcat'
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    
    stages {
        stage('Pull git repository') {
            steps {
                sh 'rm -rf boxfuse-sample-java-war-hello'
                sh 'git clone https://github.com/anpolyakov/boxfuse-sample-java-war-hello.git'
                dir("boxfuse-sample-java-war-hello") {
                    sh 'mvn package'
                }
            }
        }
        stage('Create docker image') {
            steps {
                //sh 'sudo docker login --username warious --password ed9b5efc-b8f3-4d30-bc8b-ac826ecef790'
                sh 'rm -rf boxfuse-jenkins boxfuse-jenkins'
                sh 'git clone https://github.com/anpolyakov/boxfuse-jenkins.git'
                dir("boxfuse-jenkins/Deploy") {
                    //sh 'docker build -t warious/tomcat .'
                    script {
                        dockerImage = docker.build registry
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
	                    dockerImage.push()
                    }
                }
            }
        }
    }
}
