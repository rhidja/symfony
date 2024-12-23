ARG NODE_VERSION=18.18.2
FROM node:${NODE_VERSION} AS app_nodejs

# Set custom yarn configuration
COPY docker/nodejs/.yarnrc /.yarnrc
RUN chmod 777 /.yarnrc

ENV BABEL_CACHE_PATH /home/node/.cache/babel

WORKDIR /srv/app
