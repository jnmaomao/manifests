# kubeflow 快速安装
提供通过直接部署k8s相关服务yaml配置文件的方式快速搭建kubeflow服务。

> k8s服务yaml配置文件来自于manifests文件夹并通过kustomize构建

## 部署依赖
* k8s < 1.22
* kubectl

## 部署步骤
0. premise
```
cd manifests/quick_start/

# deploy pv use localhost disk storage,don't use it on production
kubectl apply -f storage/local/k8s_storage_pv_1.yaml

kubectl apply -f storage/local/k8s_storage_pv_2.yaml

kubectl apply -f storage/local/k8s_storage_pv_3.yaml

kubectl apply -f storage/local/k8s_storage_pv_jupyter.yaml

kubectl apply -f storage/local/k8s_storage_pv_minio.yaml

# apply patchs for fix some known problems
kubectl apply -f  patch/gateways-issuer.yaml

```
1. cert-manager
```
kubectl apply -f cert-manager/01_cert-manager_base.yaml

kubectl apply -f cert-manager/02_kubeflow-issuer_base.yaml
```

2. Istio
```
kubectl apply -f Istio/01_istio-crds_base.yaml

kubectl apply -f Istio/02_istio-namespace_base.yaml

kubectl apply -f Istio/03_istio-install_base.yaml
```

3. Dex
```
kubectl apply -f dex/01_dex_overlays_istio.yaml
```

4. OIDC AuthService
```
kubectl apply -f OIDC-authservice/01_oidc-authservice_base.yaml
```

5. Knative
```
# patch for fix problem on install
kubectl apply -f knative/00_patch_knative_serving_releases_download_v0.17.1_serving-crds.yaml

kubectl apply -f knative/01_knative-serving_overlays_gateways.yaml

kubectl apply -f knative/02_istio-1-11_cluster-local-gateway_base.yaml
```

6. Kubeflow Namespace
```
kubectl apply -f kubeflow-namespace/01_kubeflow-namespace_base.yaml
```

7. Kubeflow Roles
```
kubectl apply -f kubeflow_roles/01_kubeflow-roles_base.yaml
```

8. Kubeflow Istio Resources
```
kubectl apply -f kubeflow_istio_resources/01_istio-1-11_kubeflow-istio-resources_base.yaml
```

9. Kubeflow Pipelines
```
kubectl apply -f kubeflow_pipelines/01_pipeline_upstream_env_platform-agnostic-multi-user.yaml
```

10. KServe
```
kubectl apply -f kserve/01_kserve.yaml

kubectl apply -f kserve/02_kserve_models-web-app_overlays_kubeflow.yaml
```

11. Katib
```
kubectl apply -f katib/01_katib_up_stream_installs_katib-with-kubeflow.yaml
```

12. Central Dashboard
```
kubectl apply -f central-dashboard/01_centraldashboard_upstream_overlays_kserve.yaml
```

13. Admission Webhook
```
kubectl apply -f admission-webhook/01_admission-webhook_upstream_overlays_cert-manager.yaml
```

14. Notebooks
```
kubectl apply -f notebooks/01_jupyter_notebook-controller_upstream_overlays_kubeflow.yaml

kubectl apply -f notebooks/02_jupyter_jupyter-web-app_upstream_overlays_istio.yaml
```

15. Profiles + KFAM
```
kubectl apply -f profiles+KFAM/01_profiles_upstream_overlays_kubeflow.yaml
```

16. Volumes Web App
```
kubectl apply -f volumes-web-app/01_volumes-web-app_upstream_overlays_istio.yaml
```

17. Tensorboard
```
kubectl apply -f tensorboard/01_tensorboard_tensorboards-web-app_upstream_overlays_istio.yaml

kubectl apply -f tensorboard/02_tensorboard_tensorboard-controller_upstream_overlays_kubeflow.yaml
```

18. Training Operator
```
kubectl apply -f training_operator/01_training-operator_upstream_overlays_kubeflow.yaml
```

19. User Namespace
```
kubectl apply -f user_namespace/01_user-namespace_base.yaml

# deploy pv use localhost disk storage,don't use it on production
kubectl apply -f storage/local/k8s_storage_pv_jupyter.yaml
```
