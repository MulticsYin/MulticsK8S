# 安装kubectl命令行工具

本文档介绍下载和配置 kubernetes 集群命令行工具 kubelet 的步骤。

## 下载 kubectl

注意请下载对应的Kubernetes版本的安装包，推荐直接下载`server`包，上面有安装集群的所有`kubernetes`二进制文件。

```shell script
wget https://dl.k8s.io/v1.21.0/kubernetes-server-linux-amd64.tar.gz
tar -xzvf kubernetes-server-linux-amd64.tar.gz -C /apps/server/k8s/
sudo cp /apps/server/k8s/kubernetes/server/bin/kubectl /usr/local/bin/
chmod a+x /usr/bin/kube*
```

## 创建 kubectl kubeconfig 文件

```shell script
# 直接设置进 /etc/profile
export KUBE_APISERVER="https://172.16.111.100:6443"

# 设置集群参数
kubectl config set-cluster kubernetes \
  --certificate-authority=/apps/server/k8s/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER}
  
# 设置客户端认证参数
kubectl config set-credentials admin \
  --client-certificate=/apps/server/k8s/ssl/admin.pem \
  --embed-certs=true \
  --client-key=/apps/server/k8s/ssl/admin-key.pem
  
# 设置上下文参数
kubectl config set-context kubernetes \
  --cluster=kubernetes \
  --user=admin
  
# 设置默认上下文
kubectl config use-context kubernetes
```

+ `admin.pem` 证书 OU 字段值为 `system:masters`，`kube-apiserver` 预定义的 RoleBinding `cluster-admin` 将 Group `system:masters` 与 Role `cluster-admin` 绑定，该 Role 授予了调用`kube-apiserver` 相关 API 的权限；
+ 生成的 kubeconfig 被保存到 `~/.kube/config` 文件；

**注意：**`~/.kube/config`文件拥有对该集群的最高权限，请妥善保管。


**[返回目录](https://github.com/MulticsYin/MulticsKubernetes#kubernetes-%E4%BA%8C%E8%BF%9B%E5%88%B6%E9%83%A8%E7%BD%B2)**  
**[下一章 - 创建 kubeconfig 文件](https://github.com/MulticsYin/MulticsKubernetes/blob/master/artcle/004-create-kubeconfig.md#%E5%88%9B%E5%BB%BA-kubeconfig-%E6%96%87%E4%BB%B6)**
