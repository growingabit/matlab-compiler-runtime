FROM gcr.io/google-appengine/debian

# nel caso servissero pacchetti non-free/contrib
# RUN sudo sed -i 's/main/main contrib non-free/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y locales-all
RUN apt-get install -y locales

# https://serverfault.com/questions/362903/how-do-you-set-a-locale-non-interactively-on-debian-ubuntu
ENV LC_ALL="en_US.UTF-8"
ENV LANG="en_US.UTF-8"
ENV LANGUAGE="en_US.UTF-8"

RUN locale-gen --purge en_US.UTF-8
RUN echo -e 'LANG="en_US.UTF-8"\nLANGUAGE="en_US:en"\n' > /etc/default/locale

RUN apt-get install -y wget
RUN apt-get install -y unzip
RUN apt-get install -y git
RUN apt-get install -y libxt-dev
RUN apt-get install -y mercurial
RUN apt-get install -y zip
# potrebbe non servire
# RUN apt-get install -y libc++-dev
WORKDIR /opt

RUN wget https://storage.googleapis.com/growingabit-io-backend/jdk-7u80-linux-x64.tar.gz
RUN tar -xzf jdk-7u80-linux-x64.tar.gz
ENV JAVA_HOME="/opt/jdk1.7.0_80"

ENV PATH="$JAVA_HOME/bin:$PATH"

RUN echo $PATH

RUN java -version

RUN wget https://storage.googleapis.com/growingabit-io-backend/apache-maven-3.3.9-bin.zip
RUN unzip apache-maven-3.3.9-bin.zip
ENV M2_HOME="/opt/apache-maven-3.3.9"

ENV PATH="$M2_HOME/bin:$PATH"

RUN echo $PATH

RUN mvn -v

RUN mkdir mcr_installer
RUN mkdir mcr

WORKDIR /opt/mcr_installer

RUN wget https://storage.googleapis.com/growingabit-io-backend/MCR_R2017a_glnxa64_installer.zip && \
    unzip MCR_R2017a_glnxa64_installer.zip && \
    ./install -destinationFolder /opt/mcr -agreeToLicense yes -mode silent && \
    cd /opt && \
    rm -R /opt/mcr_installer

ENV MCR_ROOT="/opt/mcr"
RUN echo $MCR_ROOT
ENV LD_LIBRARY_PATH="${MCR_ROOT}/v92/runtime/glnxa64:${MCR_ROOT}/v92/bin/glnxa64:${MCR_ROOT}/v92/sys/os/glnxa64"
RUN echo $LD_LIBRARY_PATH
ENV XAPPLRESDIR="${MCR_ROOT}/v92/X11/app-defaults"
RUN echo $XAPPLRESDIR

# https://www.mathworks.com/support/bugreports/1297894
# We have done limited testing with version 20 of libstdc++.so.6 !!
# RUN mv ${MCR_ROOT}/v92/sys/os/glnxa64/libstdc++.so.6 ${MCR_ROOT}/v92/sys/os/glnxa64/libstdc++.so.6.old

# https://www.mathworks.com/support/bugreports/1531964
# WORKDIR ${MCR_ROOT}/v92/sys/os/glnxa64
# RUN ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 libstdc++.so.6

WORKDIR /opt

RUN git clone https://github.com/growingabit/jd-core-java

WORKDIR /opt/jd-core-java

RUN ./gradlew assemble

WORKDIR /opt

CMD ["printenv"]