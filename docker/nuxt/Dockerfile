FROM node:12.13.1-alpine
ARG APP_NAME

ENV LANG=C.UTF-8 \
    TZ=Asia/Tokyo

RUN apk update && \
    apk add git && \
    npm install -g npm && \
    npm install -g vue-cli

RUN mkdir /${APP_NAME}
WORKDIR /${APP_NAME}

ADD repos/${APP_NAME}/package.json package.json
RUN npm install

ENV HOST 0.0.0.0
EXPOSE 3000
