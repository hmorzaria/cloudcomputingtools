FROM ubuntu:20.04
MAINTAINER hemnalini.morzarialuna@noaa.gov

ENV TZ America/Los_Angeles
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt update
RUN apt install -y tzdata

# Install minimum requirements

RUN apt-get update -qq && apt-get -y --no-install-recommends install \
    autoconf \
    automake \
    build-essential \
    dos2unix \
    flip \
    libnetcdf-dev \
    libxml2-dev \
    make \
    nano \
    pkg-config \
    subversion \
    unzip \
    valgrind \
    wget

RUN svn co http://svn.osgeo.org/metacrs/proj/branches/4.8/proj/ --non-interactive --trust-server-cert-failures unknown-ca,cn-mismatch,expired,not-yet-valid,other

RUN cd proj/nad
RUN wget http://download.osgeo.org/proj/proj-datumgrid-1.5.zip

RUN unzip -o -q proj-datumgrid-1.5.zip

#make distclean

RUN cd proj/ && ./configure  &&  make  &&  make install && ldconfig

#Install AzCopy, ver. 7.2 includes .NET Core dependencies; they do not
#need to install them a pre-requisite
#https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-linux
#to uninstall azcopy use https://github.com/MicrosoftDocs/azure-docs/issues/18771

RUN wget -O azcopy_linux_amd64_10.20.1.tar https://aka.ms/downloadazcopy-v10-linux --no-check-certificate &&\
tar -xzf azcopy_linux_amd64_10.20.1.tar &&\
cp ./azcopy_linux_amd64_*/azcopy /usr/bin/

#Add Atlantis source files
COPY trunk_6693/.svn /app/.svn
COPY trunk_6693/atlantis /app/atlantis

#Add model files, shouls contain hydrofiles
COPY atlantis_amps_mv_1 /app/model

#source Atlantis
RUN cd /app/atlantis && aclocal && autoheader && autoconf && automake -a && ./configure && make && make install

WORKDIR /app/model

ENTRYPOINT ["sh"]

#this line can be uncommented, it will run Atlantis immediately after the container starts running
#CMD ["amps_cal.sh"]


