# k8s-ansible

This repo provides a set of ansible scripts (playbook) to setup a 
high availability kubernetes cluster for mimicking a production
environment.

通过 ansible 安装 k8s 高可用集群。

## Getting Started | 快速起步

It can be run in place with the ready of locally vagrant and
virtualbox.

依赖于本机上的 vagrant 和 virtualbox 环境，可以就地立即开始试验。

> You may add/append more ssh connections to remote servers into `.inventories/hosts`.
>
> 可以自行准备到 Remote Servers 的连接，并自建 `.inventories/hosts` 条目即可。参见 [`Customize 定制`](#定制) 一节。

1. Startup the VM(s) with auto provisioning | 启动虚拟机，应用 ansible 的启动部分：

   ```bash
   vagrant up

   # 可以逐台虚拟机单独启动，以便仔细审查输出和排除错误
   # startup the VMs one by one or run all of them once
   vagrant up k8mm001
   vagrant up k8mm002
   vagrant up k8mm003
   vagrant up k8ss061
   vagrant up k8ss062
   vagrant up k8ss063
   # ...
   ```

   这将会全部或者逐个地启动虚拟机，如果虚拟机尚未存在则会自动创建。随后会应用 ansible 启动
   脚本，完成节点的部署任务。

   在顺利完成全部安装步骤之后，应该可以看到就绪的控制平面和数据平面在 nodes 列表中且处于 Ready 状态。

   After all VMs startup-ed and privisioned, the status of k8s control plane should be in `Ready`.

   ```bash
   kubectl get nodes -o wide
   kubectl get pods -A
   kubectl get svc
   ```

   如果发生意外和错误，通常是因为网络质量不佳导致部分下拉未能完成，则可以单独针对该节点重新执行 ansible 脚本：

   Sometimes the unexpected matters might be occurred, but you can always provision
   the VM node again.

   ```bash
   vagrant provision k8smm003
   ```

2. To install the k8s dashboard - NOT YET | 安装 dashboard
   此能力尚未有效整理：

   ```bash
   ansible-playbook site.yaml --limit local
   ```

3. Reset to the factory state | 推倒重来

   在任何时候均可重新来过：

   ```bash
   vagrant destroy -f
   ```

   这将会删除全部有关的虚拟机。

   **CAUTION**: All VMs will be destooied.

## 定制

可以将本脚本应用到其它云环境。

1. 首先保证你可以 ssh 免密登录远程服务器，假设为 `aws001-aws006` 以及 `aws100~aws106`。

2. 然后编辑 `.inventories/hosts` 文件，使其内容为：

   ```bash
   [local]
   localhost

   [masters]
   aws00[1:9]

   [slaves]
   aws10[1:9]

   [ha]
   ```

3. 现在就可以进一步修改 `vars/main.yml` 中的 virtual ip 以及 nodes_lan_cidr，nodes 等等参数以符合你的要求。

4. 最后，可以开始向远程服务器群发出操作指令：

   ```bash
   ansible-playbook site.yaml --limit masters
   ansible-playbook site.yaml --limit slaves
   ```

## More（TODO）：

- 安装 Metrics Server
- 启用 Kubernetes API Aggregator
- 安装 istio 集群

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
