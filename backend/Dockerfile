FROM node:20.18.3-alpine3.21 AS build
WORKDIR /opt/backend
COPY package.json .
COPY *.js . 
RUN npm install

# Distroless image 
FROM gcr.io/distroless/nodejs20-debian12
RUN addgroup -S expense && adduser -S expense -G expense && \
    mkdir /opt/backend && \
    chown -R expense:expense /opt/backend
ENV DB_HOST="mysql"
WORKDIR /opt/backend
USER expense
COPY --from=build /opt/backend /opt/backend
CMD ["node", "index.js"]



