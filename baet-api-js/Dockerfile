#FROM node:10-alpine
FROM bitnami/node:10
LABEL maintainer="Yilmaz Guleryuz 397125+zeusbaba@users.noreply.github.com"

ENV APP_HOME=/usr/src/app

# optionally if you want to run npm global bin without specifying path
ENV NPM_CONFIG_PREFIX=$APP_HOME/.npm-global
ENV PATH=$PATH:$APP_HOME/.npm-global/bin

RUN mkdir -p $APP_HOME
#RUN chown -R node.node APP_HOME
WORKDIR $APP_HOME

ENV ENV_IMPORTER=import-env-vars.sh
COPY $ENV_IMPORTER .
RUN chmod +x ./$ENV_IMPORTER

# use a unified script to run the app
ENV DOCKER_STARTER=docker-starter.sh
COPY $DOCKER_STARTER .
RUN chmod +x ./$DOCKER_STARTER

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm config set strict-ssl false
RUN npm install
# If you are building your code for production
# RUN npm install --only=production

## used to check if DB is up
ADD https://github.com/ufoscout/docker-compose-wait/releases/download/2.4.0/wait ./wait
RUN chmod +x ./wait

# Bundle app source
#COPY . .
COPY src ./src
COPY config ./config
COPY public ./public
COPY .* ./

EXPOSE 4042

ENTRYPOINT exec ./${DOCKER_STARTER}
## CMD /wait && npm start

