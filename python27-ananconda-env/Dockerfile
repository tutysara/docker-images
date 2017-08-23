# Set the base image
From tutysara/base-dev-env
# Dockerfile author / maintainer 
MAINTAINER tutysara <tutysara@gmailcom>


RUN wget --quiet https://repo.continuum.io/archive/Anaconda2-4.4.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /home/tutysara/anaconda2 && \
    rm ~/anaconda.sh

ENV PATH /home/tutysara/anaconda2/bin:$PATH
#ENV PYTHONHOME /home/tutysara/anaconda2
