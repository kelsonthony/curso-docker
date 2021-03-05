FROM node:latest

WORKDIR /home/developer/dev/curso-docker/node-docker
COPY package*.json ./
ENV PORT 3000

RUN npm cache clear --force && npm install

ENTRYPOINT ["npm", "start"]