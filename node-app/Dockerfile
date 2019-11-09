FROM node:alpine
# Create app directory
WORKDIR /usr/src/app
# Install app dependencies
# Wildcard is used to ensure that both package.json and package-lock.json are copied
# Only for npm 5+
COPY src/package*.json ./
RUN npm install
# Copy the source code to the container workdir
COPY src/ .
# Node.js app listens on port 8080 so let's expose it
EXPOSE 8080
CMD [ "node", "main.js" ]


