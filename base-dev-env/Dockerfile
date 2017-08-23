# Set the base image
FROM phusion/baseimage:0.9.22
# Dockerfile author / maintainer
MAINTAINER tutysara <tutysara@gmailcom>

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

RUN  cd /var/lib/apt && mv lists lists.old
RUN apt-get update && apt-get install -y  software-properties-common
# install emacs
RUN add-apt-repository ppa:kelleyk/emacs && \
   apt update && \
   apt install -y emacs25
RUN apt install -y curl  \
  git  \
  htop  \
  mosh \
  tmux \
  telnet \
  eog \
  wget \
  bzip2 \
  unzip


RUN apt install locales && locale-gen en_US en_US.UTF-8

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# add user and configure
RUN useradd -m -s /bin/bash tutysara && usermod -aG sudo tutysara
# run everything as this new user
USER tutysara
WORKDIR /home/tutysara

RUN git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
RUN mkdir /tmp/adodefont && \
  cd /tmp/adodefont && \
  wget https://github.com/adobe-fonts/source-code-pro/archive/2.010R-ro/1.030R-it.zip && \
  unzip 1.030R-it.zip &&\
  mkdir -p ~/.fonts &&\
  cp source-code-pro-2.010R-ro-1.030R-it/OTF/*.otf ~/.fonts/ &&\
  fc-cache -f -v
RUN echo 'export LC_ALL="en_US.UTF-8"' > ~/.bashrc

USER root
WORKDIR /home/tutysara
CMD ["/sbin/my_init","--", "setuser", "tutysara", "bash"]
