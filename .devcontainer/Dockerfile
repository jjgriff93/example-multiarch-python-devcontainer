# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.166.1/containers/python-3/.devcontainer/base.Dockerfile

# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT="3.8"
FROM mcr.microsoft.com/vscode/devcontainers/python:dev-${VARIANT}

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Set up non-root user
COPY .devcontainer/scripts/non-root-user.sh /tmp/
RUN bash /tmp/non-root-user.sh "${USERNAME}" "${USER_UID}" "${USER_GID}"

# Set env for tracking that we're running in a devcontainer
ENV DEVCONTAINER=true

# [Option] Install Node.js
ARG INSTALL_NODE="false"
ARG NODE_VERSION="lts/*"
RUN if [ "${INSTALL_NODE}" = "true" ]; then su $USERNAME -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# Install terraform
ARG TERRAFORM_VERSION="1.0.5"
RUN apt-get update -y && apt-get install -y unzip wget \
    && wget "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_$(dpkg --print-architecture).zip" -O /tmp/terraform.zip \
    && unzip /tmp/terraform.zip -d /usr/local/bin/

# Make sure docker group id matches that on WSL.
ARG DOCKER_GROUP_ID
COPY .devcontainer/scripts/docker-client.sh /tmp/
RUN /tmp/docker-client.sh $USERNAME

# Install Docker
RUN apt-get update && sudo apt-get install -y  apt-transport-https ca-certificates curl gnupg  lsb-release \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
    | tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && apt-get update && apt-get install -y docker-ce docker-ce-cli containerd.io

# Install azure-cli
COPY ./.devcontainer/scripts/azure-cli.sh /tmp/
RUN /tmp/azure-cli.sh

USER $USERNAME
