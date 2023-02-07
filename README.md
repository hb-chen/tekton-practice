# Tekton Practice

## Install

```shell
kubectl apply -f install/pipeline_v0.43.2.yaml
kubectl apply -f install/dashboard_v0.31.0.yaml
kubectl apply -f install/trigger_v0.22.1.yaml
kubectl apply -f install/interceptor_v0.22.1.yaml
```

| Date       | Pipeline  | Dashboard | Trigger   | Interceptor |
|------------|-----------|-----------|-----------|-------------|
| 2023-01-15 | `v0.43.2` | `v0.31.0` | `v0.22.1` | `v0.22.1`   |
| 2022-11-13 | `v0.41.0` | `v0.30.0` | `v0.21.0` | `v0.21.0`   |
| 2022-06-18 | `v0.36.0` | `v0.27.0` | `v0.20.0` | `v0.20.0`   |
| 2022-01-08 | `v0.31.0` | `v0.23.0` | `v0.18.0` | `v0.18.0`   |
| 2021-08-23 | `v0.26.0` | `v0.18.1` | `v0.15.0` | `v0.15.0`   |

## Namespace

在 `v0.36.0` 及以前在 `tekton-pipelines` 命名空间管理流水线资源没有问题，但在 `v0.41.0` 开始增加了 `PodSecurity`，直接使用 `tekton-pipelines` 命名空间会导致 Task Pod 无法正常创建，相关问题参考 [TaskRun failures dued to k8s PodSecurity](https://github.com/tektoncd/pipeline/issues/5779)

如果是旧版本升级可通过修改命名空间的 `pod-security.kubernetes.io/enforce` 标签，而如果是新部署建议创建独立的命名空间
```shell
# 继续使用 tekton-pipelines
kubectl label --overwrite ns tekton-pipelines \       
pod-security.kubernetes.io/enforce=privileged

# 创建独立命名空间
kubectl create ns tekton-tasks
```

> 下文示例以创建独立 Namespace 为例 

## Task

```shell
kubectl apply -n tekton-tasks -f task
```

## Config

```shell script
# 注意在 config/trigger-rbac.yml 中 ClusterRoleBinding 的 Namespace 如果不同需要手动修改
kubectl apply -n tekton-tasks -f config
```

[Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry)

> 注意如果 `config.json` 中没有 `auth`，删掉 `credsStore`，重新 `docker login`  

```shell script
kubectl create secret generic dockerconfig \
  -n tekton-tasks \
  --from-file=config.json=$HOME/.docker/config.json \
  --type=kubernetes.io/dockerconfig
```

```shell script
kubectl create secret generic k8s-kubeconfig \
  -n tekton-tasks \
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
kubectl apply -n tekton-tasks -f example/grpc-gateway-pipeline.yml
kubectl apply -n tekton-tasks -f example/gmqtt-pipeline.yml
```

### 手动运行

```shell
kubectl create -n tekton-tasks -f example/grpc-gateway-run.yml
kubectl create -n tekton-tasks -f example/gmqtt-run.yml
```

### GitHub Trigger

```shell
kubectl apply -n tekton-tasks -f example/trigger.yml
```
