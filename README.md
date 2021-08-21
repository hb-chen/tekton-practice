# Tekton Practice

## Install

```shell
kubectl apply -f install/pipeline_v0.26.0.yaml
kubectl apply -f install/dashboard_v0.18.1.yaml
kubectl apply -f install/trigger_v0.15.0.yaml
kubectl apply -f install/interceptor_v0.15.0.yaml
```

## Task

```shell
kubectl apply -n tekton-pipelines -f task
```

## Config

```shell script
kubectl apply -n tekton-pipelines -f config
```

[Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private
-registry)

> 注意如果 `config.json` 中没有 `auth`，删掉 `credsStore`，重新 `docker login`  

```shell script
kubectl create secret generic dockerconfig \
  -n tekton-pipelines \
  --from-file=config.json=$HOME/.docker/config.json \
  --type=kubernetes.io/dockerconfig
```

```shell script
kubectl create secret generic k8s-kubeconfig \
  -n tekton-pipelines \
  --from-file=$HOME/.kube/config
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
kubectl apply -n tekton-pipelines -f example/grpc-gateway-pipeline.yml
kubectl apply -n tekton-pipelines -f example/gmqtt-pipeline.yml
```

### 手动运行

```shell
kubectl apply -n tekton-pipelines -f example/grpc-gateway-run.yml
kubectl apply -n tekton-pipelines -f example/gmqtt-run.yml
```

### GitHub Trigger

```shell
kubectl apply -n tekton-pipelines -f example/trigger.yml
```
