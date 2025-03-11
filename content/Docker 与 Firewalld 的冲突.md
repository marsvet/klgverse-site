---
evl-share: "true"
---
firewalld 会管理底层 iptables/nftables 规则，来实现防火墙功能。
docker 的网络也是基于 iptables 的，比如容器端口映射到主机上、容器间的访问、容器对互联网的访问，都是基于 iptables 实现的。
这就导致 docker 和 firewalld 维护 iptables 规则时可能会冲突。

冲突的核心原因是：
1. firewalld 在重启时（reload 没事），会先清空 iptables 规则，然后根据设置好的 firewalld 规则来生成 iptables 规则，这就导致 docker 生成的 iptables 规则没了。
2. docker 启动时会写入 iptables 规则，容器网络变更时也会修改 iptables 规则，但如果规则没了，docker 不会再自动重新加回去。

根据这两个核心原因，可以想到两个解决方案：
1. 根据核心原因一，firewalld 重启后会根据设置好的 firewalld 规则生成 iptables 规则，那是不是可以将 docker 需要的规则也配置成 firewalld 规则？不过这比较难实现，docker 的规则跟该机器上跑的容器的个数、网络有关，并不是静态的。比较简单的解决方案是，直接将 docker0 网卡加入 firewalld 的 trusted 这个 zone（网上看来的，没验证过）。
2. 根据核心原因二，我们可以在 firewalld 重启后，再重启 docker，就没问题了，但这会导致所有容器全部停止或重启。

以上是 docker 20.10.0 之前。
docker 20.10.0 之后，docker 适配了一下 firewalld：创建一个叫 docker 的自定义 zone，并自动将 docker 相关的网卡全部加入这个 zone（比如 docker0，以及容器里的虚拟网卡等），然后 docker 会不断维护这个 zone，即使将某个网卡从 docker zone 中移除，几秒后又会被添加回去。
所以也就是说，假设重启了 firewalld，docker 的所有 iptables 规则丢失了，但几秒后，docker 又会自动将规则添回去。

> [!example] 
> firewalld 重启 → iptables 规则被清空 → `1~2s` 后 firewalld 规则生效 → `2~10s` 内 docker 陆续自动重新生成 iptables 规则

所以也就是说，从 docker 20.10.0 以后，firewalld 重启基本不会对 docker 网络产生什么影响了，唯一的影响是导致容器网络几秒钟内不可用。

> [!warning] 
> 注意：以上都是基于 CentOS 7 测试的，其他系统不能保证是这样。
> 比如 CentOS 8 的 firewalld 底层不再使用 iptables，而是改用 nftables，还不知是否符合上面所述的行为。
