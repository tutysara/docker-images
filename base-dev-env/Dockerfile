# Set the base image
FROM phusion/baseimage:0.9.22
# Dockerfile author / maintainer
MAINTAINER tutysara <tutysara@gmailcom>

RUN  cd /var/lib/apt && mv lists lists.old

RUN apt-get update && apt-get install -y \
	    bzip2 \
	    ca-certificates \
	    curl \
	    emacs \
	    eog \
	    git \
	    htop \
	    iputils-ping \
	    mosh \
	    net-tools \
	    software-properties-common \
	    sudo \
	    tmux \
	    telnet \
	    vim \
	    wget \
	    unzip	


# install emacs
RUN add-apt-repository ppa:kelleyk/emacs && \
   apt update && \
   apt install -y emacs25

# generate locales
RUN apt install locales && locale-gen en_US en_US.UTF-8

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# add user and configure
RUN useradd -m -s /bin/bash tutysara -p $(openssl passwd -1 changeme) && usermod -aG sudo tutysara 

# sshd config
RUN sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config && \
	sed -i s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication\ no/ /etc/ssh/sshd_config && \
	sed -i s/.*PasswordAuthentication.*/PasswordAuthentication\ no/ /etc/ssh/sshd_config && \
	sed -i s/.*UsePAM.*/UsePAM\ no/ /etc/ssh/sshd_config

# copy rc.local script -- for changing mount permission
COPY rc.local /etc/rc.local
RUN chmod +x /etc/rc.local
# run everything as this new user
USER tutysara
WORKDIR /home/tutysara

# install spacemacs and update fonts
RUN git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
RUN mkdir /tmp/adodefont && \
  cd /tmp/adodefont && \
  wget https://github.com/adobe-fonts/source-code-pro/archive/2.010R-ro/1.030R-it.zip && \
  unzip 1.030R-it.zip &&\
  mkdir -p ~/.fonts &&\
  cp source-code-pro-2.010R-ro-1.030R-it/OTF/*.otf ~/.fonts/ &&\
  fc-cache -f -v
RUN cp /etc/skel/.bashrc ~/.bashrc
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc

USER root
WORKDIR /home/tutysara

# enable ssh
RUN rm -f /etc/service/sshd/down
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Use baseimage-docker's init system.
#CMD ["/sbin/my_init","--", "setuser", "tutysara", "bash"]
