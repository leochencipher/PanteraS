FROM ubuntu:14.04.2

MAINTAINER <siteops@team.mobile.de>

RUN apt-get update
RUN dpkg-divert --local --rename --add /sbin/initctl
RUN apt-get update \
    && apt-get -y install software-properties-common wget curl

# Someone can be behind proxy so add-apt-repository might not work
#   && add-apt-repository ppa:webupd8team/java

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" > /etc/apt/sources.list.d/java-trusty.list
RUN gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv EEA14886 && gpg --export --armor EEA14886 | apt-key add -

RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get -y install oracle-java8-installer && apt-get clean
RUN update-alternatives --display java
RUN echo "JAVA_HOME=/usr/lib/jvm/java-8-oracle" >> /etc/environment
RUN echo '# /lib/init/fstab: cleared out for bare-bones lxc' > /lib/init/fstab
RUN ln -sf /proc/self/mounts /etc/mtab

ADD start.sh /usr/local/bin/start.sh
RUN chmod +x /usr/local/bin/start.sh

ENV HOME  /
WORKDIR /

ENTRYPOINT ["/usr/local/bin/start.sh"]
