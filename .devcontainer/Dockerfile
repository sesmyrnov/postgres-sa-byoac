# Base Dev Container Image
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# Install core utilities
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y --no-install-recommends \
    curl \
    git \
    wget \
    gnupg2 \
    lsb-release \
    software-properties-common \
    apt-transport-https \
    ca-certificates \
    unzip \    
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Microsoft repo and Powershell
RUN wget -q https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    apt-get update && \
    apt-get install -y powershell && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install python3.11 and pip
RUN add-apt-repository ppa:deadsnakes/ppa && apt-get update && \
    apt-get install -y python3.11 python3.11-venv python3.11-dev python3-pip && \
    if [ "$(readlink /usr/bin/python3)" != "python3.11" ]; then \
      ln -sf /usr/bin/python3.11 /usr/bin/python3; \
    fi && \
    if [ "$(readlink /usr/bin/pip)" != "pip3" ]; then \
      ln -sf /usr/bin/pip3 /usr/bin/pip; \
    fi

# Install Node.js (LTS version)
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Install Azure Developer CLI (azd)
RUN curl -fsSL https://aka.ms/install-azd.sh | bash

# Install PowerShell 7 (already installed above)

# Install Azure ML extension
RUN az extension add -n ml

# Set default shell to bash
SHELL ["/bin/bash", "-c"]

# Create docker group and add vscode user to it
RUN if ! getent group docker; then groupadd docker; fi && usermod -aG docker vscode

# Use vscode user
USER vscode