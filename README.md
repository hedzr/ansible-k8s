# istio-ans

通过 ansible 安装 k8s 和 istio 环境。

1. 启动虚拟机，应用 ansible 的启动部分：

    ```bash
    vagrant up

    # 可以逐台虚拟机单独启动，以便仔细审查输出和排除错误
    vagrant up k8mm001
    vagrant up k8mm002
    vagrant up k8mm003
    vagrant up k8ss061
    vagrant up k8ss062
    vagrant up k8ss063
    # ...
    ```

2. 安装 dashboard
   此能力尚未有效整理：
    ```bash
    ansible-playbook site.yaml --limit local
    ```

More（TODO）：

-   安装 Metrics Server
-   启用 Kubernetes API Aggregator

## I Have Many config files

If you're managing multiple K8s clusters, there are multple `config` files in `~/.kube`.

For example, you may name them as: `config`, `config.ali.vpc1.corp1`, `config.lan.test.1`, ....

So `KUBECONFIG` is your helper. `kubectl` will lookup all files in this environment variable to authenticate into the target k8s networks.

> See also: [Configure Access Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/#set-the-kubeconfig-environment-variable)

We recommend the following script fragment to build it (fit for both bash and zsh):

```bash
[ -d ~/.kube ] && {
  if test -n "$(find ~/.kube -maxdepth 1 -name 'config.*' -print -quit)"; then
    KUBECONFIG="~/.kube/config"
    for f in ~/.kube/config.*; do
      KUBECONFIG="$KUBECONFIG:$f"
    done
    export KUBECONFIG
  fi
}
```

Simply insert it into `~/.bashrc` and `~/.zshrc` to take effect.

.
