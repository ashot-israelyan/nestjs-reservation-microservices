FROM node:alpine As development

WORKDIR /usr/src/app

COPY package.json ./
COPY yarn.lock ./
COPY tsconfig.json tsconfig.json
COPY nest-cli.json nest-cli.json

RUN npm install -g yarn --force

RUN yarn install

COPY apps/gateway apps/gateway
COPY libs libs

RUN cd apps/gateway && yarn install

RUN yarn build gateway

FROM node:alpine As production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json ./
COPY yarn.lock ./

RUN npm install -g yarn --force

RUN yarn install

COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/gateway/main"]
