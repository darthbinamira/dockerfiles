FROM jekyll/jekyll:stable
MAINTAINER Darth Binamira

RUN apt-get update && apt-get install -y curl openjdk-7-jre

ENV S3_WEBSITE_VER 2.10.0
RUN gem install s3_website -v $S3_WEBSITE_VER
RUN cd /usr/lib/ruby/gems/2.2.0/gems/s3_website-$S3_WEBSITE_VER/ && curl -L -o s3_website-$S3_WEBSITE_VER.jar https://github.com/laurilehmijoki/s3_website/releases/download/v$S3_WEBSITE_VER/s3_website.jar

