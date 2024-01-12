# Use an official Nginx runtime as a parent image
FROM nginx:latest

# Set the working directory to the default Nginx web root directory
WORKDIR /usr/share/nginx/html

# Copy the current directory contents into the container at the working directory
COPY . .

# Expose port 80 to allow external access to the web application
EXPOSE 80

# Define the default command to run when the container starts
CMD ["nginx", "-g", "daemon off;"]
