ARG CUDA_VERSION=11.8.0
ARG IMAGE_DISTRO=ubi8

FROM nvidia/cuda:${CUDA_VERSION}-devel-${IMAGE_DISTRO} AS builder

WORKDIR /build

COPY . /build/

RUN make

FROM nvidia/cuda:${CUDA_VERSION}-runtime-${IMAGE_DISTRO}

COPY --from=builder /build/gpu_burn /app/
COPY --from=builder /build/compare.ptx /app/

# Install Nsight Systems for profiling on Red Hat UBI8
RUN rpm --import https://developer.download.nvidia.com/devtools/repos/rhel8/x86_64/nvidia.pub && \
    dnf install -y 'dnf-command(config-manager)' && \
    dnf config-manager --add-repo "https://developer.download.nvidia.com/devtools/repos/rhel8/$(uname -m)/" && \
    dnf install -y nsight-systems-cli && \
    dnf clean all && \
    rm -rf /var/cache/dnf

WORKDIR /app

CMD ["./gpu_burn", "60"]
