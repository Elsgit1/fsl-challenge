FROM node:15.5.1-buster-slim AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . . 

RUN npm run build

FROM nginx:1.27-alpine-slim

WORKDIR /usr/share/nginx/html

COPY --from=builder /app/build ./

EXPOSE 80

ENTRYPOINT ["nginx", "-g", "daemon off;"]