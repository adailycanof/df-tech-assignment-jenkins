FROM jenkins/jenkins:lts

# Switch to root to install additional tools
USER root

# Install Docker CLI and other useful tools
RUN apt-get update && apt-get install -y \
    docker.io \
    curl \
    vim \
    git \
    && rm -rf /var/lib/apt/lists/*

# Add jenkins user to docker group
RUN usermod -aG docker jenkins

# Install commonly used plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

# Switch back to jenkins user
USER jenkins

# Skip the initial setup wizard
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# Expose ports
EXPOSE 8080 50000