# Tekton Practice

## Install

```shell
kubectl apply -f install/pipeline_v0.20.0.yaml
kubectl apply -f install/trigger_v0.10.2.yaml
kubectl apply -f install/dashboard_v0.15.0.yaml
```

## Task

```shell
kubectl apply -f task
```

## Config

```shell
kubectl apply -f config
```

## Golang 示例

- Clone 代码
- Go test
- Go build
- Docker 镜像
    - Kaniko
    - 没有使用 `docker-build`，用 Kaniko 方便取到 Digest 
- Helm & Kubectl

### Pipeline

```shell
kubectl apply -f example/grpc-gateway-pipeline.yml
```

### 手动运行

```shell
kubectl apply -f example/grpc-gateway-run.yml
```

### GitHub Trigger

```shell
kubectl apply -f example/grpc-gateway-trigger.yml
```
