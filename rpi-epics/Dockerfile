FROM arm32v7/debian:jessie

ENV EPICS_BASE=/usr/local/epics/base \
    PATH=$PATH:/usr/local/epics/base/bin/linux-arm:/usr/local/epics/extensions/bin/linux-arm

RUN apt-get update \
    && apt-get install -y \
	autoconf \
	curl \
	gcc \
	git \
	g++ \
	ipython \
	libtool \
	make \
	perl \
	python-setuptools \
	re2c \
    && apt-get clean	

RUN cd /usr/local/share/ca-certificates \
    && curl -sLO https://pki.jlab.org/JLabCA.crt \
    && update-ca-certificates

RUN mkdir /usr/local/epics \
    && cd /usr/local/epics \
    && curl -sLO http://www.aps.anl.gov/epics/download/base/baseR3.14.12.6.tar.gz \
    && tar xzf baseR3.14.12.6.tar.gz \
    && rm baseR3.14.12.6.tar.gz \
    && ln -s base-3.14.12.6 base \
    && cd base \
    && make install

# build extensions
RUN cd /usr/local/epics \
    && git clone https://github.com/wmoore28/epics-extensions extensions \
    && find . -name .git -exec rm -rf {} \+ \
    && cd extensions \
    && make

# build synApps
RUN cd /usr/local/epics \
    && curl -sLO https://www3.aps.anl.gov/bcda/synApps/tar/synApps_5_8.tar.gz \
    && tar xf synApps_5_8.tar.gz \
    && ln -s synApps_5_8/support support \
    && rm -rf synApps_5_8.tar.gz
COPY configure/RELEASE /usr/local/epics/support/configure/
RUN cd /usr/local/epics/support \
    && sed -i "s/EPICS_MODULES/#EPICS_MODULES/" devIocStats-*/configure/RELEASE \
    && make release \
    && make

# PyEpics
RUN easy_install -U PyEpics

COPY configure/.epicsrc /usr/local/epics
