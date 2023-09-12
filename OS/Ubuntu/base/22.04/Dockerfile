FROM ubuntu:22.04


ENV DEBIAN_FRONTEND=noninteractive

RUN sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list \
    && apt update\
    && apt upgrade -y\
    && apt install -y zsh openssh-server git wget curl bzip2 \
    && sed -ri 's/#PermitEmptyPasswords no/PermitEmptyPasswords yes/' /etc/ssh/sshd_config \
    && sed -ri 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && /etc/init.d/ssh start

WORKDIR /root

SHELL [ "/bin/zsh", "-c" ]

RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)" -- \
    -t robbyrussell \
    && apt install -y sudo vim net-tools build-essential sudo autoconf automake cpp-doc gcc-doc make-doc autoconf-doc linux-doc glibc-doc git-doc screen


RUN echo "SHELL=/usr/bin/zsh" >> /etc/default/useradd\
    && chsh -s /bin/zsh


# CMD ["/usr/sbin/service", "ssh", "restart"]
# CMD ["/usr/sbin/sshd", "-D"]
# ENTRYPOINT [ "/usr/sbin/service", "ssh", "restart", "&& /bin/zsh" ]
ENTRYPOINT [ "sh", "-c", "/usr/sbin/service ssh restart && /bin/zsh" ]
