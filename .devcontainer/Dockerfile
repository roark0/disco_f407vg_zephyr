FROM mcr.microsoft.com/devcontainers/cpp:1-debian-12

# 避免交互式安装提示
ENV DEBIAN_FRONTEND=noninteractive

ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="3.22.2"

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

USER vscode

# Install PlatformIO
RUN wget https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py  -O /tmp/get-platformio.py && \
    python3 /tmp/get-platformio.py

ARG USERNAME=vscode
RUN mkdir -p /home/$USERNAME/.platformio/packages

# COPY 会自动解引用软链接，复制真实内容
COPY framework-zephyr /home/$USERNAME/.platformio/packages/framework-zephyr/

RUN chown -R $USERNAME:$USERNAME /home/$USERNAME/.platformio/packages/framework-zephyr
# 修复权限（确保 vscode 用户拥有它）
# RUN chown -R $USERNAME:$USERNAME /home/$USERNAME/.platformio/packages/framework-zephyr

# [Optional] Uncomment this section to install additional vcpkg ports.
# RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# [Optional] Uncomment this section to install additional packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
