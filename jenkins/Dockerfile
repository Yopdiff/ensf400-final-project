FROM jenkins/jenkins:lts-jdk11

# Disable the setup wizard
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# Switch to root to install additional packages and update permissions
USER root
RUN apt-get update && \
    apt-get install -y git python3 chromium-driver docker.io && \
    rm -rf /var/lib/apt/lists/* && \
    groupadd -f docker && \
    usermod -aG docker jenkins

# Switch back to the jenkins user
USER jenkins

# Install required Jenkins plugins
RUN jenkins-plugin-cli --plugins "git sidebar-link workflow-aggregator pipeline-stage-view"

# Copy initialization Groovy scripts (e.g., to set up the admin account)
COPY init.groovy.d/ /usr/share/jenkins/ref/init.groovy.d/

# Expose default Jenkins ports
EXPOSE 8080 50000