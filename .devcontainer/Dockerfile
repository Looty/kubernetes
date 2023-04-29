ARG VARIANT=jammy
FROM mcr.microsoft.com/devcontainers/base:${VARIANT}

# Install kubectl
RUN curl -sSL -o /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
    && chmod +x /usr/local/bin/kubectl

# Install Helm
RUN curl -s https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash -

# Install Datree
RUN curl https://get.datree.io | /bin/bash

# Install kubeseal
RUN wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.20.5/kubeseal-0.20.5-linux-amd64.tar.gz && \
    tar -xvzf kubeseal-0.20.5-linux-amd64.tar.gz kubeseal && \
    sudo install -m 755 kubeseal /usr/local/bin/kubeseal