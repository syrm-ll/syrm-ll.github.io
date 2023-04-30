<a name="R4oF5"></a>

## 拦截器执行流程:

`org.springframework.web.filter.DelegatingFilterProxy`<br />在这里, 延迟初始化拦截器链,
把请求传递给实例化后的拦截器链代理<br />↓<br />`org.springframework.security.web.FilterChainProxy`<br />这这里可能会包装 select Request 和 Response
为 `org.springframework.security.web.firewall.FirewalledRequest` 和 `javax.servlet.http.HttpServletResponse`

对包装后的请求和响应, 应用拦截器查找, 从 `this.filterChains` 中 使用 `org.springframework.security.web.SecurityFilterChain#matches`接口进行匹配,
返回匹配的拦截器
> 默认仅包含 `org.springframework.security.web.DefaultSecurityFilterChain` 其中包含大约 12 个拦截器
> ![image.png](https://cdn.nlark.com/yuque/0/2022/png/22804074/1647011945718-3afc988a-24ee-4230-9ae2-ec4a5a6c278e.png#clientId=u134501cc-2777-4&from=paste&height=456&id=u5ce2b905&originHeight=456&originWidth=574&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47597&status=done&style=none&taskId=u06102c51-7bc1-49c5-bd4b-302a93d6cf3&title=&width=574)


对每个拦截器逐一尝试, 跳过表示不支持的拦截器<br />对于认证拦截器, 找到合适的 HttpRequest 后 提取请求参数
包装为特有的 `org.springframework.security.core.Authentication`实现 作为一个认证请求,
委托 `org.springframework.security.authentication.AuthenticationManager`去实现认证

<a name="NcF7r"></a>

## 认证: AuthenticationManager

<a name="jWzze"></a>

## 其他: 关于 Servlet Filter

基础的 Spring Boot + Spring Security 会出现这些拦截器:<br />备忘
以后慢慢看<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/22804074/1647447982412-5f040fbe-a6e0-4f76-9a4f-6922111f3655.png#clientId=ua3f6a524-2ce3-4&from=paste&height=337&id=u92fed47a&originHeight=337&originWidth=1534&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73390&status=done&style=none&taskId=ubc71fde7-60cb-4823-a5f1-1514a1f8069&title=&width=1534)
