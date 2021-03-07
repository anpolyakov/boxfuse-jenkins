FROM tomcat:9.0
RUN apt update -y && apt install tomcat9-admin -y
COPY /var/lib/jenkins/workspace/Build and Deploy/boxfuse-sample-java-war-hello/target/hello-1.0.war /usr/local/tomcat/webapps/
