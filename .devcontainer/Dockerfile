FROM mcr.microsoft.com/devcontainers/cpp:1-debian-12

# 避免交互式安装提示
ENV DEBIAN_FRONTEND=noninteractive

ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="3.22.2"
ARG USERNAME=vscode

# Optionally install the cmake for vcpkg
COPY ./reinstall-cmake.sh /tmp/

RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
        chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
    fi \
    && rm -f /tmp/reinstall-cmake.sh

# 安装基础开发工具
RUN apt update && apt install -y \
    build-essential \
    cmake \
    gdb \
    git \
    vim \
    wget \
    curl \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Python (如果需要)
 RUN apt update && apt install -y \
     python3 \
     python3-pip \
     python3-dev \
     python3-venv \
     clangd clang-format \
     && rm -rf /var/lib/apt/lists/*

# Install PlatformIO
USER vscode
WORKDIR /home/vscode

# 使用 --chown 确保文件属于 vscode
# COPY --chown=vscode:vscode ./install_platformio.sh .
# RUN chmod +x install_platformio.sh && ./install_platformio.sh && rm install_platformio.sh

RUN wget https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py  -O /tmp/get-platformio.py && \
    python3 /tmp/get-platformio.py

RUN mkdir -p /tmp/disco_f407vg && . ~/.platformio/penv/bin/activate && \
    pio init -d /tmp/disco_f407vg -b disco_f407vg --project-option "framework = zephyr" && \
    pio run -d /tmp/disco_f407vg && rm -rf /tmp/disco_f407vg

# [Optional] Uncomment this section to install additional vcpkg ports.
# RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# [Optional] Uncomment this section to install additional packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
