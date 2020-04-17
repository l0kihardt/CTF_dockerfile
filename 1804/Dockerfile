#----------------------------------------------------------------#
# Dockerfile to build a container for binary reverse engineering #
# and exploitation. Suitable for CTFs.                           #
#                                                                #
# See https://github.com/superkojiman/pwnbox for details.        #
#                                                                #
# To build: docker build -t superkojiman/pwnbox                  #
#----------------------------------------------------------------#

FROM ubuntu:18.04
MAINTAINER superkojiman@techorganic.com

ENV DEBIAN_FRONTEND noninteractive

RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list

RUN dpkg --add-architecture i386
RUN apt-get update && apt-get -y upgrade

#-------------------------------------#
# Install packages from Ubuntu repos  #
#-------------------------------------#
RUN apt-get install -y \
    sudo \
    apt-utils \
    locales \
    build-essential \
    gcc-multilib \
    g++-multilib \
    gdb \
    gdb-multiarch \
    python3-dev \
    python3-pip \
    ipython \
    default-jdk \
    net-tools \
    nasm \
    cmake \
    rubygems \
    ruby-dev \
    vim \
    tmux \
    git \
    binwalk \
    strace \
    ltrace \
    autoconf \
    socat \
    netcat \
    nmap \
    wget \
    tcpdump \
    exiftool \
    squashfs-tools \
    unzip \
    upx-ucl \
    man-db \
    manpages-dev \
    libtool-bin \
    bison \
    gperf \
    libseccomp-dev \
    libini-config-dev \
    libssl-dev \
    libffi-dev \
    libc6-dbg \
    libglib2.0-dev \
    libc6:i386 \
    libc6-dbg:i386 \
    libncurses5:i386 \
    libstdc++6:i386 \
    libc6-dev-i386

RUN apt-get -y autoremove
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# update locale to include en_US.UTF-8
RUN locale-gen en_US.UTF-8
RUN update-locale

#-------------------------------------#
# Install stuff from pip repos        #
#-------------------------------------#
RUN pip3 install pycipher -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
RUN pip3 install uncompyle6 -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
RUN pip3 install ropper -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
RUN pip3 install pwntools -i http://pypi.douban.com/simple --trusted-host pypi.douban.com

#-------------------------------------#
# Install stuff from GitHub repos     #
#-------------------------------------#
# install gef
RUN git clone https://github.com/hugsy/gef.git /opt/gef
RUN echo source /opt/gef/gef.py >> /root/.gdbinit

# install libseccomp
RUN git clone https://github.com/seccomp/libseccomp.git /opt/libseccomp && \
    cd /opt/libseccomp && \
    ./autogen.sh && ./configure && make && make install 

# install one_gadget
RUN gem install one_gadget

# install seccomp-tools
RUN gem install seccomp-tools


#install untimate vim
RUN git clone --depth=1 https://github.com/amix/vimrc.git /root/.vim_runtime
RUN sh /root/.vim_runtime/install_awesome_vimrc.sh

#setup vim
RUN cd /root/.vim_runtime/sources_non_forked &&\
	git clone https://github.com/vim-scripts/AutoComplPop &&\
	rm /root/.vim_runtime/sources_non_forked/vim-snippets/snippets/python.snippets

COPY python.snippets /root/.vim_runtime/sources_non_forked/vim-snippets/snippets/python.snippets

#remove not useful plugin
RUN rm -rf /root/.vim_runtime/sources_non_forked/comfortable-motion.vim/

# Fix locales after installing everything
ENV LC_CTYPE=C.UTF-8

ENTRYPOINT ["/bin/bash"]
