<!-- ex_nonav -->
<h1 style="font-size:250%;">Docker</h1>

<p style="font-size:150%;">容器的一种封装，提供简单易用的容器使用接口。目前最流行的容器解决方案。</p>


<ul style="font-size:150%;">
<li>基于 LXC -> runc</li>
<li>利用 namespaces 来做权限的控制和隔离</li>

<!-- 
内核的全局资源做封装，使得每个Namespace都有一份独立的资源，因此不同的进程在各自的Namespace内对同一种资源的使用不会互相干扰

IPC：隔离System V IPC和POSIX消息队列。
Network：隔离网络资源。网络设备、协议栈、路由表、防火墙规则
Mount：隔离文件系统挂载点。
PID：隔离进程ID。
UTS：隔离主机名和域名。
User：隔离用户ID和组ID。

 -->

<li>利用 cgroups 来进行资源的配置</li>

<!-- 
devices：设备权限控制。
控制CPU占用率。
内存使用上限。
冻结（暂停）Cgroup中的进程。
网络带宽。
进程的网络流量优先级。
 -->

<li>通过 AuFS 提高文件系统的资源利用率</li>
</ul>

<!-- 
rootfs -> Union File System 也叫 UnionFS -> aufs
 -->

<br>
<br>