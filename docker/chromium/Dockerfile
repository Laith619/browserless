ARG VERSION=latest
FROM ghcr.io/browserless/base:$VERSION
LABEL org.opencontainers.image.source https://github.com/browserless/browserless

COPY fonts/* /usr/share/fonts/truetype/
COPY src src/
RUN rm -r src/routes/
COPY src/routes/management src/routes/management/
COPY src/routes/chromium src/routes/chromium/

RUN echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections && \
  apt-get -y -qq install software-properties-common &&\
  apt-add-repository "deb http://archive.canonical.com/ubuntu $(lsb_release -sc) partner" && \
  apt-get -y -qq --no-install-recommends install \
    fontconfig \
    fonts-freefont-ttf \
    fonts-gfs-neohellenic \
    fonts-indic \
    fonts-ipafont-gothic \
    fonts-kacst \
    fonts-liberation \
    fonts-noto-cjk \
    fonts-noto-color-emoji \
    fonts-roboto \
    fonts-thai-tlwg \
    fonts-ubuntu \
    fonts-wqy-zenhei

RUN npx --yes playwright install chromium &&\
  npx --yes playwright install-deps chromium &&\
  npm run build &&\
  npm run build:function &&\
  npm prune production &&\
  npm run install:debugger &&\
  chown -R blessuser:blessuser $APP_DIR &&\
  fc-cache -f -v && \
  apt-get -qq clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/fonts/truetype/noto

USER blessuser

CMD ["./scripts/start.sh"]
