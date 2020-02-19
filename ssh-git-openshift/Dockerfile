FROM fedora:latest

RUN dnf -y install openssh-server ed && \
    dnf clean all

RUN sed -i "s/#PasswordAuthentication yes/PasswordAuthentication no/" /etc/ssh/sshd_config && \
    sed -i 's/#UsePrivilegeSeparation.*$/UsePrivilegeSeparation no/' /etc/ssh/sshd_config

RUN sed -i 's/#Port.*$/Port 2022/' /etc/ssh/sshd_config && \
    chmod 775 /var/run && \
    rm -f /var/run/nologin

RUN adduser --system -s /bin/bash -u 1234321 -g 0 git  && \
    chmod 775 /etc/ssh /home && \
    chmod 660 /etc/ssh/sshd_config && \
    chmod 664 /etc/passwd /etc/group

EXPOSE 2022
LABEL Description="IceMobile Famous SFTP server" Vendor="IceMobile" Version="0.0.0"

USER git
# CMD ["/usr/sbin/sshd", "-D"]
CMD echo -e ",s/1234321/`id -u`/g\\012 w" | ed -s /etc/passwd && \
    mkdir -p /home/git/.ssh && \
    touch /home/git/.ssh/authorized_keys && \
    chmod 700 /home/git/.ssh && \
    chmod 600 /home/git/.ssh/authorized_keys && \
    mkdir /home/git/sample.git && \
    ssh-keygen -A && \
    exec /usr/sbin/sshd -D
