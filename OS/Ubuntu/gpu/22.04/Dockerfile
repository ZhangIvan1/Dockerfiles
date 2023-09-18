FROM nvidia/cuda:11.7.1-cudnn8-devel-ubuntu22.04



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
    && apt install -y sudo vim iputils-ping net-tools build-essential cmake autoconf automake cpp-doc gcc-doc make-doc autoconf-doc linux-doc glibc-doc git-doc screen

# RUN curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \ 
#     sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
#     && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
#     sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
#     sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
#     && \
#     sudo apt-get update \
#     && apt-get install -y nvidia-container-toolkit
#     && apt install zlib1g \ 
#     && wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb \
#     && dpkg -i cuda-keyring_1.1-1_all.deb \
#     && dpkg -i cuda-keyring_1.1-1_all.deb \
#     && apt-get update \
#     && apt-get install cuda nvidia-gds \
#     && apt-get install libcudnn8=${cudnn_version}-1+${cuda_version} \
#     && apt-get install libcudnn8-dev=${cudnn_version}-1+${cuda_version} \
#     && apt-get install libcudnn8-samples=${cudnn_version}-1+${cuda_version} \
#     &&

    ##

COPY /Anaconda3-2023.07-2-Linux-x86_64.sh /root/

RUN bash Anaconda3-2023.07-2-Linux-x86_64.sh -b \ 
    # && wget https://repo.anaconda.com/archive/Anaconda3-2023.07-2-Linux-x86_64.sh \
    && rm Anaconda3-2023.07-2-Linux-x86_64.sh \
    && /root/anaconda3/bin/conda init zsh 
    # && cat ./.zshrc \
    # && 

RUN source ~/.zshrc  \
    && pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple  \
    # && rm ~/.condarc \
    && echo "channels:" > ~/.condarc \
    && echo "  - defaults" >> ~/.condarc \
    && echo "show_channel_urls: true" >> ~/.condarc \
    && echo "ssl_verify: false" >> ~/.condarc \
    && echo "default_channels:" >> ~/.condarc \
    && echo "  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/" >> ~/.condarc \
    && echo "  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/" >> ~/.condarc \
    && echo "  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2" >> ~/.condarc \
    && echo "custom_channels:" >> ~/.condarc \
    && echo "  conda-forge: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && echo "  msys2: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && echo "  bioconda: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && echo "  menpo: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && echo "  pytorch: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc  \
    && echo "  pytorch-lts: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && echo "  simpleitk: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud" >> ~/.condarc \
    && conda install conda-build=3.26.1 -y \
    # && conda config --set show_channel_urls yes \
    # && conda config --set ssl_verify false\
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ \
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ \
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/ \
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/ \
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/ \
    # && conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/ \
    && conda update -n base -c defaults conda -y \
    && conda update -n base -c defaults conda -y 
    # && conda update conda -y  \
    # && conda update anaconda -y \
    # && conda update --all -y

RUN echo "SHELL=/usr/bin/zsh" >> /etc/default/useradd\
    && chsh -s /bin/zsh


ENTRYPOINT [ "sh", "-c", "/usr/sbin/service ssh restart && /bin/zsh" ]
