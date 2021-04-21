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

`example` 提供了两个项目的示例，在 trigger 中通过`repository.full_name`进行过滤触发不同的 Pipeline，实际应用可以通过 `cel` 条件触发更多 Pipeline，如`PR`、`TAG`、`RELEASE`等 

### Pipeline

```shell
kubectl apply -f example/grpc-gateway-pipeline.yml
kubectl apply -f example/gmqtt-pipeline.yml
```

### 手动运行

```shell
kubectl apply -f example/grpc-gateway-run.yml
kubectl apply -f example/gmqtt-run.yml
```

### GitHub Trigger

```shell
kubectl apply -f example/trigger.yml
```
