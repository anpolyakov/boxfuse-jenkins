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
                sh 'sudo rm -rf boxfuse-sample-java-war-hello'
                sh 'git clone https://github.com/anpolyakov/boxfuse-sample-java-war-hello.git'
                dir("boxfuse-sample-java-war-hello") {
                    sh 'sudo mvn package'
                }
            }
        }
        stage('Create docker image') {
            steps {
                sh 'sudo rm -rf boxfuse-jenkins boxfuse-jenkins'
                sh 'git clone https://github.com/anpolyakov/boxfuse-jenkins.git'
                sh 'sudo cp boxfuse-jenkins/Deploy/Dockerfile boxfuse-sample-java-war-hello/target/'
                dir("boxfuse-sample-java-war-hello/target") {
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
        stage('Run Docker Container') {
            steps {
                sshagent(['36ff06eb-9726-4ade-8a51-38e5d065f6e8']) {
                    sh 'ssh -o StrictHostKeyChecking=no jenkins@devops-lesson11-instance2 sudo docker run -d -p 8080:8080 warious/tomcat'
                }
            }
        }
    }
}
