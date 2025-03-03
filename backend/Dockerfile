# Build stage (use node:20 for building the application)
FROM node:20.18.3-alpine3.21 AS build

WORKDIR /opt/backend

# Copy package.json and install dependencies
COPY package.json . 
COPY *.js . 
RUN npm install

# Create user and set permissions (in the build stage, since distroless doesn't support this)
RUN addgroup -S expense && adduser -S expense -G expense && \
    chown -R expense:expense /opt/backend

# Runtime stage (use distroless Node.js 18 image)
FROM gcr.io/distroless/nodejs:18

# Create the expense user in the distroless image to match the build stage user
RUN addgroup -S expense && adduser -S expense -G expense

# Set environment variables
ENV DB_HOST="mysql"

# Copy the built app from the build stage (including the user creation and permissions set in the build stage)
COPY --from=build /opt/backend /opt/backend

# Set the working directory in the distroless image
WORKDIR /opt/backend

# Set the user to run the app as (this step is done in the build stage now)
USER expense

# Expose the application port (modify if necessary)
EXPOSE 8080

# Set the command to run your application
CMD ["node", "index.js"]
