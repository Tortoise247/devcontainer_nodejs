# Use Ubuntu as the base image
FROM ubuntu:22.04

# Switch to the root user
USER root

# Set the root user's password using an environment variable
ARG ROOT_PASSWORD
RUN echo "root:${ROOT_PASSWORD}" | chpasswd

# Set the DEBIAN_FRONTEND environment variable to noninteractive
ENV DISPLAY=:0
ENV WAYLAND_DISPLAY=wayland-0
ENV XDG_RUNTIME_DIR=/mnt/wslg/runtime-dir/


# Install sudo
RUN apt-get update && \
    apt-get install -y sudo curl git\
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    libgtk-3-0 \
    libnss3 \
    libxss1 \
    libasound2 \
    libxtst6 \
    libx11-xcb1 \
    libxcomposite1 \
    libxrandr2 \
    libxdamage1 \
    libxkbfile1 \
    libdrm2 \
    libgbm-dev \
    && apt-get clean


# Add the developer user to the sudoers file
RUN echo "developer ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Create a new user and group
RUN groupadd -r developer && useradd -r -g developer developer

# Switch to the developer user
RUN mkdir -p /home/developer/.vscode-server/bin && chown -R developer:developer /home/developer
USER developer



# Run an infinite loop to prevent the container from exiting
ENTRYPOINT ["tail", "-f", "/dev/null"]