# Tekton Practice

## Install

```shell script
kubectl apply -f install
```

## Task

```shell script
kubectl apply -f task
```

## Config

```shell script
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

```shell script
kubectl apply -f example/grpc-gateway-pipeline.yml
```

### 手动运行

```shell script
kubectl apply -f example/grpc-gateway-run.yml
```

### GitHub Trigger

```
kubectl apply -f example/grpc-gateway-trigger.yml
```
