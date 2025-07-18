FROM golang:1.21-alpine

RUN apk add --no-cache \
    # Core utilities
    ca-certificates \
    tzdata \
    curl \
    wget \
    jq \
    yq \
    # Network debugging
    netcat-openbsd \
    nmap \
    # DNS debugging
    bind-tools \
    iputils \
    # File system tools
    findutils \
    grep \
    sed \
    gawk \
    # Security and debugging
    openssl \
    # Kubernetes tools
    kubectl \
    # AWS tools
    aws-cli \
    # Shell improvements
    bash \
    # Development tools
    vim \
    git \
    # Clean up
    && rm -rf /var/cache/apk/*

RUN wget -O /usr/local/bin/kubectx https://github.com/ahmetb/kubectx/releases/latest/download/kubectx && \
    wget -O /usr/local/bin/kubens https://github.com/ahmetb/kubectx/releases/latest/download/kubens && \
    chmod +x /usr/local/bin/kubectx /usr/local/bin/kubens

RUN addgroup -g 1000 appgroup && \
    adduser -D -s /bin/bash -u 1000 -G appgroup appuser

WORKDIR /app

RUN mkdir -p /home/appuser/.kube && \
    mkdir -p /home/appuser/.aws && \
    mkdir -p /home/appuser/.config && \
    chown -R appuser:appgroup /home/appuser

ENV PATH="/home/appuser/go/bin:${PATH}"
ENV KUBECONFIG="/home/appuser/.kube/config"
ENV AWS_CONFIG_FILE="/home/appuser/.aws/config"

USER appuser

CMD ["/bin/bash"]
