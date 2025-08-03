# Stage 1: Build the Angular application
FROM node:latest as build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve the application with Nginx
FROM nginx:latest
COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build /usr/src/app/dist/first-angular-app /usr/share/nginx/html

# Install OpenSSH server
RUN apt-get update && apt-get install -y openssh-server

# Create user and set password for user and root user
RUN  useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 ubuntu && \
    echo 'ubuntu:secret_password' | chpasswd && \
    echo 'root:secret_password' | chpasswd
	
# Create a directory for SSH, if it doesn't exist
RUN mkdir -p /var/run/sshd

# Expose the SSH port
EXPOSE 22

# Start the SSH server when the container runs
# This example uses a simple startup script; consider a more robust entrypoint script for production
CMD ["/usr/sbin/sshd", "-D"]