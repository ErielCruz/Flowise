# Stage 1: Build stage
FROM node:20-alpine as build

USER root

# Skip downloading Chrome for Puppeteer (saves build time)
ENV PUPPETEER_SKIP_DOWNLOAD=true

# Set the working directory inside the container
WORKDIR /app

# Copy the local repository files into the container
COPY . .

# Install the dependencies locally
RUN npm install

# Stage 2: Runtime stage
FROM node:20-alpine

# Install runtime dependencies
RUN apk add --no-cache chromium git python3 py3-pip make g++ build-base cairo-dev pango-dev

# Set the environment variable for Puppeteer to find Chromium
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Copy the built application from the build stage
COPY --from=build /app /app

# Set the working directory
WORKDIR /app

# Ensure the local bin folder is in the PATH
ENV PATH /app/node_modules/.bin:$PATH

ENTRYPOINT ["flowise", "start"]
