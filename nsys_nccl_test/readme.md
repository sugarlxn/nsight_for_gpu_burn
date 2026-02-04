## docker build

```shell
docker build -t nccl-test-nsys:12.8.1-devel-ubuntu22.04-nccl2.29.2-1 .
```


## docker run

```shell
docker run --rm -it \
  --gpus all \
  --network host \
  --ipc host \
  --privileged \
  --device=/dev/infiniband \
  -v /dev/nvidia-caps:/dev/nvidia-caps \
  -v /root/.ssh:/root/.ssh \
  --cap-add SYS_ADMIN \
  --cap-add SYS_PTRACE \
  nccl-test-nsys:12.8.1-devel-ubuntu22.04-nccl2.29.2-1 /bin/bash
```

in the container, run the following command to test nccl:
**without the NVswitch,  NCCL_NVLS_ENABLE=0 **
```shell
mpirun --allow-run-as-root -mca pml ucx --mca osc ucx   -n 8 -x NCCL_NVLS_ENABLE=0 -x NCCL_SHARP_DISABLE=1 -x NCCL_COLLNET_ENABLE=0 -x UCX_TLS=sm,cuda_copy,cuda_ipc -x NCCL_DEBUG=INFO -x NCCL_GDR_READ=1 -x NCCL_IB_HCA=mlx5_0,mlx5_1,mlx5_2,mlx5_3,mlx5_4,mlx5_5,mlx5_6,mlx5_7 -x NCCL_IB_GID_INDEX=0 ./build/all_reduce_perf -b 8 -e 1G -f 2 -g 1
```

kubeflow mpijob
```shell
kubectl apply -f single_node_nsys_nccl_test.yaml
```
