FROM node:latest
MAINTAINER Daniel Dent (https://www.danieldent.com/)

ENV METEOR_VERSION latest
ENV METEOR_INSTALLER_SHA256 4020ef4d3bc257cd570b5b2d49e3490699c52d0fd98453e29b7addfbdfba9c80
ENV METEOR_LINUX_X86_32_SHA256 latest
ENV METEOR_LINUX_X86_64_SHA256 latest

# 1. Download & verify the meteor installer.
# 2. Patch it to validate the meteor tarball's checksums.
# 3. Install meteor

COPY meteor-installer.patch /tmp/meteor/meteor-installer.patch
COPY vboxsf-shim.sh /usr/local/bin/vboxsf-shim
RUN curl -SL https://install.meteor.com/ -o /tmp/meteor/inst \
    && sed -e "s/^RELEASE=.*/RELEASE=\"\$METEOR_VERSION\"/" /tmp/meteor/inst > /tmp/meteor/inst-canonical \
    && echo $METEOR_INSTALLER_SHA256 /tmp/meteor/inst-canonical | sha256sum -c \
    && patch /tmp/meteor/inst /tmp/meteor/meteor-installer.patch \
    && chmod +x /tmp/meteor/inst \
    && /tmp/meteor/inst \
    && rm -rf /tmp/meteor

# 4. Install demeteorizer (using my fork of demeteorizer until Dockerfile support is merged upstream)

RUN npm install -g DanielDent/demeteorizer#v2.1.0 \
    && npm cache clear

VOLUME /app
WORKDIR /app
EXPOSE 3000
CMD [ "meteor create muzo" ]
WORKDIR /muzo
CMD [ "meteor" ]
