FROM wmoore28/rpi-epics

# Added to dialout for access to devices such as /dev/ttyUSB0
RUN useradd -ms /bin/bash epics \
    && usermod -a -G dialout epics
RUN apt-get update \
    && apt-get -y install vim \
    && apt-get clean

USER epics
WORKDIR /home/epics

RUN cat /usr/local/epics/.epicsrc >> ~/.bashrc

RUN git clone https://github.com/wmoore28/hallc-rpi-epics apps \
    && cd apps \
    && make
