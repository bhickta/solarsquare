# Use the official Frappe bench image as the base image
FROM frappe/bench:latest

# Set environment variables
ENV SHELL=/bin/bash

# Install required dependencies
USER root
RUN apt-get update && \
    apt-get install -y \
        wget \
        tar \
        xz-utils \
        git \
        curl \
        sudo \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Download and extract Neovim from tar file
RUN wget https://github.com/neovim/neovim/releases/download/v0.9.0/nvim-linux64.tar.gz -O /tmp/nvim-linux64.tar.gz && \
    tar xzf /tmp/nvim-linux64.tar.gz -C /opt/ && \
    mv /opt/nvim-linux64 /opt/nvim && \
    ln -s /opt/nvim/bin/nvim /usr/local/bin/nvim && \
    rm /tmp/nvim-linux64.tar.gz

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && \
    apt-get install -y nodejs && \
    npm install -g yarn

# Set up Neovim configuration and plugins
USER frappe
RUN git clone https://github.com/bhickta/nvim.git ~/.config/nvim

# Launch Neovim to trigger Lazy plugin loading
RUN nvim --headless +'autocmd User LazyLoaded quitall' +':PlugInstall --sync' +':qall'

# Set the working directory
WORKDIR /workspace/development/frappe-bench

# Copy the contents of the current directory to /workspace
COPY . /workspace

# Set the command to sleep infinity to keep the container running
CMD ["sleep", "infinity"]

