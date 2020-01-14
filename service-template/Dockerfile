FROM node:12-alpine

WORKDIR /opt/actano/rplan

COPY yarn.lock yarn.lock
COPY package.json package.json
RUN yarn install --frozen-lockfile
COPY babel.config.js babel.config.js
COPY src src
RUN yarn clean && yarn build
COPY .rplan-config.yml .rplan-config.yml

EXPOSE 3000

# We can't use yarn here because of https://github.com/yarnpkg/yarn/issues/4667
ENTRYPOINT ["node", "lib/server.js"]
