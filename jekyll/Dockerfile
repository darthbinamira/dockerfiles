FROM jekyll/jekyll:stable
MAINTAINER Darth Binamira

# Add community repositories and install some utilities + our fave text editor VIM
RUN \
	echo "" >> /etc/apk/repositories \
	&& echo "http://dl-1.alpinelinux.org/alpine/v3.3/community" >> /etc/apk/repositories \
	&& echo "http://dl-2.alpinelinux.org/alpine/v3.3/community" >> /etc/apk/repositories \
	&& apk --update add curl wget ca-certificates tar vim

# Oracle Java installation special thanks to https://developer.atlassian.com/blog/2015/08/minimal-java-docker-containers/
# Glibc-2.21 is required by Oracle JDK 8
RUN \
	curl -Ls https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64/glibc-2.21-r2.apk > /tmp/glibc-2.21-r2.apk \
	&& apk add --allow-untrusted /tmp/glibc-2.21-r2.apk
# Java version
ENV JAVA_VERSION_MAJOR 8
ENV JAVA_VERSION_MINOR 73
ENV JAVA_VERSION_BUILD 02
ENV JAVA_PACKAGE jdk
# Download and extract
RUN mkdir /opt && curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie"\
  http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
    | tar -xzf - -C /opt &&\
    ln -s /opt/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} /opt/jdk &&\
    rm -rf /opt/jdk/*src.zip \
           /opt/jdk/lib/missioncontrol \
           /opt/jdk/lib/visualvm \
           /opt/jdk/lib/*javafx* \
           /opt/jdk/jre/lib/plugin.jar \
           /opt/jdk/jre/lib/ext/jfxrt.jar \
           /opt/jdk/jre/bin/javaws \
           /opt/jdk/jre/lib/javaws.jar \
           /opt/jdk/jre/lib/desktop \
           /opt/jdk/jre/plugin \
           /opt/jdk/jre/lib/deploy* \
           /opt/jdk/jre/lib/*javafx* \
           /opt/jdk/jre/lib/*jfx* \
           /opt/jdk/jre/lib/amd64/libdecora_sse.so \
           /opt/jdk/jre/lib/amd64/libprism_*.so \
           /opt/jdk/jre/lib/amd64/libfxplugins.so \
           /opt/jdk/jre/lib/amd64/libglass.so \
           /opt/jdk/jre/lib/amd64/libgstreamer-lite.so \
           /opt/jdk/jre/lib/amd64/libjavafx*.so \
           /opt/jdk/jre/lib/amd64/libjfx*.so
# Set environment
ENV JAVA_HOME /opt/jdk
ENV PATH ${PATH}:${JAVA_HOME}/bin

# Since we want to use pagination
RUN gem install jekyll-paginate

# Handy dandy press
ENV OCTOPRESS_VER 3.0.11
RUN gem install octopress -v $OCTOPRESS_VER

# And a pretty slick tool for pushing static sites to AWS S3
ENV S3_WEBSITE_VER 2.12.2
ENV S3_WEBSITE_JAR https://github.com/laurilehmijoki/s3_website/releases/download/v$S3_WEBSITE_VER/s3_website.jar
RUN \
	gem install s3_website -v $S3_WEBSITE_VER \
	&& cd /usr/lib/ruby/gems/2.2.0/gems/s3_website-$S3_WEBSITE_VER/ \
	&& curl -L -o s3_website-$S3_WEBSITE_VER.jar $S3_WEBSITE_JAR

# Last but not the least, set the localtime and timezone
ENV TIMEZONE Asia/Singapore
RUN apk --update add tzdata \
	&& cp /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
	&& echo $TIMEZONE > /etc/timezone

