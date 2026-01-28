# nsys_gpu_burn

fork from [gpu-burn](https://github.com/wilicc/gpu-burn)

## Build
docker build -t nsys_gpu_burn:0.1 .

## Run
```shell
docker run -it --rm --gpus all -v $(pwd)/output:/output nsys_gpu_burn:0.1 \
  nsys profile --stats=true --force-overwrite=true -o /output/gpu_burn_profile ./gpu_burn 10
```

指定GPU 设备

```shell
docker run -it --rm --gpus device=0 -v $(pwd)/output:/output nsys_gpu_burn:0.1 \
  nsys profile --stats=true --force-overwrite=true -o /output/gpu_burn_profile ./gpu_burn 10
```

## k8s submit

### 前置要求
- 集群已安装 [NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/getting-started.html) 或 NVIDIA Device Plugin
- 节点已正确标记 GPU 资源

### 部署镜像到集群
```shell
# 方式1: 推送到镜像仓库
docker tag nsys_gpu_burn:0.1 your-registry/nsys_gpu_burn:0.1
docker push your-registry/nsys_gpu_burn:0.1

# 方式2: 或者在每个节点上构建/加载镜像
docker save nsys_gpu_burn:0.1 | ssh node01 docker load
```

### 提交 Job
```shell
# 创建输出目录（在目标节点上）
kubectl apply -f gpu-burn-job.yaml

# 查看 Job 状态
kubectl get jobs
kubectl get pods -l app=nsys-gpu-burn

# 查看日志
kubectl logs -f job/nsys-gpu-burn

# 获取输出文件（从节点的 /data/nsys-output 目录）
kubectl describe job nsys-gpu-burn
```

### 清理
```shell
kubectl delete job nsys-gpu-burn
```

### 自定义配置
编辑 `gpu-burn-job.yaml` 可以修改：
- 运行时长（args 中的 "10"）
- GPU 数量（resources.limits.nvidia.com/gpu）
- 输出路径（volumes.hostPath.path）
- 节点选择器（nodeSelector）

