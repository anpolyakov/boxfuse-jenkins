FROM ubuntu:20.04

# Install required packages
RUN apt update; apt install -y tzdata; dpkg-reconfigure -f noninteractive tzdata && \ 
    apt install -y git default-jdk tzdata maven openssh-server && \
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd && \
# Create jenkins user to the image
RUN useradd jenkins; echo "jenkins:jenkins" | chpasswd

# Expose SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
