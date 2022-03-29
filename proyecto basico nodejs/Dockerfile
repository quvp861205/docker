FROM node:alpine

WORKDIR /var/www

COPY package.json .

COPY index.js .

RUN npm install

COPY . .

EXPOSE 8080

CMD ["node", "index.js"]