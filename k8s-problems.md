<!-- ex_nonav -->
<h1 style="font-size:250%;">K8s 存在的问题</h1>
<p style="font-size:250%;">微服务引入的问题</p>

![microservice-concern](/img/microservice-concern.jpg)

<br>

---
<p style="font-size:250%;">负载均衡</p>
<ul style="font-size:150%;">
<li>DNS负载均衡</li>
    <ul>
    <li>多级解析延迟</li>
    <li>策略过于简单</li>
    </ul>
<li>普通软件负载均衡</li>
    <ul>
    <li>5 万级</li>
    </ul>
<li>硬件负载均衡</li>
    <ul>
    <li>F5: 15w ~ 55w</li>
    <li>A10: 55w ~ 100w</li>
    <li>根据网络层, 而非系统与应用的状态</li>
    </ul>
<li>kube-proxy 负载均衡</li>
</ul>

<br>

---

<p style="font-size:250%;">K8s 存在的问题</p>
<ul style="font-size:150%;">
<li>微服务治理</li>
    <ul>
    <li>遥测数据</li>
    <li>速率限制</li>
    <li>负载均衡</li>
    <li>TLS 证书</li>
    </ul>
<li>如何解决? service mesh!</li>
</ul>