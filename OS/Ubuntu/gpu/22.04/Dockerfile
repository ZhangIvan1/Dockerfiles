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

RUN curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \ 
    sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
    && \
    sudo apt-get update \
    && apt-get install -y nvidia-container-toolkit\
    ##
    && wget https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh \
    && bash Anaconda3-2020.02-Linux-x86_64.sh -b \ 
    && rm Anaconda3-2020.02-Linux-x86_64.sh \
    && /root/anaconda3/bin/conda init zsh \
    && cat ./.zshrc \
    && source ~/.zshrc \
    && pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple  \
    && conda config --set show_channel_urls yes \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/ \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/ \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/ \
    && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/ \
    && conda update -n base -c defaults conda 
    # && conda update conda \
    # && conda update anaconda \
    # && conda update --all

RUN echo "SHELL=/usr/bin/zsh" >> /etc/default/useradd\
    && chsh -s /bin/zsh


# CMD ["/usr/sbin/service", "ssh", "restart"]
# CMD ["/usr/sbin/sshd", "-D"]
# ENTRYPOINT [ "/usr/sbin/service", "ssh", "restart", "&& /bin/zsh" ]
ENTRYPOINT [ "sh", "-c", "/usr/sbin/service ssh restart && /bin/zsh" ]
