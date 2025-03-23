# Kubeflow Pipelines Installation and Usage

This document outlines the steps taken to install Kubeflow Pipelines and execute a pipeline.

## Installation

1.  **Set Pipeline Version:**

    ```bash
    export PIPELINE_VERSION=2.4.0
    ```

2.  **Apply Cluster-Scoped Resources:**

    ```bash
    kubectl apply -k "[github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION](https://www.google.com/search?q=https://github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources%3Fref%3D%24PIPELINE_VERSION)"
    ```

3.  **Wait for CRD Establishment:**

    ```bash
    kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
    ```

4.  **Apply Platform-Agnostic Environment:**

    ```bash
    kubectl apply -k "[github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic?ref=$PIPELINE_VERSION](https://www.google.com/search?q=https://github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic%3Fref%3D%24PIPELINE_VERSION)"
    ```

5.  **Verify Namespaces:**

    ```bash
    kubectl get ns
    ```

    Output:

    ```
    NAME                   STATUS   AGE
    default                Active   140m
    kube-node-lease        Active   140m
    kube-public            Active   140m
    kube-system            Active   140m
    kubeflow               Active   126m
    kubernetes-dashboard   Active   133m
    ```

6.  **Verify Kubeflow Pods:**

    ```bash
    kubectl get pods -n kubeflow
    ```

    Output:

    ```
    NAME                                                   READY   STATUS      RESTARTS       AGE
    cache-deployer-deployment-86d88fd8-l2tm6               1/1     Running     0              126m
    cache-server-699d6d4b58-qmvxz                          1/1     Running     0              126m
    metadata-envoy-deployment-946867bf-9hgbk               1/1     Running     0              126m
    metadata-grpc-deployment-6c44975f56-mjvnr              1/1     Running     6 (122m ago)   126m
    metadata-writer-844c4c496d-qzdwz                       1/1     Running     1 (122m ago)   126m
    minio-5d5574b5cd-9t2sh                                 1/1     Running     0              126m
    ml-pipeline-7b8c745b88-bv768                           1/1     Running     2 (121m ago)   126m
    ml-pipeline-8xfss-system-container-driver-1351616903   0/2     Completed   0              6m38s
    ml-pipeline-8xfss-system-container-driver-1740384058   0/2     Completed   0              7m27s
    ml-pipeline-8xfss-system-container-driver-2157042636   0/2     Completed   0              8m52s
    ml-pipeline-8xfss-system-container-driver-703045280    0/2     Completed   0              5m51s
    ml-pipeline-8xfss-system-container-impl-2277833670     0/2     Completed   0              5m41s
    ml-pipeline-8xfss-system-container-impl-2740109218     0/2     Completed   0              8m42s
    ml-pipeline-8xfss-system-container-impl-2969906868     0/2     Completed   0              7m17s
    ml-pipeline-8xfss-system-container-impl-3352320617     0/2     Completed   0              6m28s
    ml-pipeline-8xfss-system-dag-driver-4173759003         0/2     Completed   0              9m13s
    ml-pipeline-persistenceagent-9cdb8686b-n5r54           1/1     Running     1 (122m ago)   126m
    ml-pipeline-scheduledworkflow-bcfc5899-59d5c           1/1     Running     0              126m
    ml-pipeline-ui-585dfd5955-kqbdg                        1/1     Running     0              126m
    ml-pipeline-viewer-crd-cbb68b94-44th4                  1/1     Running     0              126m
    ml-pipeline-visualizationserver-55c694986-ldmfh        1/1     Running     0              126m
    ml-pipeline-wnmt8-system-container-driver-1293009282   0/2     Completed   0              15s
    ml-pipeline-wnmt8-system-container-driver-1364318255   0/2     Completed   0              5s
    ml-pipeline-wnmt8-system-container-driver-2526786180   0/2     Completed   0              25s
    ml-pipeline-wnmt8-system-dag-driver-1022281107         0/2     Completed   0              35s
    mysql-85fd58798-2thzg                                 1/1     Running     0              126m
    workflow-controller-7bc7b46bfd-6g7hb                   1/1     Running     0              126m
    ```

7.  **Verify all Kubeflow resources:**

    ```bash
    kubectl get all -n kubeflow
    ```

    Output:

    ```
    NAME                                                  READY   STATUS    RESTARTS      AGE
    pod/cache-deployer-deployment-86d88fd8-l2tm6          1/1     Running   0             78m
    pod/cache-server-699d6d4b58-qmvxz                     1/1     Running   0             78m
    pod/metadata-envoy-deployment-946867bf-9hgbk          1/1     Running   0             78m
    pod/metadata-grpc-deployment-6c44975f56-mjvnr         1/1     Running   6 (75m ago)   78m
    pod/metadata-writer-844c8c496d-qzdwz                  1/1     Running   1 (75m ago)   78m
    pod/minio-5d5574b5cd-9t2sh                            1/1     Running   0             78m
    pod/ml-pipeline-7b8c745b88-bv768                      1/1     Running   2 (74m ago)   78m
    pod/ml-pipeline-persistenceagent-9cdb8686b-n5r54      1/1     Running   1 (74m ago)   78m
    pod/ml-pipeline-scheduledworkflow-bcfc5899-59d5c      1/1     Running   0             78m
    pod/ml-pipeline-ui-585dfd5955-kqbdg                   1/1     Running   0             78m
    pod/ml-pipeline-viewer-crd-cbb68b94-44th4             1/1     Running   0             78m
    pod/ml-pipeline-visualizationserver-55c694986-ldmfh   1/1     Running   0             78m
    pod/mysql-85fd58798-2thzg                             1/1     Running   0             78m
    pod/workflow-controller-7bc7b46bfd-6g7hb              1/1     Running   0             78m

    NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
    service/cache-server                      ClusterIP   10.102.162.68    <none>        443/TCP             78m
    service/metadata-envoy-service            ClusterIP   10.110.28.101    <none>        9090/TCP

![img_1.png](img_1.png)

![img_2.png](img_2.png)