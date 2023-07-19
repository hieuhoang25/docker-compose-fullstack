# Stage 1: Build the Angular app
FROM node:14-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Set the default environment to 'development' if not specified during build
ARG TARGET_ENV=development

# Build the Angular app with the specified environment
RUN ng build --configuration=$TARGET_ENV

# Stage 2: Serve the Angular app using a lightweight web server
FROM nginx:alpine

COPY --from=build /app/dist/angular-app /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
