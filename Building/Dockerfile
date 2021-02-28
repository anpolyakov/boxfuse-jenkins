FROM ubuntu:20.04

# Install required packages
RUN apt update; apt install openssh-server default-jdk maven -y
# Create jenkins user to the image
    useradd jenkins && echo password | --stdin jenkins

# Expose SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
