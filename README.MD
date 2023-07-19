### 1. Configuration Frontend
1. Step 1: Create environment files
Create the necessary environment files for different environments in your Angular project. For example, you might have the following files:

src/environments/environment.ts (default development environment)
src/environments/environment.prod.ts (production environment)
src/environments/environment.staging.ts (staging environment)
and so on...
Ensure that each environment file contains the corresponding configuration for its target environment.

2. Step 2: Modify the Dockerfile
Update the Dockerfile to specify the desired environment when building the Angular app. You can use build arguments to pass the desired environment configuration to the ng build command. Here's an updated version of the Dockerfile:

```Dockerfile

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
```
3. Step 3: Build and run the Angular app with Docker Compose
With the updated Dockerfile, you can now use Docker Compose to build and run the Angular app with a specific environment configuration.

Place the updated Dockerfile in the root directory of your Angular project.

Create a Docker Compose file (e.g., docker-compose.yml) in the same root directory as the Dockerfile.

Use the previous Docker Compose configuration from the Angular-specific answer provided earlier.

Modify the docker-compose.yml file to include an args section to pass the desired environment configuration during the build:

```yaml

version: '3.8'

services:
angular-app:
build:
context: .
dockerfile: Dockerfile
args:
- TARGET_ENV=production  # Replace 'production' with the desired environment
ports:
- "80:80"
```
In this example, we set the TARGET_ENV build argument to production, which will cause the Angular app to be built using the environment.prod.ts configuration.