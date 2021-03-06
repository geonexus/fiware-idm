FROM ubuntu:14.04

MAINTAINER FIWARE Indentity Manager Team. DIT-UPM

WORKDIR /opt

# fix source
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Install Ubuntu dependencies
RUN sudo apt-get update && \
    sudo apt-get install -y wget python python-dev git && \
    wget https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py

# Install virtualenvwrapper
RUN pip install virtualenvwrapper && \
    export WORKON_HOME=~/venvs && \
    mkdir -p $WORKON_HOME

# Download latest version of the code 
RUN git clone https://github.com/ging/fiware-idm idm && \
    cd idm && \
    cp conf/settings.py.example conf/settings.py

WORKDIR /opt/idm

# Install python dependecies
RUN source /usr/local/bin/virtualenvwrapper.sh && \
    mkvirtualenv idm_tools && \
    pip install -r requirements.txt

# Install Keystone back-end
RUN source /usr/local/bin/virtualenvwrapper.sh && \
    workon idm_tools && \
    fab keystone.install

# Install Horizon front-end
RUN source /usr/local/bin/virtualenvwrapper.sh && \
    workon idm_tools && \
    fab horizon.install


# Run IdM
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod 755 /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]

### Exposed ports
# - Keystone
EXPOSE 5000
# - Horizon (HTTP)
EXPOSE 8000