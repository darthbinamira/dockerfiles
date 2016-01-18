FROM jekyll/jekyll:stable
MAINTAINER Darth Binamira

# Add community repositories and install OpenJDK 8 JRE + utilities + our fave text editor VIM
RUN \
	echo "" >> /etc/apk/repositories \
	&& echo "http://dl-1.alpinelinux.org/alpine/v3.3/community" >> /etc/apk/repositories \
	&& echo "http://dl-2.alpinelinux.org/alpine/v3.3/community" >> /etc/apk/repositories \
	&& apk --update add curl wget ca-certificates openjdk8-jre vim

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

