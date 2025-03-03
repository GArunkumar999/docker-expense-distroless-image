# Build stage (use node:20 for building the application)
FROM node:20.18.3-alpine3.21 AS build

WORKDIR /opt/backend

# Copy package.json and install dependencies
COPY package.json . 
COPY *.js . 
RUN npm install

# Runtime stage (use distroless Node.js 18 image)
FROM node:20.18.3-alpine3.21
# Set environment variables
ENV DB_HOST="mysql"

# Set the working directory in the distroless image
WORKDIR /opt/backend

# Copy the built app from the build stage (including the user creation and permissions set in the build stage)
COPY --from=build /opt/backend /opt/backend



# Expose the application port (modify if necessary)
EXPOSE 8080

# Set the command to run your application
CMD ["node", "index.js"]
