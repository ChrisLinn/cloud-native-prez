<!-- ex_nonav -->

<h1 style="font-size:250%;">Container</h1>

<!-- 
将软件以及它运行安装所需的一切文件（代码、运行时、系统工具、系统库）打包到一起
 -->

![container](/img/why_containers.svg)

<br>

---

<br>

<h1 style="font-size:250%;">Container vs VM</h1>

<br>

![vm-vs-container](/img/vm-vs-container.png)

<br>
<br>

---

<h1 style="font-size:250%;">VM</h1>
<p style="font-size:150%;">在软件层面通过计算资源模拟一台物理机的所有资源，CPU、网卡、显卡等等</p>
<ul style="font-size:150%;">
<li>资源占用多</li>
<li>冗余步骤多</li>
<li>启动慢</li>
<li>体积大</li>
</ul>


<h1 style="font-size:250%;">Container</h1>
<p style="font-size:150%;">对进程进行隔离, 接触到的各种资源都是虚拟的</p>
<ul style="font-size:150%;">
<li>轻量级</li>
<li>易于配置</li>
<li>易于使用</li>
</ul>


<p style="font-size:150%;">左侧是 LXC 虚拟的 Ubuntu ，默认安装使用 11 MB 大小。</p>

![lxc](/img/lxc.png)