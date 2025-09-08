# --- STAGE 1: Build Environment ---
# We're renaming the stage to a more unique name to avoid conflicts.
FROM node:20-alpine AS frontend_builder

# Set the working directory inside the container.
WORKDIR /app

# Copy the package.json and package-lock.json files first.
COPY package*.json ./

# Install all the project dependencies.
RUN npm install

# Copy the rest of the application source code.
COPY . .

# Run the build command to create the optimized, static production files.
RUN npm run build

# --- STAGE 2: Production Server ---
# This stage creates the final, lightweight image using Nginx.
FROM nginx:stable-alpine

# Remove the default Nginx index.html file to avoid conflicts.
RUN rm /etc/nginx/conf.d/default.conf

# Copy the custom Nginx configuration.
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# Copy the static build files from the 'frontend_builder' stage.
# We've updated the source name here.
COPY --from=frontend_builder /app/build /usr/share/nginx/html

# Expose port 80 so Nginx can serve the application.
EXPOSE 80

# The CMD command to run the Nginx server in the foreground.
CMD ["nginx", "-g", "daemon off;"]