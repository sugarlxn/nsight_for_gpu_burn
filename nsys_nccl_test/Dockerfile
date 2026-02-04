FROM ghcr.io/coreweave/nccl-tests:12.8.1-devel-ubuntu22.04-nccl2.29.2-1-2276a5e

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y libglib2.0-0 && \
    wget -O /root/NsightSystems-linux-cli-public-2026.1.1.204-3717666.deb https://developer.nvidia.com/downloads/assets/tools/secure/nsight-systems/2026_1/NsightSystems-linux-cli-public-2026.1.1.204-3717666.deb && \
    apt-get install -y /root/NsightSystems-linux-cli-public-2026.1.1.204-3717666.deb && \
    rm -rf /var/lib/apt/lists/*