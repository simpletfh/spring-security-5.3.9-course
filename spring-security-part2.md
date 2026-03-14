# Spring Security 5.3.9.RELEASE 课程讲义（超详细版）

## 第二部分：授权机制与会话管理

---

# 模块3：授权机制详解

## 知识点9：授权核心概念

### 1. 授权概述

授权（Authorization）是Spring Security的核心功能之一，用于确定已认证的用户是否有权执行特定操作或访问特定资源。在Spring Security中，授权是通过`AccessDecisionManager`来实现的，它基于投票机制决定是否授予访问权限。

### 2. 授权架构

Spring Security的授权架构主要由以下几个核心组件组成：

#### 2.1 AccessDecisionManager（访问决策管理器）

`AccessDecisionManager`是授权决策的核心接口，负责协调多个`AccessDecisionVoter`进行投票，并根据投票结果做出最终决策。

```java
public interface AccessDecisionManager {
    /**
     * 决定是否授予访问权限
     * @param authentication 包含用户信息的认证对象
     * @param object 被保护的安全对象（如Method、FilterInvocation等）
     * @param configAttributes 与对象关联的配置属性（如角色、权限等）
     * @throws AccessDeniedException 如果访问被拒绝
     * @throws InsufficientAuthenticationException 如果认证信息不足
     */
    void decide(Authentication authentication, Object object, 
                Collection<ConfigAttribute> configAttributes)
        throws AccessDeniedException, InsufficientAuthenticationException;
}
```

Spring Security提供了三种主要的实现：

**1. AffirmativeBased（肯定式决策）**

只要有一个投票者投票同意，就授予访问权限。

```java
public class AffirmativeBased extends AbstractAccessDecisionManager {
    
    public void decide(Authentication authentication, Object object,
            Collection<ConfigAttribute> configAttributes) throws AccessDeniedException {
        
        // 获取投票者列表
        List<AccessDecisionVoter<?>> decisionVoters = getDecisionVoters();
        
        int deny = 0;
        
        // 遍历所有投票者
        for (AccessDecisionVoter<?> voter : decisionVoters) {
            // 投票
            int result = voter.vote(authentication, object, configAttributes);
            
            // 如果有任何投票者同意，立即授予访问权限
            if (result == AccessDecisionVoter.ACCESS_GRANTED) {
                return;
            }
            
            // 统计拒绝票数
            if (result == AccessDecisionVoter.ACCESS_DENIED) {
                deny++;
            }
        }
        
        // 如果所有投票者都弃权，检查是否允许
        if (deny > 0) {
            throw new AccessDeniedException(messages.getMessage(
                    "AbstractAccessDecisionManager.accessDenied", "Access is denied"));
        }
        
        // 检查是否允许所有投票者都弃权的情况
        checkAllowIfAllAbstainDecisions();
    }
}
```

**2. ConsensusBased（共识式决策）**

基于多数投票原则，赞成票多于反对票时授予访问权限。

```java
public class ConsensusBased extends AbstractAccessDecisionManager {
    
    private boolean allowIfEqualGrantedDeniedDecisions = true;
    
    public void decide(Authentication authentication, Object object,
            Collection<ConfigAttribute> configAttributes) throws AccessDeniedException {
        
        int grant = 0;
        int deny = 0;
        List<AccessDecisionVoter<?>> decisionVoters = getDecisionVoters();
        
        for (AccessDecisionVoter<?> voter : decisionVoters) {
            int result = voter.vote(authentication, object, configAttributes);
            
            if (result == AccessDecisionVoter.ACCESS_GRANTED) {
                grant++;
            } else if (result == AccessDecisionVoter.ACCESS_DENIED) {
                deny++;
            }
        }
        
        // 如果赞成票多于反对票，授予访问权限
        if (grant > deny) {
            return;
        }
        
        // 如果赞成票和反对票相等，根据配置决定
        if (deny > grant) {
            throw new AccessDeniedException(messages.getMessage(
                    "AbstractAccessDecisionManager.accessDenied", "Access is denied"));
        }
        
        // 相等的情况
        if (this.allowIfEqualGrantedDeniedDecisions) {
            return;
        } else {
            throw new AccessDeniedException(messages.getMessage(
                    "AbstractAccessDecisionManager.accessDenied", "Access is denied"));
        }
    }
}
```

**3. UnanimousBased（一致同意决策）**

所有投票者都必须投票同意，才授予访问权限。

```java
public class UnanimousBased extends AbstractAccessDecisionManager {
    
    public void decide(Authentication authentication, Object object,
            Collection<ConfigAttribute> configAttributes) throws AccessDeniedException {
        
        int grant = 0;
        List<AccessDecisionVoter<?>> decisionVoters = getDecisionVoters();
        
        for (AccessDecisionVoter<?> voter : decisionVoters) {
            int result = voter.vote(authentication, object, configAttributes);
            
            // 如果有任何投票者拒绝，立即拒绝访问
            if (result == AccessDecisionVoter.ACCESS_DENIED) {
                throw new AccessDeniedException(messages.getMessage(
                        "AbstractAccessDecisionManager.accessDenied", "Access is denied"));
            }
            
            // 统计赞成票数
            if (result == AccessDecisionVoter.ACCESS_GRANTED) {
                grant++;
            }
        }
        
        // 如果所有投票者都弃权，检查是否允许
        if (grant == 0) {
            checkAllowIfAllAbstainDecisions();
        }
    }
}
```

#### 2.2 AccessDecisionVoter（访问决策投票者）

`AccessDecisionVoter`是具体的投票者实现，负责对访问请求进行投票。

```java
public interface AccessDecisionVoter<S> {
    // 访问授予的常量
    int ACCESS_GRANTED = 1;
    // 访问拒绝的常量
    int ACCESS_DENIED = -1;
    // 弃权的常量
    int ACCESS_ABSTAIN = 0;
    
    /**
     * 投票方法
     * @param authentication 认证信息
     * @param object 安全对象
     * @param attributes 配置属性
     * @return ACCESS_GRANTED, ACCESS_DENIED, 或 ACCESS_ABSTAIN
     */
    int vote(Authentication authentication, S object, 
             Collection<ConfigAttribute> attributes);
    
    /**
     * 判断投票者是否支持给定的ConfigAttribute
     */
    boolean supports(ConfigAttribute attribute);
    
    /**
     * 判断投票者是否支持给定的安全对象类型
     */
    boolean supports(Class<?> clazz);
}
```

**常见的投票者实现：**

**1. RoleVoter（角色投票者）**

基于角色进行投票，这是最常用的投票者。

```java
public class RoleVoter implements AccessDecisionVoter<Object> {
    
    // 角色前缀，默认为"ROLE_"
    private String rolePrefix = "ROLE_";
    
    public int vote(Authentication authentication, Object object,
            Collection<ConfigAttribute> attributes) {
        
        if (authentication == null) {
            return ACCESS_DENIED;
        }
        
        int result = ACCESS_ABSTAIN;
        // 获取用户的所有权限
        Collection<? extends GrantedAuthority> authorities = extractAuthorities(authentication);
        
        for (ConfigAttribute attribute : attributes) {
            // 检查是否支持该属性（以ROLE_开头）
            if (this.supports(attribute)) {
                result = ACCESS_DENIED;
                
                // 遍历用户的所有权限
                for (GrantedAuthority authority : authorities) {
                    // 比较角色
                    if (attribute.getAttribute().equals(authority.getAuthority())) {
                        return ACCESS_GRANTED;
                    }
                }
            }
        }
        
        return result;
    }
    
    // 判断是否支持给定的ConfigAttribute
    public boolean supports(ConfigAttribute attribute) {
        if ((attribute.getAttribute() != null)
                && attribute.getAttribute().startsWith(getRolePrefix())) {
            return true;
        } else {
            return false;
        }
    }
    
    public boolean supports(Class<?> clazz) {
        return true;
    }
    
    // 从认证对象中提取权限
    Collection<? extends GrantedAuthority> extractAuthorities(
            Authentication authentication) {
        return authentication.getAuthorities();
    }
}
```

**2. WebExpressionVoter（Web表达式投票者）**

基于Spring EL表达式进行投票。

```java
public class WebExpressionVoter implements AccessDecisionVoter<FilterInvocation> {
    
    private SecurityExpressionHandler<FilterInvocation> expressionHandler = 
        new DefaultWebSecurityExpressionHandler();
    
    public int vote(Authentication authentication, FilterInvocation fi,
            Collection<ConfigAttribute> attributes) {
        
        assert authentication != null;
        assert fi != null;
        assert attributes != null;
        
        // 查找WebExpressionConfigAttribute
        WebExpressionConfigAttribute weca = findConfigAttribute(attributes);
        
        if (weca == null) {
            return ACCESS_ABSTAIN;
        }
        
        // 获取表达式
        FilterInvocationSecurityExpressionRoot root = weca.getExpressionRoot();
        root.setAuthentication(authentication);
        
        // 评估表达式
        boolean allowed = weca.getExpression().getValue(root, Boolean.class);
        
        return allowed ? ACCESS_GRANTED : ACCESS_DENIED;
    }
    
    private WebExpressionConfigAttribute findConfigAttribute(
            Collection<ConfigAttribute> attributes) {
        
        for (ConfigAttribute attribute : attributes) {
            if (attribute instanceof WebExpressionConfigAttribute) {
                return (WebExpressionConfigAttribute) attribute;
            }
        }
        return null;
    }
}
```

**3. AuthenticatedVoter（认证状态投票者）**

基于用户的认证状态进行投票（如isAnonymous(), isRememberMe(), isAuthenticated(), fullyAuthenticated()）。

```java
public class AuthenticatedVoter implements AccessDecisionVoter<Object> {
    
    public static final String IS_AUTHENTICATED_FULLY = "IS_AUTHENTICATED_FULLY";
    public static final String IS_AUTHENTICATED_REMEMBERED = "IS_AUTHENTICATED_REMEMBERED";
    public static final String IS_AUTHENTICATED_ANONYMOUSLY = "IS_AUTHENTICATED_ANONYMOUSLY";
    
    private AuthenticationTrustResolver trustResolver = new AuthenticationTrustResolverImpl();
    
    public int vote(Authentication authentication, Object object,
            Collection<ConfigAttribute> attributes) {
        
        int result = ACCESS_ABSTAIN;
        
        for (ConfigAttribute attribute : attributes) {
            if (this.supports(attribute)) {
                result = ACCESS_DENIED;
                
                if (IS_AUTHENTICATED_FULLY.equals(attribute.getAttribute())) {
                    // 必须是完全认证的用户（非匿名，非RememberMe）
                    if (trustResolver.isAnonymous(authentication)) {
                        return ACCESS_DENIED;
                    }
                    if (trustResolver.isRememberMe(authentication)) {
                        return ACCESS_DENIED;
                    }
                    return ACCESS_GRANTED;
                }
                
                if (IS_AUTHENTICATED_REMEMBERED.equals(attribute.getAttribute())) {
                    // 可以是完全认证或RememberMe
                    if (trustResolver.isAnonymous(authentication)) {
                        return ACCESS_DENIED;
                    }
                    return ACCESS_GRANTED;
                }
                
                if (IS_AUTHENTICATED_ANONYMOUSLY.equals(attribute.getAttribute())) {
                    // 任何用户都可以（包括匿名）
                    return ACCESS_GRANTED;
                }
            }
        }
        
        return result;
    }
}
```

#### 2.3 SecurityMetadataSource（安全元数据源）

`SecurityMetadataSource`负责获取与特定安全对象关联的配置属性。

```java
public interface SecurityMetadataSource {
    /**
     * 获取与给定安全对象关联的配置属性
     * @param object 安全对象
     * @return 配置属性集合
     */
    Collection<ConfigAttribute> getAttributes(Object object) 
        throws IllegalArgumentException;
    
    /**
     * 获取所有定义的配置属性
     */
    Collection<ConfigAttribute> getAllConfigAttributes();
    
    /**
     * 判断是否支持给定的类类型
     */
    boolean supports(Class<?> clazz);
}
```

**FilterInvocationSecurityMetadataSource实现：**

```java
public interface FilterInvocationSecurityMetadataSource extends SecurityMetadataSource {
    // 继承父接口方法
}
```

**DefaultFilterInvocationSecurityMetadataSource：**

```java
public class DefaultFilterInvocationSecurityMetadataSource implements
        FilterInvocationSecurityMetadataSource, InitializingBean {
    
    // 存储URL模式与配置属性的映射
    private final Map<RequestMatcher, Collection<ConfigAttribute>> requestMap;
    
    public DefaultFilterInvocationSecurityMetadataSource(
            LinkedHashMap<RequestMatcher, Collection<ConfigAttribute>> requestMap) {
        this.requestMap = requestMap;
    }
    
    @Override
    public Collection<ConfigAttribute> getAttributes(Object object) {
        final HttpServletRequest request = ((FilterInvocation) object).getRequest();
        
        // 遍历所有RequestMatcher，找到匹配的配置
        for (Map.Entry<RequestMatcher, Collection<ConfigAttribute>> entry : requestMap
                .entrySet()) {
            if (entry.getKey().matches(request)) {
                return entry.getValue();
            }
        }
        
        // 如果没有匹配的，返回null（表示拒绝访问）
        return null;
    }
    
    @Override
    public Collection<ConfigAttribute> getAllConfigAttributes() {
        Set<ConfigAttribute> allAttributes = new HashSet<>();
        
        for (Map.Entry<RequestMatcher, Collection<ConfigAttribute>> entry : requestMap
                .entrySet()) {
            allAttributes.addAll(entry.getValue());
        }
        
        return allAttributes;
    }
    
    @Override
    public boolean supports(Class<?> clazz) {
        return FilterInvocation.class.isAssignableFrom(clazz);
    }
}
```

### 3. 授权流程

完整的授权流程如下：

```java
// 1. 用户发起请求
HttpServletRequest request = ...;

// 2. 请求经过过滤器链
FilterInvocation fi = new FilterInvocation(request, response, chain);

// 3. FilterSecurityInterceptor拦截请求
public class FilterSecurityInterceptor extends AbstractSecurityInterceptor implements
        Filter {
    
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        FilterInvocation fi = new FilterInvocation(request, response, chain);
        
        // 执行授权
        InterceptorStatusToken token = super.beforeInvocation(fi);
        
        try {
            // 继续过滤器链
            fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
        } finally {
            super.finallyInvocation(token);
        }
        
        super.afterInvocation(token, null);
    }
}

// 4. AbstractSecurityInterceptor执行授权决策
public abstract class AbstractSecurityInterceptor implements
        InitializingBean, ApplicationEventPublisherAware {
    
    protected InterceptorStatusToken beforeInvocation(Object object) {
        
        // 获取安全元数据
        Collection<ConfigAttribute> attributes = this.obtainSecurityMetadataSource()
                .getAttributes(object);
        
        if (attributes == null || attributes.isEmpty()) {
            // 没有配置属性，拒绝访问
            if (rejectPublicInvocations) {
                throw new IllegalArgumentException(...);
            }
            publishEvent(new PublicInvocationEvent(object));
            return null; // 无需认证
        }
        
        // 确保已认证
        Authentication authenticated = authenticateIfRequired();
        
        try {
            // 授权决策
            this.accessDecisionManager.decide(authenticated, object, attributes);
        } catch (AccessDeniedException accessDeniedException) {
            publishEvent(new AuthorizationFailureEvent(object, attributes, authenticated,
                    accessDeniedException));
            throw accessDeniedException;
        }
        
        // ... 运行后处理逻辑
        
        return new InterceptorStatusToken(...);
    }
    
    private Authentication authenticateIfRequired() {
        Authentication authentication = SecurityContextHolder.getContext()
                .getAuthentication();
        
        if (authentication.isAuthenticated() && !alwaysReauthenticate) {
            return authentication;
        }
        
        // 重新认证
        authentication = authenticationManager.authenticate(authentication);
        
        SecurityContextHolder.getContext().setAuthentication(authentication);
        
        return authentication;
    }
}
```

### 4. 完整示例

#### 4.1 Web安全配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                // 管理员页面需要ADMIN角色
                .antMatchers("/admin/**").hasRole("ADMIN")
                // 用户页面需要USER角色
                .antMatchers("/user/**").hasRole("USER")
                // 公开页面
                .antMatchers("/public/**", "/home").permitAll()
                // 其他请求需要认证
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .logout()
                .permitAll();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth
            .inMemoryAuthentication()
                .withUser("admin")
                .password("{noop}admin123")
                .roles("ADMIN")
                .and()
                .withUser("user")
                .password("{noop}user123")
                .roles("USER");
    }
}
```

#### 4.2 方法级安全配置

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
    
    @Override
    protected MethodSecurityExpressionHandler createExpressionHandler() {
        DefaultMethodSecurityExpressionHandler handler = 
            new DefaultMethodSecurityExpressionHandler();
        handler.setPermissionEvaluator(new CustomPermissionEvaluator());
        return handler;
    }
}

@Service
public class DocumentService {
    
    // 使用@PreAuthorize
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteDocument(Long id) {
        // 只有ADMIN角色可以删除文档
    }
    
    // 使用@PostAuthorize
    @PostAuthorize("returnObject.owner == authentication.name")
    public Document getDocument(Long id) {
        // 返回后检查文档的所有者
        return documentRepository.findById(id);
    }
    
    // 使用@Secured
    @Secured("ROLE_USER")
    public void createDocument(Document document) {
        // 需要USER角色
    }
    
    // 使用自定义权限
    @PreAuthorize("@permissionEvaluator.canAccessDocument(#id, authentication.name)")
    public Document viewDocument(Long id) {
        return documentRepository.findById(id);
    }
}
```

### 5. FAQ

**Q1: AccessDecisionManager的三种实现有什么区别？应该如何选择？**

A: 
- **AffirmativeBased（默认）**：只要有一个投票者同意就授权。适合大多数场景，效率高。
- **ConsensusBased**：基于多数投票，赞成票多于反对票才授权。适合需要更严格控制的场景。
- **UnanimousBased**：所有投票者都必须同意才授权。适合最高安全级别的场景。

选择建议：
- 一般应用使用AffirmativeBased即可
- 金融、医疗等敏感系统考虑ConsensusBased
- 军事、国家安全等极高安全要求使用UnanimousBased

**Q2: 自定义投票者应该如何实现？**

A: 实现`AccessDecisionVoter`接口：

```java
public class IpAddressVoter implements AccessDecisionVoter<FilterInvocation> {
    
    private Set<String> allowedIps = new HashSet<>();
    
    public void setAllowedIps(Set<String> allowedIps) {
        this.allowedIps = allowedIps;
    }
    
    @Override
    public int vote(Authentication authentication, FilterInvocation fi,
            Collection<ConfigAttribute> attributes) {
        
        String clientIp = fi.getRequest().getRemoteAddr();
        
        for (ConfigAttribute attribute : attributes) {
            if (attribute.getAttribute().startsWith("IP_")) {
                String allowedIp = attribute.getAttribute().substring(3);
                if (clientIp.equals(allowedIp)) {
                    return ACCESS_GRANTED;
                }
            }
        }
        
        return ACCESS_ABSTAIN;
    }
    
    @Override
    public boolean supports(ConfigAttribute attribute) {
        return attribute.getAttribute().startsWith("IP_");
    }
    
    @Override
    public boolean supports(Class<?> clazz) {
        return FilterInvocation.class.isAssignableFrom(clazz);
    }
}
```

然后在配置中注册：

```java
@Bean
public AccessDecisionManager accessDecisionManager() {
    List<AccessDecisionVoter<? extends Object>> decisionVoters = new ArrayList<>();
    decisionVoters.add(new RoleVoter());
    decisionVoters.add(new WebExpressionVoter());
    decisionVoters.add(new IpAddressVoter());
    
    return new AffirmativeBased(decisionVoters);
}
```

**Q3: hasRole()和hasAuthority()有什么区别？**

A: 
- `hasRole("ADMIN")` 会自动添加"ROLE_"前缀，实际检查的是"ROLE_ADMIN"
- `hasAuthority("ADMIN")` 直接检查"ADMIN"，不会添加前缀

示例：
```java
// 用户有权限：ROLE_ADMIN
hasRole("ADMIN")        // true - 自动添加ROLE_前缀
hasAuthority("ROLE_ADMIN")  // true
hasAuthority("ADMIN")   // false - 没有前缀

// 配置用户时
auth.inMemoryAuthentication()
    .withUser("user")
    .password("password")
    .roles("ADMIN");  // 自动添加ROLE_前缀

auth.inMemoryAuthentication()
    .withUser("user")
    .password("password")
    .authorities("ROLE_ADMIN");  // 不添加前缀
```

**Q4: 如何实现动态权限控制？**

A: 实现动态权限有几种方式：

方式1：自定义SecurityMetadataSource

```java
public class DatabaseSecurityMetadataSource implements 
        FilterInvocationSecurityMetadataSource {
    
    @Autowired
    private PermissionRepository permissionRepository;
    
    private Map<String, Collection<ConfigAttribute>> configAttributeMap = new ConcurrentHashMap<>();
    
    @PostConstruct
    public void loadConfig() {
        // 从数据库加载权限配置
        List<Permission> permissions = permissionRepository.findAll();
        
        for (Permission permission : permissions) {
            Collection<ConfigAttribute> attributes = new ArrayList<>();
            attributes.add(new SecurityConfig(permission.getRole()));
            configAttributeMap.put(permission.getUrl(), attributes);
        }
    }
    
    @Override
    public Collection<ConfigAttribute> getAttributes(Object object) {
        FilterInvocation fi = (FilterInvocation) object;
        String url = fi.getRequestUrl();
        
        // 动态查找匹配的权限配置
        for (Map.Entry<String, Collection<ConfigAttribute>> entry : 
                configAttributeMap.entrySet()) {
            if (new AntPathMatcher().match(entry.getKey(), url)) {
                return entry.getValue();
            }
        }
        
        return null;
    }
    
    // 定时刷新配置
    @Scheduled(cron = "0 0/5 * * * ?")
    public void refreshConfig() {
        loadConfig();
    }
}
```

方式2：使用自定义PermissionEvaluator

```java
@Component
public class CustomPermissionEvaluator implements PermissionEvaluator {
    
    @Autowired
    private PermissionService permissionService;
    
    @Override
    public boolean hasPermission(Authentication authentication, 
            Object targetId, Object permission) {
        
        String username = authentication.getName();
        String resource = targetId.toString();
        String action = permission.toString();
        
        return permissionService.hasPermission(username, resource, action);
    }
    
    @Override
    public boolean hasPermission(Authentication authentication, 
            Serializable targetId, String targetType, Object permission) {
        // 实现逻辑
        return true;
    }
}

// 使用
@PreAuthorize("@permissionEvaluator.hasPermission(#id, 'READ')")
public Document getDocument(Long id) {
    return documentRepository.findById(id);
}
```

**Q5: 为什么我的自定义AccessDecisionManager不生效？**

A: 常见原因和解决方案：

1. **没有正确配置到HttpSecurity**

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .accessDecisionManager(accessDecisionManager())  // 必须设置
            .antMatchers("/admin/**").hasRole("ADMIN")
            // ...
}

@Bean
public AccessDecisionManager accessDecisionManager() {
    List<AccessDecisionVoter<? extends Object>> voters = new ArrayList<>();
    voters.add(new RoleVoter());
    voters.add(new WebExpressionVoter());
    return new AffirmativeBased(voters);
}
```

2. **使用@EnableGlobalMethodSecurity时没有配置全局AccessDecisionManager**

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
    
    @Override
    protected AccessDecisionManager accessDecisionManager() {
        // 自定义AccessDecisionManager
        return new AffirmativeBase(getDecisionVoters());
    }
}
```

3. **FilterSecurityInterceptor没有使用自定义的AccessDecisionManager**

检查WebSecurityConfigurerAdapter的配置是否正确。

### 6. 思考题

1. 在一个微服务架构的系统中，如何实现统一的权限管理？请设计一个基于Spring Security的方案。

2. Spring Security的授权模型和Shiro的授权模型有什么异同？各自的优缺点是什么？

3. 如何设计一个高性能的动态权限系统？考虑缓存、数据库查询优化等方面。

4. 在分布式系统中，如何保证权限配置的一致性？如果权限配置变更，如何实时生效？

5. 如何实现基于数据的权限控制（Data-level Security）？比如用户只能看到自己创建的数据？

---

## 知识点10：过滤器链详解

### 1. 过滤器链概述

Spring Security的核心是一个过滤器链（Filter Chain），通过一系列过滤器来处理安全相关的任务。每个过滤器负责特定的安全功能，认证、授权、CSRF防护、会话管理等都是通过过滤器实现的。

### 2. 核心过滤器详解

#### 2.1 FilterChainProxy（过滤器链代理）

`FilterChainProxy`是Spring Security的入口点，它是一个特殊的过滤器，内部管理着一个或多个安全过滤器链。

```java
public class FilterChainProxy extends GenericFilterBean {
    
    // 存储多个SecurityFilterChain
    private List<SecurityFilterChain> filterChains;
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        // 清除之前的SecurityContext
        boolean clearContext = true;
        
        try {
            // 执行匹配的过滤器链
            doFilterInternal(request, response, chain);
            clearContext = false;
        } finally {
            if (clearContext) {
                // 清除SecurityContext
                SecurityContextHolder.clearContext();
            }
        }
    }
    
    private void doFilterInternal(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        FirewalledRequest fwRequest = firewall.getFirewalledRequest((HttpServletRequest) request);
        HttpServletResponse fwResponse = firewall.getFirewalledResponse((HttpServletResponse) response);
        
        // 获取匹配的SecurityFilterChain
        List<Filter> filters = getFilters(fwRequest);
        
        if (filters == null || filters.size() == 0) {
            // 没有匹配的过滤器链，继续执行
            if (logger.isDebugEnabled()) {
                logger.debug(ChainProxy.class.getName()
                        + " has no empty filters, delegating to fallback filter chain.");
            }
            
            fwRequest.reset();
            chain.doFilter(fwRequest, fwResponse);
            return;
        }
        
        // 创建虚拟过滤器链
        VirtualFilterChain vfc = new VirtualFilterChain(fwRequest, chain, filters);
        vfc.doFilter(fwRequest, fwResponse);
    }
    
    /**
     * 获取匹配当前请求的过滤器列表
     */
    private List<Filter> getFilters(HttpServletRequest request) {
        for (SecurityFilterChain chain : filterChains) {
            if (chain.matches(request)) {
                return chain.getFilters();
            }
        }
        return null;
    }
    
    /**
     * VirtualFilterChain - 虚拟过滤器链
     * 用于执行Spring Security的过滤器列表
     */
    private static class VirtualFilterChain implements FilterChain {
        
        private final FilterChain originalChain;
        private final List<Filter> additionalFilters;
        private final FirewalledRequest firewalledRequest;
        private final int size;
        private int currentPosition = 0;
        
        private VirtualFilterChain(FirewalledRequest firewalledRequest,
                FilterChain chain, List<Filter> additionalFilters) {
            this.originalChain = chain;
            this.additionalFilters = additionalFilters;
            this.size = additionalFilters.size();
            this.firewalledRequest = firewalledRequest;
        }
        
        @Override
        public void doFilter(ServletRequest request, ServletResponse response)
                throws IOException, ServletException {
            
            // 重置请求
            if (currentPosition == size) {
                if (logger.isDebugEnabled()) {
                    logger.debug("Reached end of additional filter chain; proceeding with original chain");
                }
                
                // 所有过滤器执行完毕，执行原始过滤器链
                firewalledRequest.reset();
                originalChain.doFilter(request, response);
            } else {
                currentPosition++;
                
                // 获取下一个过滤器
                Filter nextFilter = additionalFilters.get(currentPosition - 1);
                
                if (logger.isDebugEnabled()) {
                    logger.debug("Invoking filter: " + nextFilter);
                }
                
                // 执行过滤器
                nextFilter.doFilter(request, response, this);
            }
        }
    }
}
```

#### 2.2 SecurityContextPersistenceFilter（安全上下文持久化过滤器）

在请求开始时从SecurityContextRepository中加载SecurityContext，请求结束时保存SecurityContext。

```java
public class SecurityContextPersistenceFilter extends GenericFilterBean {
    
    private SecurityContextRepository repo;
    private boolean forceEagerSessionCreation = false;
    
    public SecurityContextPersistenceFilter() {
        this(new HttpSessionSecurityContextRepository());
    }
    
    public SecurityContextPersistenceFilter(SecurityContextRepository repo) {
        this.repo = repo;
    }
    
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        // 请求开始前，从仓库加载SecurityContext
        SecurityContext contextBeforeChainExecution = repo.loadContext(
                new HttpRequestResponseHolder(request, response));
        
        try {
            // 设置SecurityContext到ThreadLocal
            SecurityContextHolder.setContext(contextBeforeChainExecution);
            
            // 继续过滤器链
            chain.doFilter(request, response);
        } finally {
            // 请求结束后，获取当前SecurityContext
            SecurityContext contextAfterChainExecution = SecurityContextHolder
                    .getContext();
            
            // 清除ThreadLocal，防止内存泄漏
            SecurityContextHolder.clearContext();
            
            // 保存SecurityContext到仓库
            repo.saveContext(contextAfterChainExecution, request, response);
        }
    }
}
```

**HttpSessionSecurityContextRepository实现：**

```java
public class HttpSessionSecurityContextRepository implements
        SecurityContextRepository {
    
    // Session中保存SecurityContext的属性名
    static final String SPRING_SECURITY_CONTEXT_KEY = "SPRING_SECURITY_CONTEXT";
    
    private boolean allowSessionCreation = true;
    private boolean disableUrlRewriting = true;
    private String springSecurityContextKey = SPRING_SECURITY_CONTEXT_KEY;
    
    @Override
    public SecurityContext loadContext(
            HttpRequestResponseHolder requestResponseHolder) {
        
        HttpServletRequest request = requestResponseHolder.getRequest();
        HttpServletResponse response = requestResponseHolder.getResponse();
        
        HttpSession httpSession = request.getSession(false);
        
        SecurityContext context = readSecurityContextFromSession(httpSession);
        
        if (context == null) {
            context = generateNewContext();
        }
        
        // 包装请求和响应，用于延迟保存Session
        requestResponseHolder.setRequest(
                new SaveToSessionRequestWrapper(request, response, context));
        requestResponseHolder.setResponse(
                new SaveToSessionResponseWrapper(response, request));
        
        return context;
    }
    
    @Override
    public void saveContext(SecurityContext context, HttpServletRequest request,
            HttpServletResponse response) {
        
        SaveToSessionRequestWrapper requestWrapper = 
            (SaveToSessionRequestWrapper) request;
        
        // 检查context是否改变
        if (requestWrapper.isContextSaved()) {
            return;
        }
        
        HttpSession httpSession = request.getSession(false);
        
        if (httpSession == null) {
            httpSession = request.getSession(true);
        }
        
        // 保存SecurityContext到Session
        httpSession.setAttribute(springSecurityContextKey, context);
    }
    
    private SecurityContext readSecurityContextFromSession(HttpSession httpSession) {
        if (httpSession == null) {
            return null;
        }
        
        Object contextFromSession = httpSession.getAttribute(springSecurityContextKey);
        
        if (contextFromSession == null) {
            return null;
        }
        
        return (SecurityContext) contextFromSession;
    }
}
```

#### 2.3 LogoutFilter（登出过滤器）

处理用户登出逻辑。

```java
public class LogoutFilter extends GenericFilterBean {
    
    private final String logoutSuccessUrl;
    private final LogoutHandler[] handlers;
    private final FilterProcessesUrlFilterProcessesUrl filterProcessesUrl;
    
    public LogoutFilter(LogoutSuccessHandler logoutSuccessHandler,
            LogoutHandler... handlers) {
        this.handlers = handlers;
        this.logoutSuccessHandler = logoutSuccessHandler;
    }
    
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        // 检查是否是登出请求
        if (requiresLogout(request, response)) {
            // 执行登出处理器
            for (LogoutHandler handler : handlers) {
                handler.logout(request, response, authentication);
            }
            
            // 登出成功处理
            logoutSuccessHandler.onLogoutSuccess(request, response, authentication);
            
            return;
        }
        
        // 继续过滤器链
        chain.doFilter(req, res);
    }
    
    private boolean requiresLogout(HttpServletRequest request,
            HttpServletResponse response) {
        
        String uri = request.getRequestURI();
        
        return uri.endsWith(filterProcessesUrl.getFilterProcessesUrl(
                request.getServletPath()));
    }
}
```

**LogoutHandler实现：**

```java
// SecurityContextLogoutHandler - 清除SecurityContext
public class SecurityContextLogoutHandler implements LogoutHandler {
    
    private boolean invalidateHttpSession = true;
    private boolean clearAuthentication = true;
    
    @Override
    public void logout(HttpServletRequest request, HttpServletResponse response,
            Authentication authentication) {
        
        if (clearAuthentication) {
            // 清除SecurityContext
            SecurityContextHolder.clearContext();
        }
        
        if (invalidateHttpSession) {
            // 使Session失效
            HttpSession session = request.getSession(false);
            if (session != null) {
                session.invalidate();
            }
        }
    }
}

// CookieClearingLogoutHandler - 清除指定的Cookie
public class CookieClearingLogoutHandler implements LogoutHandler {
    
    private final List<String> cookiesToClear;
    
    public CookieClearingLogoutHandler(String... cookiesToClear) {
        this.cookiesToClear = Arrays.asList(cookiesToClear);
    }
    
    @Override
    public void logout(HttpServletRequest request, HttpServletResponse response,
            Authentication authentication) {
        
        for (String cookieName : cookiesToClear) {
            Cookie cookie = new Cookie(cookieName, null);
            String cookiePath = request.getContextPath();
            if (!cookiePath.isEmpty()) {
                cookie.setPath(cookiePath);
            }
            cookie.setMaxAge(0);
            cookie.setSecure(request.isSecure());
            cookie.setHttpOnly(true);
            
            // 清除Cookie
            response.addCookie(cookie);
        }
    }
}
```

#### 2.4 UsernamePasswordAuthenticationFilter（用户名密码认证过滤器）

处理基于表单的登录认证。

```java
public class UsernamePasswordAuthenticationFilter extends
        AbstractAuthenticationProcessingFilter {
    
    public static final String SPRING_SECURITY_FORM_USERNAME_KEY = "username";
    public static final String SPRING_SECURITY_FORM_PASSWORD_KEY = "password";
    
    private String usernameParameter = SPRING_SECURITY_FORM_USERNAME_KEY;
    private String passwordParameter = SPRING_SECURITY_FORM_PASSWORD_KEY;
    private boolean postOnly = true;
    
    public UsernamePasswordAuthenticationFilter() {
        super(new AntPathRequestMatcher("/login", "POST"));
    }
    
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request,
            HttpServletResponse response) throws AuthenticationException {
        
        // 必须是POST请求
        if (postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                    "Authentication method not supported: " + request.getMethod());
        }
        
        // 获取用户名和密码
        String username = obtainUsername(request);
        String password = obtainPassword(request);
        
        if (username == null) {
            username = "";
        }
        if (password == null) {
            password = "";
        }
        
        username = username.trim();
        
        // 创建未认证的Authentication对象
        UsernamePasswordAuthenticationToken authRequest = 
            new UsernamePasswordAuthenticationToken(username, password);
        
        // 设置详细信息（IP地址、Session ID等）
        setDetails(request, authRequest);
        
        // 委托给AuthenticationManager进行认证
        return this.getAuthenticationManager().authenticate(authRequest);
    }
    
    protected String obtainUsername(HttpServletRequest request) {
        return request.getParameter(usernameParameter);
    }
    
    protected String obtainPassword(HttpServletRequest request) {
        return request.getParameter(passwordParameter);
    }
    
    protected void setDetails(HttpServletRequest request,
            UsernamePasswordAuthenticationToken authRequest) {
        authRequest.setDetails(authenticationDetailsSource.buildDetails(request));
    }
}
```

**AbstractAuthenticationProcessingFilter：**

```java
public abstract class AbstractAuthenticationProcessingFilter extends GenericFilterBean
        implements ApplicationEventPublisherAware {
    
    private AuthenticationManager authenticationManager;
    private AuthenticationSuccessHandler successHandler;
    private AuthenticationFailureHandler failureHandler;
    
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        // 检查是否需要认证
        if (!requiresAuthentication(request, response)) {
            chain.doFilter(request, response);
            return;
        }
        
        Authentication authResult;
        
        try {
            // 尝试认证
            authResult = attemptAuthentication(request, response);
            
            if (authResult == null) {
                return;
            }
            
            // 认证成功后的处理
            successfulAuthentication(request, response, chain, authResult);
        } catch (InternalAuthenticationServiceException failed) {
            // 认证失败
            unsuccessfulAuthentication(request, response, failed);
        } catch (AuthenticationException failed) {
            unsuccessfulAuthentication(request, response, failed);
        }
    }
    
    protected abstract Authentication attemptAuthentication(
            HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException, IOException, ServletException;
    
    protected void successfulAuthentication(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain, Authentication authResult)
            throws IOException, ServletException {
        
        // 将认证结果保存到SecurityContext
        SecurityContextHolder.getContext().setAuthentication(authResult);
        
        // 记住我服务
        rememberMeServices.loginSuccess(request, response, authResult);
        
        // 发布认证成功事件
        if (this.eventPublisher != null) {
            eventPublisher.publishEvent(new 
                InteractiveAuthenticationSuccessEvent(authResult, this.getClass()));
        }
        
        // 成功处理器
        successHandler.onAuthenticationSuccess(request, response, authResult);
    }
    
    protected void unsuccessfulAuthentication(HttpServletRequest request,
            HttpServletResponse response, AuthenticationException failed)
            throws IOException, ServletException {
        
        // 清除SecurityContext
        SecurityContextHolder.clearContext();
        
        // 记住我服务
        rememberMeServices.loginFail(request, response);
        
        // 失败处理器
        failureHandler.onAuthenticationFailure(request, response, failed);
    }
}
```

#### 2.5 FilterSecurityInterceptor（过滤器安全拦截器）

在过滤器链中进行最终的授权决策。

```java
public class FilterSecurityInterceptor extends AbstractSecurityInterceptor implements
        Filter {
    
    private FilterInvocationSecurityMetadataSource securityMetadataSource;
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        FilterInvocation fi = new FilterInvocation(request, response, chain);
        
        invoke(fi);
    }
    
    public void invoke(FilterInvocation fi) throws IOException, ServletException {
        
        // 在执行前进行授权决策
        InterceptorStatusToken token = super.beforeInvocation(fi);
        
        try {
            // 继续过滤器链
            fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
        } finally {
            // 执行后处理
            super.finallyInvocation(token);
        }
        
        super.afterInvocation(token, null);
    }
}
```

**AbstractSecurityInterceptor核心逻辑：**

```java
public abstract class AbstractSecurityInterceptor implements
        InitializingBean, ApplicationEventPublisherAware {
    
    protected InterceptorStatusToken beforeInvocation(Object object) {
        
        Assert.notNull(object, "Object was null");
        
        if (!getSecureObjectClass().isAssignableFrom(object.getClass())) {
            throw new IllegalArgumentException(...);
        }
        
        // 获取配置属性（URL模式对应的角色等）
        Collection<ConfigAttribute> attributes = this.obtainSecurityMetadataSource()
                .getAttributes(object);
        
        if (attributes == null || attributes.isEmpty()) {
            // 没有配置属性，拒绝访问
            if (rejectPublicInvocations) {
                throw new IllegalArgumentException(...);
            }
            publishEvent(new PublicInvocationEvent(object));
            
            return null; // 无需认证
        }
        
        // 确保已认证
        Authentication authenticated = authenticateIfRequired();
        
        try {
            // 授权决策 - 调用AccessDecisionManager
            this.accessDecisionManager.decide(authenticated, object, attributes);
        } catch (AccessDeniedException accessDeniedException) {
            publishEvent(new AuthorizationFailureEvent(object, attributes, authenticated,
                    accessDeniedException));
            throw accessDeniedException;
        }
        
        // 运行后处理
        RunAsManager runAsManager = this.runAsManager;
        if (runAsManager != null) {
            Authentication newAuth = runAsManager.buildRunAs(authenticated, object,
                    attributes);
            if (newAuth != null) {
                SecurityContextHolder.getContext().setAuthentication(newAuth);
                return new InterceptorStatusToken(authenticated, false, attributes, object);
            }
        }
        
        return new InterceptorStatusToken(authenticated, false, attributes, object);
    }
    
    protected void finallyInvocation(InterceptorStatusToken token) {
        if (token != null && token.isContextHolderRefreshRequired()) {
            // 恢复原始的Authentication
            SecurityContextHolder.getContext().setAuthentication(
                    token.getAuthentication());
        }
    }
    
    protected void afterInvocation(InterceptorStatusToken token, Object returnedObject) {
        if (token == null) {
            return; // beforeInvocation返回null，无需后处理
        }
        
        // 调用AfterInvocationManager进行后处理
        if (afterInvocationManager != null) {
            try {
                returnedObject = afterInvocationManager.decide(
                        token.getAuthentication(), token.getObject(),
                        token.getAttributes(), returnedObject);
            } catch (AccessDeniedException accessDeniedException) {
                publishEvent(new AuthorizationFailureEvent(...));
                throw accessDeniedException;
            }
        }
        
        return returnedObject;
    }
}
```

#### 2.6 ExceptionTranslationFilter（异常转换过滤器）

捕获AccessDeniedException和AuthenticationException，并委托给相应的处理策略。

```java
public class ExceptionTranslationFilter extends GenericFilterBean {
    
    private AccessDeniedHandler accessDeniedHandler;
    private AuthenticationEntryPoint authenticationEntryPoint;
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        try {
            chain.doFilter(request, response);
        } catch (Exception ex) {
            // 处理异常
            handleException(request, response, chain, ex);
        }
    }
    
    private void handleException(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain, Exception exception)
            throws IOException, ServletException {
        
        if (exception instanceof AccessDeniedException) {
            AccessDeniedException accessDeniedException = (AccessDeniedException) exception;
            
            Authentication authentication = SecurityContextHolder.getContext()
                    .getAuthentication();
            
            if (authenticationTrustResolver.isAnonymous(authentication) ||
                authenticationTrustResolver.isRememberMe(authentication)) {
                
                // 如果是匿名用户或RememberMe用户，抛出AuthenticationException
                // 交给AuthenticationEntryPoint处理
                sendStartAuthentication(request, response, chain,
                        new InsufficientAuthenticationException(
                            "Full authentication is required to access this resource"));
            } else {
                // 已认证用户但权限不足
                accessDeniedHandler.handle(request, response, accessDeniedException);
            }
        } else if (exception instanceof AuthenticationException) {
            // 认证异常
            sendStartAuthentication(request, response, chain,
                    (AuthenticationException) exception);
        }
    }
    
    protected void sendStartAuthentication(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain,
            AuthenticationException reason) throws ServletException, IOException {
        
        SecurityContextHolder.getContext().setAuthentication(null);
        
        // 委托给AuthenticationEntryPoint处理
        authenticationEntryPoint.commence(request, response, reason);
    }
}
```

#### 2.7 CsrfFilter（CSRF防护过滤器）

防护跨站请求伪造攻击。

```java
public final class CsrfFilter extends OncePerRequestFilter {
    
    public static final String CSRF_TOKEN_ATTR_NAME = 
        CsrfToken.class.getName();
    
    private final CsrfTokenRepository tokenRepository;
    private RequestMatcher requireCsrfProtectionMatcher = 
        new DefaultRequiresCsrfMatcher();
    private AccessDeniedHandler accessDeniedHandler = 
        new AccessDeniedHandlerImpl();
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        
        request.setAttribute(HttpServletResponse.class.getName(), response);
        
        // 获取或创建CSRF Token
        CsrfToken csrfToken = tokenRepository.loadToken(request);
        
        if (csrfToken == null) {
            csrfToken = tokenRepository.generateToken(request);
            tokenRepository.saveToken(csrfToken, request, response);
        }
        
        // 将Token设置到请求属性中
        request.setAttribute(csrfToken.getParameterName(), csrfToken);
        request.setAttribute(CSRF_TOKEN_ATTR_NAME, csrfToken);
        
        // 检查是否需要CSRF保护
        if (this.requireCsrfProtectionMatcher.matches(request)) {
            // 从请求中获取Token
            String actualToken = request.getHeader(csrfToken.getHeaderName());
            
            if (actualToken == null) {
                actualToken = request.getParameter(csrfToken.getParameterName());
            }
            
            if (!csrfToken.getToken().equals(actualToken)) {
                // Token不匹配，拒绝访问
                accessDeniedHandler.handle(request, response,
                    new InvalidCsrfTokenException(csrfToken, actualToken));
                return;
            }
        }
        
        // 继续过滤器链
        filterChain.doFilter(request, response);
    }
    
    /**
     * 默认需要CSRF保护的请求匹配器
     */
    private static final class DefaultRequiresCsrfMatcher implements RequestMatcher {
        
        private final HashSet<String> allowedMethods = new HashSet<>(
                Arrays.asList("GET", "HEAD", "TRACE", "OPTIONS"));
        
        @Override
        public boolean matches(HttpServletRequest request) {
            // GET、HEAD、TRACE、OPTIONS方法不需要CSRF保护
            return !this.allowedMethods.contains(request.getMethod());
        }
    }
}
```

#### 2.8 其他重要过滤器

**ConcurrentSessionFilter（并发会话过滤器）：**

```java
public class ConcurrentSessionFilter extends GenericFilterBean {
    
    private SessionRegistry sessionRegistry;
    private String expiredUrl;
    
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        HttpSession session = request.getSession(false);
        
        if (session != null) {
            // 检查Session是否过期
            SessionInformation info = sessionRegistry.getSessionInformation(session
                    .getId());
            
            if (info != null) {
                if (info.isExpired()) {
                    // Session已过期
                    sessionRegistry.removeSessionInformation(session.getId());
                    session.invalidate();
                    
                    if (expiredUrl != null) {
                        response.sendRedirect(expiredUrl);
                        return;
                    } else {
                        response.getWriter().print(
                            "This session has been expired (possibly due to multiple concurrent logins being attempted by the same user).");
                        return;
                    }
                }
                
                // 更新最后访问时间
                info.refreshLastRequest();
            }
        }
        
        chain.doFilter(req, res);
    }
}
```

**RequestCacheAwareFilter（请求缓存过滤器）：**

```java
public class RequestCacheAwareFilter extends GenericFilterBean {
    
    private RequestCache requestCache;
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        // 检查是否有缓存的请求
        HttpServletRequest wrappedSavedRequest = requestCache.getMatchingRequest(
                (HttpServletRequest) request, (HttpServletResponse) response);
        
        chain.doFilter(wrappedSavedRequest == null ? request : wrappedSavedRequest,
                response);
    }
}
```

### 3. 完整过滤器链配置

#### 3.1 默认过滤器链顺序

Spring Security的默认过滤器链顺序（按执行顺序）：

1. **ChannelProcessingFilter** - 渠道决策过滤器（HTTP/HTTPS）
2. **ConcurrentSessionFilter** - 并发会话控制
3. **SecurityContextPersistenceFilter** - SecurityContext持久化
4. **LogoutFilter** - 登出处理
5. **UsernamePasswordAuthenticationFilter** - 表单登录认证
6. **CasAuthenticationFilter** - CAS认证（如果使用CAS）
7. **BasicAuthenticationFilter** - HTTP Basic认证
8. **RememberMeAuthenticationFilter** - RememberMe认证
9. **AnonymousAuthenticationFilter** - 匿名身份认证
10. **SessionManagementFilter** - 会话管理
11. **ExceptionTranslationFilter** - 异常转换
12. **FilterSecurityInterceptor** - 最终授权决策
13. **SwitchUserFilter** - 用户切换（可选）

#### 3.2 自定义配置示例

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // 添加自定义过滤器
            .addFilterBefore(new CustomLoggingFilter(), 
                ChannelProcessingFilter.class)
            .addFilterAfter(new CustomHeaderFilter(), 
                SecurityContextPersistenceFilter.class)
            .addFilterAt(new CustomAuthenticationFilter(),
                UsernamePasswordAuthenticationFilter.class)
            
            // CSRF配置
            .csrf()
                .csrfTokenRepository(customCsrfTokenRepository())
                .ignoringAntMatchers("/api/public/**")
            
            // 配置安全头
            .headers()
                .contentSecurityPolicy("default-src 'self'")
                .and()
                .frameOptions().deny()
            
            // 配置Session管理
            .sessionManagement()
                .sessionFixation().migrateSession()
                .maximumSessions(1)
                .maxSessionsPreventsLogin(true)
                .and()
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
            
            // 配置异常处理
            .exceptionHandling()
                .authenticationEntryPoint(new CustomAuthenticationEntryPoint())
                .accessDeniedHandler(new CustomAccessDeniedHandler())
            
            // 配置授权
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            
            // 配置登录
            .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/home")
                .failureHandler(authenticationFailureHandler())
                .successHandler(authenticationSuccessHandler())
            
            // 配置登出
            .logout()
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login?logout")
                .addLogoutHandler(new CustomLogoutHandler())
            
            // 配置Remember Me
            .rememberMe()
                .key("uniqueAndSecret")
                .tokenRepository(persistentTokenRepository())
                .tokenValiditySeconds(86400);
    }
    
    @Bean
    public CsrfTokenRepository customCsrfTokenRepository() {
        HttpSessionCsrfTokenRepository repository = new HttpSessionCsrfTokenRepository();
        repository.setHeaderName("X-CSRF-TOKEN");
        return repository;
    }
}
```

#### 3.3 自定义过滤器示例

```java
/**
 * 自定义日志过滤器
 */
public class CustomLoggingFilter extends OncePerRequestFilter {
    
    private static final Logger logger = LoggerFactory.getLogger(CustomLoggingFilter.class);
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        
        long startTime = System.currentTimeMillis();
        
        // 记录请求信息
        logger.info("Request: {} {}", request.getMethod(), request.getRequestURI());
        
        try {
            filterChain.doFilter(request, response);
        } finally {
            long duration = System.currentTimeMillis() - startTime;
            logger.info("Response: {} {} - Status: {} - Duration: {}ms",
                request.getMethod(), request.getRequestURI(),
                response.getStatus(), duration);
        }
    }
}

/**
 * 自定义认证过滤器
 */
public class CustomAuthenticationFilter extends
        AbstractAuthenticationProcessingFilter {
    
    public CustomAuthenticationFilter() {
        super(new AntPathRequestMatcher("/api/auth/login", "POST"));
    }
    
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request,
            HttpServletResponse response) throws AuthenticationException {
        
        // 从请求体中读取JSON
        ObjectMapper mapper = new ObjectMapper();
        LoginRequest loginRequest;
        try {
            loginRequest = mapper.readValue(request.getInputStream(), 
                LoginRequest.class);
        } catch (IOException e) {
            throw new AuthenticationServiceException("Invalid login request", e);
        }
        
        // 创建认证Token
        UsernamePasswordAuthenticationToken authToken = 
            new UsernamePasswordAuthenticationToken(
                loginRequest.getUsername(),
                loginRequest.getPassword()
            );
        
        // 设置详细信息
        authToken.setDetails(authenticationDetailsSource.buildDetails(request));
        
        // 委托给AuthenticationManager
        return this.getAuthenticationManager().authenticate(authToken);
    }
    
    public static class LoginRequest {
        private String username;
        private String password;
        
        // getters and setters
    }
}
```

### 4. FAQ

**Q1: 如何在过滤器链中插入自定义过滤器？**

A: Spring Security提供了三种方法：

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        // 1. 在指定过滤器之前添加
        .addFilterBefore(new MyBeforeFilter(), 
            UsernamePasswordAuthenticationFilter.class)
        
        // 2. 在指定过滤器之后添加
        .addFilterAfter(new MyAfterFilter(), 
            ExceptionTranslationFilter.class)
        
        // 3. 替换指定过滤器
        .addFilterAt(new MyReplacementFilter(), 
            UsernamePasswordAuthenticationFilter.class);
}
```

选择建议：
- `addFilterBefore`: 需要在某个过滤器之前执行
- `addFilterAfter`: 需要在某个过滤器之后执行
- `addFilterAt`: 替换默认过滤器

**Q2: SecurityContextPersistenceFilter和SessionManagementFilter有什么区别？**

A: 
- **SecurityContextPersistenceFilter**：负责在请求开始时加载SecurityContext（从Session或Repository），请求结束时保存SecurityContext
- **SessionManagementFilter**：负责会话管理，包括Session固定攻击防护、并发会话控制、Session超时检测

**Q3: 如何调试过滤器链的执行顺序？**

A: 可以通过日志查看：

```yaml
# application.yml
logging:
  level:
    org.springframework.security: DEBUG
```

或者编程方式：

```java
@Configuration
public class SecurityDebugConfig {
    
    @Bean
    public FilterRegistrationBean<FilterChainProxy> filterChainProxyFilter(
            FilterChainProxy filterChainProxy) {
        
        FilterRegistrationBean<FilterChainProxy> registration = new FilterRegistrationBean<>();
        registration.setFilter(filterChainProxy);
        registration.addUrlPatterns("/*");
        registration.setOrder(FilterChainProxy.DEFAULT_ORDER);
        registration.setName("springSecurityFilterChain");
        
        // 打印所有过滤器
        List<SecurityFilterChain> filterChains = filterChainProxy.getFilterChains();
        for (SecurityFilterChain chain : filterChains) {
            List<Filter> filters = chain.getFilters();
            System.out.println("Filter Chain:");
            for (int i = 0; i < filters.size(); i++) {
                System.out.println((i + 1) + ". " + filters.get(i).getClass().getName());
            }
        }
        
        return registration;
    }
}
```

**Q4: 如何实现多租户的过滤器链？**

A: 使用多个SecurityFilterChain：

```java
@Configuration
public class MultiTenantSecurityConfig {
    
    @Bean
    public SecurityFilterChain tenant1FilterChain(HttpSecurity http) throws Exception {
        http
            .antMatcher("/tenant1/**")
            .authorizeRequests()
                .antMatchers("/tenant1/public/**").permitAll()
                .anyRequest().hasRole("TENANT1_USER")
            .and()
            .formLogin()
                .loginPage("/tenant1/login");
        
        return http.build();
    }
    
    @Bean
    public SecurityFilterChain tenant2FilterChain(HttpSecurity http) throws Exception {
        http
            .antMatcher("/tenant2/**")
            .authorizeRequests()
                .antMatchers("/tenant2/public/**").permitAll()
                .anyRequest().hasRole("TENANT2_USER")
            .and()
            .formLogin()
                .loginPage("/tenant2/login");
        
        return http.build();
    }
    
    @Bean
    public SecurityFilterChain defaultFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
            .and()
            .formLogin();
        
        return http.build();
    }
}
```

**Q5: 过滤器链的性能如何优化？**

A: 优化建议：

1. **减少不必要的过滤器**：移除不使用的功能对应的过滤器
2. **使用高效的匹配器**：将最常访问的URL放在前面
3. **缓存权限配置**：避免每次请求都从数据库查询
4. **异步处理**：将耗时操作放到后台线程
5. **使用Spring AOP缓存**：缓存频繁调用的方法

```java
@Configuration
@EnableWebSecurity
@EnableCaching
public class PerformanceSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // 禁用不必要的功能
            .csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            
            // 优化URL匹配顺序
            .authorizeRequests()
                .antMatchers("/api/public/**").permitAll()
                .antMatchers("/api/user/**").hasRole("USER")
                .antMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated();
    }
    
    // 缓存权限配置
    @Bean
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager("securityConfig");
    }
}
```

### 5. 思考题

1. 设计一个支持动态添加/移除过滤器的Security框架，需要考虑哪些问题？

2. 在微服务架构中，如何在网关层统一处理认证，而在服务层处理授权？

3. 如何实现一个基于请求频率的过滤器，防止暴力破解攻击？

4. 分析Spring Security过滤器链的设计模式，这种设计的优缺点是什么？

5. 如果要自己实现一个安全框架，过滤器链应该如何设计？

---

## 知识点11：表达式式访问控制

### 1. 表达式式访问控制概述

Spring Security 3.0引入了对Spring EL表达式的支持，允许在授权控制中使用表达式语言，这大大增强了授权的灵活性和表达能力。

### 2. 安全表达式基础

#### 2.1 表达式根对象

所有安全表达式都是基于`SecurityExpressionRoot`类的，它提供了大量的内置方法和属性。

```java
public abstract class SecurityExpressionRoot implements SecurityExpressionOperations {
    
    protected final Authentication authentication;
    
    // 常用的内置方法和属性
    public final boolean permitAll = true;                    // 允许所有访问
    public final boolean denyAll = false;                     // 拒绝所有访问
    
    // 认证相关
    public boolean isAnonymous();                             // 是否是匿名用户
    public boolean isRememberMe();                            // 是否是RememberMe用户
    public boolean isAuthenticated();                         // 是否已认证
    public boolean isFullyAuthenticated();                    // 是否是完全认证（非匿名、非RememberMe
    
    // 权限和角色检查
    public boolean hasRole(String role);                      // 是否有指定角色
    public boolean hasAnyRole(String... roles);               // 是否有任一角色
    public boolean hasAuthority(String authority);            // 是否有指定权限
    public boolean hasAnyAuthority(String... authorities);    // 是否有任一权限
    
    // 认证对象
    public Object getPrincipal();                             // 获取主体对象
    public Object getCredentials();                           // 获取凭证
    
    // 其他
    public boolean permitAll();                               // 允许所有
    public boolean denyAll();                                 // 拒绝所有
    public boolean isAnonymous();                             // 是否匿名
    public boolean isRememberMe();                            // 是否RememberMe
    public boolean isAuthenticated();                         // 是否已认证
    public boolean isFullyAuthenticated();                    // 是否完全认证
    
    // 判断是否有指定权限
    public boolean hasPermission(Object target, Object permission);
    public boolean hasPermission(Object targetId, String targetType, Object permission);
}
```

#### 2.2 Web安全表达式

`WebSecurityExpressionRoot`扩展了基础表达式，添加了HTTP请求相关的功能。

```java
public class WebSecurityExpressionRoot extends SecurityExpressionRoot {
    
    private FilterInvocation filterInvocation;
    
    public boolean hasIpAddress(String ipAddress);            // 检查IP地址
    
    // 访问请求对象
    public HttpServletRequest getRequest();                   // 获取HttpServletRequest
    public HttpServletResponse getResponse();                 // 获取HttpServletResponse
}
```

### 3. 使用表达式进行URL安全配置

#### 3.1 基本用法

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                // 使用表达式
                .antMatchers("/admin/**").access("hasRole('ADMIN')")
                .antMatchers("/user/**").access("hasRole('USER') and hasIpAddress('192.168.1.0/24')")
                .antMatchers("/public/**").permitAll()
                .antMatchers("/api/**").access("isAuthenticated()")
                .anyRequest().access("isFullyAuthenticated()")
                .and()
            .formLogin();
    }
}
```

#### 3.2 复杂表达式示例

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            // 复合条件
            .antMatchers("/special/**").access("hasRole('ADMIN') or hasRole('SUPERVISOR')")
            
            // 多重条件
            .antMatchers("/sensitive/**").access("hasRole('ADMIN') and hasIpAddress('192.168.1.0/24') and isFullyAuthenticated()")
            
            // 使用自定义bean方法
            .antMatchers("/documents/**").access("@documentService.canAccess(#request)")
            
            // 结合IP和角色
            .antMatchers("/internal/**").access("hasRole('INTERNAL_USER') and (hasIpAddress('10.0.0.0/8') or hasIpAddress('192.168.0.0/16'))")
            
            // 表达式中的三元运算符
            .antMatchers("/conditional/**").access("T(java.time.LocalTime).now().hour >= 9 ? hasRole('DAY_USER') : hasRole('NIGHT_USER')")
            
            .anyRequest().authenticated();
}
```

### 4. 方法级安全表达式

#### 4.1 启用方法级安全

```java
@Configuration
@EnableGlobalMethodSecurity(
    prePostEnabled = true,    // 启用@PreAuthorize和@PostAuthorize
    securedEnabled = true,    // 启用@Secured
    jsr250Enabled = true      // 启用JSR-250注解
)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
    
    @Override
    protected MethodSecurityExpressionHandler createExpressionHandler() {
        DefaultMethodSecurityExpressionHandler handler = 
            new DefaultMethodSecurityExpressionHandler();
        handler.setPermissionEvaluator(new CustomPermissionEvaluator());
        return handler;
    }
}
```

#### 4.2 @PreAuthorize注解

在方法执行前进行授权检查。

```java
@Service
public class DocumentService {
    
    @Autowired
    private DocumentRepository documentRepository;
    
    // 简单角色检查
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteDocument(Long id) {
        documentRepository.deleteById(id);
    }
    
    // 复合条件
    @PreAuthorize("hasRole('USER') and hasRole('EDITOR')")
    public void updateDocument(Long id, Document document) {
        documentRepository.save(document);
    }
    
    // 访问方法参数
    @PreAuthorize("#document.owner == authentication.name")
    public void updateDocument(Document document) {
        documentRepository.save(document);
    }
    
    // 复杂表达式
    @PreAuthorize("hasRole('ADMIN') or (hasRole('USER') and #document.owner == authentication.name)")
    public void modifyDocument(Document document) {
        documentRepository.save(document);
    }
    
    // 调用自定义Bean的方法
    @PreAuthorize("@documentPermissionEvaluator.canAccess(#id, authentication.name)")
    public Document getDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    // 组合多个条件
    @PreAuthorize("hasAnyRole('ADMIN', 'MANAGER') and @securityService.isWorkingHours()")
    public void performSensitiveOperation() {
        // 敏感操作
    }
    
    // 检查集合参数
    @PreAuthorize("hasRole('ADMIN') or #ids.size() <= 10")
    public List<Document> getDocuments(List<Long> ids) {
        return documentRepository.findAllById(ids);
    }
    
    // 使用返回对象（实际是在方法执行后检查，但语法上放在这里）
    @PreAuthorize("returnObject.owner == authentication.name")
    public Document findDocument(Long id) {
        return documentRepository.findById(id);
    }
}
```

#### 4.3 @PostAuthorize注解

在方法执行后进行授权检查，可以访问方法的返回值。

```java
@Service
public class DocumentService {
    
    // 访问返回值
    @PostAuthorize("returnObject.owner == authentication.name")
    public Document getDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    // 复杂的返回值检查
    @PostAuthorize("returnObject.status == 'PUBLISHED' or returnObject.owner == authentication.name")
    public Document viewDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    // 集合返回值过滤
    @PostAuthorize("filter(returnObject, owner == authentication.name).size() > 0")
    public List<Document> getDocumentsByOwner(String owner) {
        return documentRepository.findByOwner(owner);
    }
    
    // 结合自定义逻辑
    @PostAuthorize("@permissionEvaluator.canViewResult(returnObject, authentication.name)")
    public Report generateReport(Long id) {
        return reportService.generate(id);
    }
}
```

#### 4.4 @PreFilter和@PostFilter

过滤集合类型的参数或返回值。

```java
@Service
public class DocumentService {
    
    // 过滤输入参数 - 只保留ID为偶数的文档
    @PreFilter("filterObject.id % 2 == 0")
    public void batchUpdate(List<Document> documents) {
        documentRepository.saveAll(documents);
    }
    
    // 过滤输入参数 - 只保留用户有权限的文档
    @PreFilter("hasRole('ADMIN') or filterObject.owner == authentication.name")
    public void batchDelete(List<Document> documents) {
        documentRepository.deleteAll(documents);
    }
    
    // 过滤返回值 - 只返回用户有权限查看的文档
    @PostFilter("hasRole('ADMIN') or filterObject.owner == authentication.name")
    public List<Document> getAllDocuments() {
        return documentRepository.findAll();
    }
    
    // 复杂的过滤条件
    @PostFilter("hasRole('ADMIN') or (filterObject.status == 'PUBLISHED' and filterObject.category == authentication.principal.preferredCategory)")
    public List<Document> getPublishedDocuments() {
        return documentRepository.findByStatus("PUBLISHED");
    }
    
    // 集合变量名自定义
    @PreFilter("targetDocumentList.size() > 0 and (hasRole('ADMIN') or filterObject.owner == authentication.name)")
    public void processDocuments(@Param("targetDocumentList") List<Document> documents) {
        // 处理文档
    }
}
```

### 5. 自定义安全表达式

#### 5.1 自定义PermissionEvaluator

```java
@Component
public class CustomPermissionEvaluator implements PermissionEvaluator {
    
    @Autowired
    private PermissionService permissionService;
    
    /**
     * 检查是否有权限
     * @param authentication 认证信息
     * @param targetId 目标对象ID
     * @param permission 权限类型
     */
    @Override
    public boolean hasPermission(Authentication authentication, 
            Object targetId, Object permission) {
        
        if (authentication == null) {
            return false;
        }
        
        String username = authentication.getName();
        String resource = targetId.toString();
        String action = permission.toString();
        
        return permissionService.hasPermission(username, resource, action);
    }
    
    /**
     * 检查是否有权限（带类型）
     */
    @Override
    public boolean hasPermission(Authentication authentication, 
            Serializable targetId, String targetType, Object permission) {
        
        return permissionService.hasPermission(
            authentication.getName(),
            targetId,
            targetType,
            permission.toString()
        );
    }
}

// 配置
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
    
    @Autowired
    private CustomPermissionEvaluator customPermissionEvaluator;
    
    @Override
    protected MethodSecurityExpressionHandler createExpressionHandler() {
        DefaultMethodSecurityExpressionHandler handler = 
            new DefaultMethodSecurityExpressionHandler();
        handler.setPermissionEvaluator(customPermissionEvaluator);
        return handler;
    }
}

// 使用
@Service
public class DocumentService {
    
    @PreAuthorize("hasPermission(#id, 'READ')")
    public Document getDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    @PreAuthorize("hasPermission(#id, 'Document', 'WRITE')")
    public void updateDocument(Long id, Document document) {
        documentRepository.save(document);
    }
}
```

#### 5.2 自定义SecurityExpressionHandler

```java
public class CustomSecurityExpressionHandler extends
        DefaultMethodSecurityExpressionHandler {
    
    @Override
    protected MethodSecurityExpressionOperations createSecurityExpressionRoot(
            Authentication authentication, MethodInvocation invocation) {
        
        CustomMethodSecurityExpressionRoot root = 
            new CustomMethodSecurityExpressionRoot(authentication);
        root.setPermissionEvaluator(getPermissionEvaluator());
        root.setTrustResolver(getTrustResolver());
        root.setRoleHierarchy(getRoleHierarchy());
        root.setThis(invocation.getThis());
        
        return root;
    }
}

public class CustomMethodSecurityExpressionRoot extends 
        MethodSecurityExpressionRoot {
    
    public CustomMethodSecurityExpressionRoot(Authentication authentication) {
        super(authentication);
    }
    
    // 自定义方法
    public boolean isOwner(Long resourceId) {
        String username = authentication.getName();
        return resourceService.isOwner(resourceId, username);
    }
    
    public boolean hasPermission(String resource, String action) {
        String username = authentication.getName();
        return permissionService.hasPermission(username, resource, action);
    }
    
    public boolean isWorkingTime() {
        LocalTime now = LocalTime.now();
        return now.isAfter(LocalTime.of(9, 0)) && 
               now.isBefore(LocalTime.of(18, 0));
    }
    
    public boolean isFromInternalNetwork() {
        // 假设从SecurityContext获取请求信息
        HttpServletRequest request = 
            ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes())
                .getRequest();
        String ip = request.getRemoteAddr();
        return ip.startsWith("192.168.") || ip.startsWith("10.");
    }
}

// 配置
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
    
    @Override
    protected MethodSecurityExpressionHandler createExpressionHandler() {
        CustomSecurityExpressionHandler handler = new CustomSecurityExpressionHandler();
        handler.setPermissionEvaluator(new CustomPermissionEvaluator());
        return handler;
    }
}

// 使用自定义表达式
@Service
public class DocumentService {
    
    @PreAuthorize("isOwner(#id)")
    public Document getDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    @PreAuthorize("hasPermission('document', 'write') and isWorkingTime()")
    public void updateDocument(Long id, Document document) {
        documentRepository.save(document);
    }
    
    @PreAuthorize("hasRole('ADMIN') or (isFromInternalNetwork() and isOwner(#id))")
    public void deleteDocument(Long id) {
        documentRepository.deleteById(id);
    }
}
```

#### 5.3 自定义Web安全表达式

```java
public class CustomWebSecurityExpressionHandler extends
        DefaultWebSecurityExpressionHandler {
    
    @Override
    protected SecurityExpressionOperations createSecurityExpressionRoot(
            Authentication authentication, FilterInvocation fi) {
        
        CustomWebSecurityExpressionRoot root = 
            new CustomWebSecurityExpressionRoot(authentication, fi);
        root.setPermissionEvaluator(getPermissionEvaluator());
        root.setTrustResolver(getTrustResolver());
        root.setRoleHierarchy(getRoleHierarchy());
        
        return root;
    }
}

public class CustomWebSecurityExpressionRoot extends WebSecurityExpressionRoot {
    
    public CustomWebSecurityExpressionRoot(Authentication authentication, 
            FilterInvocation fi) {
        super(authentication, fi);
    }
    
    // 检查是否是移动设备
    public boolean isMobile() {
        HttpServletRequest request = ((FilterInvocation) getFilterInvocation()).getRequest();
        String userAgent = request.getHeader("User-Agent");
        return userAgent != null && userAgent.contains("Mobile");
    }
    
    // 检查请求频率
    public boolean isUnderRateLimit(String endpoint) {
        HttpServletRequest request = ((FilterInvocation) getFilterInvocation()).getRequest();
        String clientId = request.getHeader("X-Client-ID");
        return rateLimitService.isAllowed(clientId, endpoint);
    }
    
    // 检查API版本
    public boolean apiVersion(String version) {
        HttpServletRequest request = ((FilterInvocation) getFilterInvocation()).getRequest();
        String requestVersion = request.getHeader("X-API-Version");
        return version.equals(requestVersion);
    }
}

// 配置
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        CustomWebSecurityExpressionHandler expressionHandler = 
            new CustomWebSecurityExpressionHandler();
        
        http
            .authorizeRequests()
                .expressionHandler(expressionHandler)
                .antMatchers("/api/v2/**").access("apiVersion('2') and isMobile()")
                .antMatchers("/api/rate-limited/**").access("isUnderRateLimit('/api/rate-limited')")
                .anyRequest().authenticated();
    }
}
```

### 6. 高级表达式技巧

#### 6.1 表达式中的三元运算符

```java
@Service
public class DocumentService {
    
    // 根据时间判断角色
    @PreAuthorize("T(java.time.LocalTime).now().hour >= 9 ? hasRole('DAY_USER') : hasRole('NIGHT_USER')")
    public void performOperation() {
        // 业务逻辑
    }
    
    // 根据参数选择不同的权限检查
    @PreAuthorize("#isPublic ? hasRole('GUEST') : hasRole('USER')")
    public Document getDocument(Long id, boolean isPublic) {
        return documentRepository.findById(id);
    }
    
    // 复杂的条件判断
    @PreAuthorize("#document.type == 'PUBLIC' ? true : (#document.type == 'PROTECTED' ? hasRole('USER') : hasRole('ADMIN'))")
    public Document viewDocument(Document document) {
        return documentRepository.findById(document.getId());
    }
}
```

#### 6.2 集合操作表达式

```java
@Service
public class DocumentService {
    
    // 检查集合是否包含特定元素
    @PreAuthorize("#userIds.contains(authentication.principal.id)")
    public void notifyUsers(List<Long> userIds, String message) {
        // 通知用户
    }
    
    // 检查集合大小
    @PreAuthorize("#documents.size() <= 10")
    public void batchProcess(List<Document> documents) {
        // 批量处理
    }
    
    // 结合过滤和集合操作
    @PreFilter("hasRole('ADMIN') or filterObject.owner == authentication.name")
    @PreAuthorize("#documents.size() > 0")
    public void processDocuments(List<Document> documents) {
        // 处理文档
    }
    
    // 检查集合中的所有元素是否满足条件
    @PreAuthorize("hasRole('ADMIN') or #documents.?[owner == authentication.name].size() == #documents.size()")
    public void updateAllDocuments(List<Document> documents) {
        // 更新所有文档
    }
}
```

#### 6.3 使用静态方法

```java
@Service
public class DocumentService {
    
    // 使用静态工具类
    @PreAuthorize("@securityUtils.isValidUser(#username)")
    public void createUser(String username, String password) {
        // 创建用户
    }
    
    // 使用T()操作符访问类
    @PreAuthorize("T(java.time.DayOfWeek).from(T(java.time.LocalDate).now()) != T(java.time.DayOfWeek).SUNDAY")
    public void businessOperation() {
        // 工作日才能执行的操作
    }
    
    // 访问枚举
    @PreAuthorize("#status == T(com.example.DocumentStatus).PUBLISHED")
    public Document getPublishedDocument(Long id, DocumentStatus status) {
        return documentRepository.findByIdAndStatus(id, status);
    }
}
```

### 7. 表达式性能优化

#### 7.1 缓存表达式解析结果

```java
@Configuration
public class ExpressionHandlerConfig {
    
    @Bean
    public MethodSecurityExpressionHandler methodSecurityExpressionHandler() {
        DefaultMethodSecurityExpressionHandler handler = 
            new DefaultMethodSecurityExpressionHandler();
        
        // 配置缓存
        handler.setExpressionParser(
            new SpelExpressionParser(
                new SpelParserConfiguration(
                    SpelCompilerMode.IMMEDIATE,   // 启用编译模式
                    this.getClass().getClassLoader()
                )
            )
        );
        
        return handler;
    }
}
```

#### 7.2 避免复杂的表达式

```java
// 不推荐：过于复杂的表达式
@PreAuthorize("(hasRole('ADMIN') or hasRole('MANAGER')) and (isWorkingTime() or isEmergency()) and (@permissionService.canAccess(#id) or @permissionService.hasDelegation(authentication.name, #id))")
public Document getDocument(Long id) {
    // ...
}

// 推荐：拆分为多个方法
@PreAuthorize("hasRole('ADMIN') or hasRole('MANAGER')")
@PreAuthorize("isWorkingTime() or isEmergency()")
@PreAuthorize("@permissionService.canAccess(#id) or @permissionService.hasDelegation(authentication.name, #id)")
public Document getDocument(Long id) {
    // ...
}

// 或者提取到自定义方法
@PreAuthorize("@securityService.canAccessDocument(#id, authentication.name)")
public Document getDocument(Long id) {
    // ...
}
```

### 8. FAQ

**Q1: @PreAuthorize和@Secured有什么区别？**

A: 
- **@PreAuthorize**：支持Spring EL表达式，功能更强大
- **@Secured**：只支持简单的角色名称，不支持表达式

```java
// @Secured - 只支持简单角色
@Secured("ROLE_ADMIN")
public void adminOnly() {
    // ...
}

// @PreAuthorize - 支持表达式
@PreAuthorize("hasRole('ADMIN') or hasRole('SUPERADMIN')")
public void adminOrSuperadmin() {
    // ...
}

@PreAuthorize("#user.id == authentication.principal.id")
public void updateProfile(User user) {
    // ...
}
```

**Q2: 表达式中如何访问HTTP请求信息？**

A: 在WebSecurityExpressionRoot中可以直接访问：

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/api/**")
                    .access("hasRole('API_USER') and request.getHeader('X-API-KEY') != null")
                .antMatchers("/mobile/**")
                    .access("isMobile() and hasIpAddress('192.168.1.0/24')");
    }
}
```

在方法级安全中，可以通过RequestContextHolder获取：

```java
@Service
public class DocumentService {
    
    @PreAuthorize("@securityService.isValidRequest(request)")
    public Document getDocument(Long id) {
        HttpServletRequest request = 
            ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes())
                .getRequest();
        // ...
    }
}
```

**Q3: 如何测试带表达式的安全注解？**

A: 使用Spring Security Test支持：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@WithMockUser(username = "admin", roles = {"ADMIN"})
public class DocumentServiceTest {
    
    @Autowired
    private DocumentService documentService;
    
    @Test
    public void testAdminCanDelete() {
        documentService.deleteDocument(1L);  // 应该成功
    }
    
    @Test(expected = AccessDeniedException.class)
    @WithMockUser(username = "user", roles = {"USER"})
    public void testUserCannotDelete() {
        documentService.deleteDocument(1L);  // 应该抛出异常
    }
    
    @Test
    @WithAnonymousUser
    public void testAnonymousCannotAccess() {
        // 匿名用户无法访问
    }
    
    @Test
    @WithUserDetails(value = "customUser", userDetailsServiceBeanName = "myUserDetailsService")
    public void testWithCustomUser() {
        // 使用自定义UserDetailsService加载用户
    }
}
```

**Q4: 表达式性能会不会成为问题？**

A: 简单表达式性能影响很小，但需要注意：

1. **启用表达式编译**：使用`SpelCompilerMode.IMMEDIATE`
2. **避免数据库查询**：在表达式中直接查询数据库会影响性能
3. **使用缓存**：对频繁查询的结果进行缓存
4. **简化表达式**：复杂的表达式可以提取到Service方法中

```java
// 不推荐
@PreAuthorize("@databaseService.checkPermission(authentication.name, #id)")
public Document getDocument(Long id) {
    // 每次调用都会查询数据库
}

// 推荐 - 使用缓存
@Cacheable("permissions")
@PreAuthorize("@permissionService.hasPermission(authentication.name, #id)")
public Document getDocument(Long id) {
    // ...
}
```

**Q5: 如何在表达式中访问Spring Bean？**

A: 使用`@beanName`语法：

```java
@Service("securityHelper")
public class SecurityHelper {
    
    public boolean canAccessDocument(Long id, String username) {
        return documentService.isOwner(id, username);
    }
    
    public boolean isBusinessHours() {
        LocalTime now = LocalTime.now();
        return now.isAfter(LocalTime.of(9, 0)) && 
               now.isBefore(LocalTime.of(18, 0));
    }
}

// 使用
@Service
public class DocumentService {
    
    @PreAuthorize("@securityHelper.canAccessDocument(#id, authentication.name)")
    public Document getDocument(Long id) {
        return documentRepository.findById(id);
    }
    
    @PreAuthorize("@securityHelper.isBusinessHours()")
    public void businessOperation() {
        // ...
    }
}
```

### 9. 思考题

1. 如何设计一个基于动态规则的表达式引擎，支持运行时修改权限规则？

2. 在分布式系统中，如何保证方法级安全表达式的一致性？

3. 如何实现一个支持复杂权限继承关系（如部门、组织架构）的表达式系统？

4. 比较Spring Security表达式和XACML（eXtensible Access Control Markup Language）的优劣。

5. 设计一个基于表达式访问控制的审计日志系统，记录所有的权限检查过程。

---

## 知识点12：基于角色的访问控制（RBAC）

### 1. RBAC概述

基于角色的访问控制（Role-Based Access Control，RBAC）是一种广泛使用的访问控制模型。它将权限分配给角色，然后将角色分配给用户，从而简化了权限管理。

Spring Security通过`GrantedAuthority`接口和`UserDetails`实现来支持RBAC模型。

### 2. RBAC核心概念

#### 2.1 基本模型

```
用户（User）
  ↓ 被分配
角色（Role）
  ↓ 拥有
权限（Permission）
  ↓ 控制
资源（Resource）
```

#### 2.2 Spring Security中的角色

在Spring Security中，角色本质上是一种特殊的权限，通常以"ROLE_"前缀标识。

```java
// 用户信息接口
public interface UserDetails extends Serializable {
    
    // 获取用户的权限集合（包括角色）
    Collection<? extends GrantedAuthority> getAuthorities();
    
    String getPassword();
    String getUsername();
    boolean isAccountNonExpired();
    boolean isAccountNonLocked();
    boolean isCredentialsNonExpired();
    boolean isEnabled();
}

// 权限接口
public interface GrantedAuthority extends Serializable {
    
    // 获取权限字符串（如"ROLE_ADMIN", "READ_PERMISSION"等）
    String getAuthority();
}
```

### 3. 实现RBAC系统

#### 3.1 数据库设计

```sql
-- 用户表
CREATE TABLE user (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    enabled BOOLEAN DEFAULT TRUE,
    account_non_expired BOOLEAN DEFAULT TRUE,
    account_non_locked BOOLEAN DEFAULT TRUE,
    credentials_non_expired BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 角色表
CREATE TABLE role (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL UNIQUE,
    description VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 权限表
CREATE TABLE permission (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL UNIQUE,
    description VARCHAR(200),
    resource_type VARCHAR(50),
    resource_id VARCHAR(50),
    action VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 用户-角色关联表
CREATE TABLE user_role (
    user_id BIGINT NOT NULL,
    role_id BIGINT NOT NULL,
    PRIMARY KEY (user_id, role_id),
    FOREIGN KEY (user_id) REFERENCES user(id),
    FOREIGN KEY (role_id) REFERENCES role(id)
);

-- 角色-权限关联表
CREATE TABLE role_permission (
    role_id BIGINT NOT NULL,
    permission_id BIGINT NOT NULL,
    PRIMARY KEY (role_id, permission_id),
    FOREIGN KEY (role_id) REFERENCES role(id),
    FOREIGN KEY (permission_id) REFERENCES permission(id)
);

-- 插入示例数据
INSERT INTO role (name, description) VALUES 
    ('ROLE_ADMIN', '系统管理员'),
    ('ROLE_MANAGER', '部门经理'),
    ('ROLE_USER', '普通用户');

INSERT INTO permission (name, description, resource_type, action) VALUES 
    ('USER_READ', '查看用户', 'USER', 'READ'),
    ('USER_WRITE', '编辑用户', 'USER', 'WRITE'),
    ('USER_DELETE', '删除用户', 'USER', 'DELETE'),
    ('DOCUMENT_READ', '查看文档', 'DOCUMENT', 'READ'),
    ('DOCUMENT_WRITE', '编辑文档', 'DOCUMENT', 'WRITE');

INSERT INTO role_permission (role_id, permission_id) VALUES
    -- 管理员拥有所有权限
    (1, 1), (1, 2), (1, 3), (1, 4), (1, 5),
    -- 经理有文档相关权限
    (2, 4), (2, 5),
    -- 普通用户只有查看权限
    (3, 1), (3, 4);
```

#### 3.2 实体类

```java
// 用户实体
@Entity
@Table(name = "user")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String password;
    
    private String email;
    
    private Boolean enabled = true;
    
    private Boolean accountNonExpired = true;
    
    private Boolean accountNonLocked = true;
    
    private Boolean credentialsNonExpired = true;
    
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_role",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
    
    @CreationTimestamp
    private LocalDateTime createdAt;
    
    @UpdateTimestamp
    private LocalDateTime updatedAt;
    
    // 转换为UserDetails
    public UserDetails toUserDetails() {
        return new CustomUserDetails(
            username,
            password,
            enabled,
            accountNonExpired,
            accountNonLocked,
            credentialsNonExpired,
            getAuthorities()
        );
    }
    
    // 获取所有权限（角色+权限）
    public Collection<GrantedAuthority> getAuthorities() {
        Set<GrantedAuthority> authorities = new HashSet<>();
        
        // 添加角色
        for (Role role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getName()));
            
            // 添加角色对应的权限
            for (Permission permission : role.getPermissions()) {
                authorities.add(new SimpleGrantedAuthority(permission.getName()));
            }
        }
        
        return authorities;
    }
    
    // getters and setters
}

// 角色实体
@Entity
@Table(name = "role")
public class Role {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String name;
    
    private String description;
    
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "role_permission",
        joinColumns = @JoinColumn(name = "role_id"),
        inverseJoinColumns = @JoinColumn(name = "permission_id")
    )
    private Set<Permission> permissions = new HashSet<>();
    
    @CreationTimestamp
    private LocalDateTime createdAt;
    
    @ManyToMany(mappedBy = "roles")
    private Set<User> users = new HashSet<>();
    
    // getters and setters
}

// 权限实体
@Entity
@Table(name = "permission")
public class Permission {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String name;
    
    private String description;
    
    private String resourceType;
    
    private String resourceId;
    
    private String action;
    
    @CreationTimestamp
    private LocalDateTime createdAt;
    
    @ManyToMany(mappedBy = "permissions")
    private Set<Role> roles = new HashSet<>();
    
    // getters and setters
}
```

#### 3.3 自定义UserDetails

```java
public class CustomUserDetails implements UserDetails {
    
    private String username;
    private String password;
    private boolean enabled;
    private boolean accountNonExpired;
    private boolean accountNonLocked;
    private boolean credentialsNonExpired;
    private Collection<? extends GrantedAuthority> authorities;
    
    public CustomUserDetails(String username, String password, boolean enabled,
            boolean accountNonExpired, boolean accountNonLocked,
            boolean credentialsNonExpired,
            Collection<? extends GrantedAuthority> authorities) {
        
        this.username = username;
        this.password = password;
        this.enabled = enabled;
        this.accountNonExpired = accountNonExpired;
        this.accountNonLocked = accountNonLocked;
        this.credentialsNonExpired = credentialsNonExpired;
        this.authorities = authorities;
    }
    
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }
    
    @Override
    public String getPassword() {
        return password;
    }
    
    @Override
    public String getUsername() {
        return username;
    }
    
    @Override
    public boolean isAccountNonExpired() {
        return accountNonExpired;
    }
    
    @Override
    public boolean isAccountNonLocked() {
        return accountNonLocked;
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        return credentialsNonExpired;
    }
    
    @Override
    public boolean isEnabled() {
        return enabled;
    }
}
```

#### 3.4 自定义UserDetailsService

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> 
                new UsernameNotFoundException("User not found: " + username)
            );
        
        return user.toUserDetails();
    }
}
```

#### 3.5 Repository

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    Optional<User> findByUsername(String username);
}

@Repository
public interface RoleRepository extends JpaRepository<Role, Long> {
    
    Optional<Role> findByName(String name);
}

@Repository
public interface PermissionRepository extends JpaRepository<Permission, Long> {
    
    Optional<Permission> findByName(String name);
}
```

### 4. 使用RBAC

#### 4.1 HTTP安全配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Autowired
    private CustomUserDetailsService userDetailsService;
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth
            .userDetailsService(userDetailsService)
            .passwordEncoder(passwordEncoder());
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                // 基于角色的访问控制
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/manager/**").hasAnyRole("ADMIN", "MANAGER")
                .antMatchers("/user/**").hasAnyRole("ADMIN", "MANAGER", "USER")
                
                // 基于权限的访问控制
                .antMatchers("/users/**").hasAuthority("USER_READ")
                .antMatchers("/users/new").hasAuthority("USER_WRITE")
                .antMatchers("/users/*/edit").hasAuthority("USER_WRITE")
                .antMatchers("/users/*/delete").hasAuthority("USER_DELETE")
                
                // 公开访问
                .antMatchers("/public/**", "/home", "/about").permitAll()
                
                // 其他需要认证
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .permitAll()
                .and()
            .logout()
                .logoutSuccessUrl("/login?logout")
                .permitAll();
    }
}
```

#### 4.2 方法级安全

```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    // 基于角色
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        userRepository.deleteById(userId);
    }
    
    // 基于权限
    @PreAuthorize("hasAuthority('USER_READ')")
    public User getUser(Long userId) {
        return userRepository.findById(userId).orElse(null);
    }
    
    // 复合条件
    @PreAuthorize("hasRole('ADMIN') or (hasRole('MANAGER') and hasAuthority('USER_WRITE'))")
    public void updateUser(User user) {
        userRepository.save(user);
    }
    
    // 返回对象过滤
    @PostFilter("hasRole('ADMIN') or filterObject.username == authentication.name")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    // 参数过滤
    @PreFilter("hasRole('ADMIN') or filterObject.username == authentication.name")
    public void batchUpdateUsers(List<User> users) {
        userRepository.saveAll(users);
    }
}

@Service
public class DocumentService {
    
    // 自定义权限检查
    @PreAuthorize("@permissionService.canAccessDocument(#documentId, authentication.name)")
    public Document getDocument(Long documentId) {
        return documentRepository.findById(documentId).orElse(null);
    }
    
    // 组合多个权限
    @PreAuthorize("hasAuthority('DOCUMENT_READ') and @permissionService.isOwner(#documentId, authentication.name)")
    public Document viewDocument(Long documentId) {
        return documentRepository.findById(documentId).orElse(null);
    }
}
```

### 5. 角色层级

角色层级允许定义角色之间的继承关系，拥有高级角色的用户自动拥有低级角色的所有权限。

#### 5.1 配置角色层级

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Bean
    public RoleHierarchy roleHierarchy() {
        RoleHierarchyImpl hierarchy = new RoleHierarchyImpl();
        
        // 设置角色层级关系
        hierarchy.setHierarchy(
            "ROLE_ADMIN > ROLE_MANAGER\n" +
            "ROLE_MANAGER > ROLE_USER\n" +
            "ROLE_USER > ROLE_GUEST"
        );
        
        return hierarchy;
    }
    
    @Bean
    public RoleVoter roleVoter() {
        return new RoleVoter();
    }
    
    @Bean
    public AccessDecisionManager accessDecisionManager() {
        List<AccessDecisionVoter<? extends Object>> decisionVoters = 
            Arrays.asList(new WebExpressionVoter(), roleVoter());
        return new AffirmativeBased(decisionVoters);
    }
}
```

#### 5.2 在表达式中使用角色层级

```java
@Service
public class AdminService {
    
    // ADMIN拥有MANAGER和USER的所有权限
    @PreAuthorize("hasRole('MANAGER')")  // ADMIN用户也可以访问
    public void managerOperation() {
        // ...
    }
}
```

### 6. 动态RBAC

#### 6.1 动态SecurityMetadataSource

```java
@Component
public class DynamicSecurityMetadataSource implements 
        FilterInvocationSecurityMetadataSource {
    
    @Autowired
    private PermissionService permissionService;
    
    private Map<String, Collection<ConfigAttribute>> configAttributeMap = new ConcurrentHashMap<>();
    
    @PostConstruct
    public void loadConfig() {
        // 从数据库加载权限配置
        List<ResourcePermission> permissions = permissionService.loadAllPermissions();
        
        for (ResourcePermission rp : permissions) {
            Collection<ConfigAttribute> attributes = new ArrayList<>();
            
            for (String role : rp.getRoles()) {
                attributes.add(new SecurityConfig(role));
            }
            
            configAttributeMap.put(rp.getUrl(), attributes);
        }
    }
    
    @Override
    public Collection<ConfigAttribute> getAttributes(Object object) 
            throws IllegalArgumentException {
        
        FilterInvocation fi = (FilterInvocation) object;
        String url = fi.getRequestUrl();
        
        // 查找匹配的权限配置
        for (Map.Entry<String, Collection<ConfigAttribute>> entry : 
                configAttributeMap.entrySet()) {
            
            if (new AntPathMatcher().match(entry.getKey(), url)) {
                return entry.getValue();
            }
        }
        
        return null;
    }
    
    @Override
    public Collection<ConfigAttribute> getAllConfigAttributes() {
        Set<ConfigAttribute> allAttributes = new HashSet<>();
        
        for (Collection<ConfigAttribute> attrs : configAttributeMap.values()) {
            allAttributes.addAll(attrs);
        }
        
        return allAttributes;
    }
    
    @Override
    public boolean supports(Class<?> clazz) {
        return FilterInvocation.class.isAssignableFrom(clazz);
    }
    
    // 定时刷新配置
    @Scheduled(cron = "0 0/5 * * * ?")
    public void refreshConfig() {
        loadConfig();
    }
}
```

#### 6.2 自定义FilterSecurityInterceptor

```java
@Configuration
public class CustomSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Autowired
    private DynamicSecurityMetadataSource securityMetadataSource;
    
    @Bean
    public FilterSecurityInterceptor filterSecurityInterceptor() throws Exception {
        FilterSecurityInterceptor interceptor = new FilterSecurityInterceptor();
        
        interceptor.setSecurityMetadataSource(securityMetadataSource);
        interceptor.setAccessDecisionManager(accessDecisionManager());
        interceptor.setAuthenticationManager(authenticationManagerBean());
        
        return interceptor;
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .addFilterBefore(filterSecurityInterceptor(), 
                FilterSecurityInterceptor.class)
            .authorizeRequests()
                .anyRequest().authenticated();
    }
}
```

### 7. RBAC管理接口

#### 7.1 用户管理

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private RoleService roleService;
    
    // 创建用户
    @PostMapping
    @PreAuthorize("hasAuthority('USER_WRITE')")
    public ResponseEntity<User> createUser(@RequestBody UserDto userDto) {
        User user = userService.createUser(userDto);
        return ResponseEntity.ok(user);
    }
    
    // 分配角色
    @PostMapping("/{userId}/roles")
    @PreAuthorize("hasAuthority('USER_WRITE')")
    public ResponseEntity<Void> assignRole(
            @PathVariable Long userId, 
            @RequestBody AssignRoleDto assignRoleDto) {
        
        userService.assignRole(userId, assignRoleDto.getRoleId());
        return ResponseEntity.ok().build();
    }
    
    // 移除角色
    @DeleteMapping("/{userId}/roles/{roleId}")
    @PreAuthorize("hasAuthority('USER_WRITE')")
    public ResponseEntity<Void> removeRole(
            @PathVariable Long userId, 
            @PathVariable Long roleId) {
        
        userService.removeRole(userId, roleId);
        return ResponseEntity.ok().build();
    }
    
    // 查询用户角色
    @GetMapping("/{userId}/roles")
    @PreAuthorize("hasAuthority('USER_READ')")
    public ResponseEntity<Set<Role>> getUserRoles(@PathVariable Long userId) {
        Set<Role> roles = userService.getUserRoles(userId);
        return ResponseEntity.ok(roles);
    }
}
```

#### 7.2 角色管理

```java
@RestController
@RequestMapping("/api/roles")
public class RoleController {
    
    @Autowired
    private RoleService roleService;
    
    @Autowired
    private PermissionService permissionService;
    
    // 创建角色
    @PostMapping
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Role> createRole(@RequestBody RoleDto roleDto) {
        Role role = roleService.createRole(roleDto);
        return ResponseEntity.ok(role);
    }
    
    // 分配权限
    @PostMapping("/{roleId}/permissions")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> assignPermission(
            @PathVariable Long roleId,
            @RequestBody AssignPermissionDto dto) {
        
        roleService.assignPermission(roleId, dto.getPermissionId());
        return ResponseEntity.ok().build();
    }
    
    // 移除权限
    @DeleteMapping("/{roleId}/permissions/{permissionId}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> removePermission(
            @PathVariable Long roleId,
            @PathVariable Long permissionId) {
        
        roleService.removePermission(roleId, permissionId);
        return ResponseEntity.ok().build();
    }
    
    // 查询角色权限
    @GetMapping("/{roleId}/permissions")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Set<Permission>> getRolePermissions(@PathVariable Long roleId) {
        Set<Permission> permissions = roleService.getRolePermissions(roleId);
        return ResponseEntity.ok(permissions);
    }
}
```

### 8. FAQ

**Q1: 角色和权限有什么区别？应该如何设计？**

A: 
- **角色**：代表用户的身份或职责，如ADMIN、MANAGER、USER
- **权限**：代表具体的操作能力，如USER_READ、USER_WRITE、USER_DELETE

设计建议：
1. 角色通常是业务相关的抽象概念
2. 权限是具体的系统操作
3. 一个角色可以拥有多个权限
4. 同一权限可以分配给多个角色

示例：
```
角色: ADMIN
  权限: USER_READ, USER_WRITE, USER_DELETE, DOCUMENT_READ, DOCUMENT_WRITE

角色: USER
  权限: DOCUMENT_READ, DOCUMENT_WRITE
```

**Q2: hasRole()和hasAuthority()有什么区别？**

A: 
- `hasRole("ADMIN")` 会自动添加"ROLE_"前缀，检查的是"ROLE_ADMIN"
- `hasAuthority("USER_READ")` 直接检查"USER_READ"，不添加前缀

```java
// 用户有权限: ROLE_ADMIN, USER_READ
hasRole("ADMIN")          // true - 检查ROLE_ADMIN
hasAuthority("ROLE_ADMIN") // true
hasAuthority("ADMIN")      // false - 没有前缀

hasAuthority("USER_READ")  // true - 不添加前缀
hasRole("USER_READ")       // false - 会检查ROLE_USER_READ
```

**Q3: 如何实现临时授权（如临时提升权限）？**

A: 使用临时权限表：

```sql
CREATE TABLE temporary_permission (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    permission_id BIGINT NOT NULL,
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP NOT NULL,
    reason VARCHAR(200),
    granted_by VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES user(id),
    FOREIGN KEY (permission_id) REFERENCES permission(id)
);
```

```java
@Service
public class TemporaryPermissionService {
    
    public boolean hasTemporaryPermission(Long userId, String permission) {
        // 查询临时权限
        List<TemporaryPermission> tempPerms = temporaryPermissionRepository
            .findByUserIdAndPermissionAndTimeRange(userId, permission, new Date());
        
        return !tempPerms.isEmpty();
    }
    
    public void grantTemporaryPermission(Long userId, Long permissionId, 
            Date startTime, Date endTime, String reason, String grantedBy) {
        
        TemporaryPermission temp = new TemporaryPermission();
        temp.setUserId(userId);
        temp.setPermissionId(permissionId);
        temp.setStartTime(startTime);
        temp.setEndTime(endTime);
        temp.setReason(reason);
        temp.setGrantedBy(grantedBy);
        
        temporaryPermissionRepository.save(temp);
    }
}

// 在UserDetailsService中加载临时权限
public Collection<GrantedAuthority> getAuthorities(Long userId) {
    Set<GrantedAuthority> authorities = new HashSet<>();
    
    // 加载永久权限
    authorities.addAll(userRepository.getPermanentPermissions(userId));
    
    // 加载临时权限
    authorities.addAll(temporaryPermissionRepository
        .getCurrentPermissions(userId, new Date()));
    
    return authorities;
}
```

**Q4: 如何实现基于部门的RBAC？**

A: 扩展RBAC模型，添加组织结构：

```sql
-- 部门表
CREATE TABLE department (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    parent_id BIGINT,
    FOREIGN KEY (parent_id) REFERENCES department(id)
);

-- 用户-部门关联
CREATE TABLE user_department (
    user_id BIGINT NOT NULL,
    department_id BIGINT NOT NULL,
    position VARCHAR(50),
    PRIMARY KEY (user_id, department_id),
    FOREIGN KEY (user_id) REFERENCES user(id),
    FOREIGN KEY (department_id) REFERENCES department(id)
);

-- 资源-部门关联（资源属于特定部门）
CREATE TABLE resource_department (
    resource_type VARCHAR(50),
    resource_id VARCHAR(50),
    department_id BIGINT NOT NULL,
    PRIMARY KEY (resource_type, resource_id, department_id),
    FOREIGN KEY (department_id) REFERENCES department(id)
);
```

```java
@Service
public class DepartmentPermissionService {
    
    public boolean canAccessResource(String username, String resourceType, String resourceId) {
        // 获取用户所属部门
        List<Department> userDepts = userRepository.getUserDepartments(username);
        
        // 获取资源所属部门
        List<Department> resourceDepts = resourceRepository.getResourceDepartments(
            resourceType, resourceId);
        
        // 检查是否有交集（同一个部门或父部门）
        return hasCommonDepartment(userDepts, resourceDepts);
    }
    
    private boolean hasCommonDepartment(List<Department> userDepts, 
            List<Department> resourceDepts) {
        
        for (Department userDept : userDepts) {
            for (Department resourceDept : resourceDepts) {
                if (isSameOrParent(userDept, resourceDept)) {
                    return true;
                }
            }
        }
        
        return false;
    }
}
```

**Q5: 如何实现数据级权限控制？**

A: 实现数据级权限（如用户只能看到自己创建的数据）：

```java
@Service
public class DocumentService {
    
    @PreAuthorize("hasAuthority('DOCUMENT_READ')")
    @PostFilter("hasRole('ADMIN') or filterObject.owner == authentication.name")
    public List<Document> getAllDocuments() {
        return documentRepository.findAll();
    }
    
    @PreAuthorize("@documentPermission.canView(#id, authentication.name)")
    public Document getDocument(Long id) {
        return documentRepository.findById(id).orElse(null);
    }
}

@Component
public class DocumentPermission {
    
    public boolean canView(Long documentId, String username) {
        Document doc = documentRepository.findById(documentId).orElse(null);
        if (doc == null) {
            return false;
        }
        
        // 管理员可以查看所有文档
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        if (auth.getAuthorities().contains(new SimpleGrantedAuthority("ROLE_ADMIN"))) {
            return true;
        }
        
        // 检查是否是文档所有者
        if (doc.getOwner().equals(username)) {
            return true;
        }
        
        // 检查是否是共享的
        if (doc.isShared() && doc.getSharedUsers().contains(username)) {
            return true;
        }
        
        return false;
    }
}
```

### 9. 思考题

1. 如何设计一个支持跨域/跨系统SSO的RBAC系统？

2. 在微服务架构中，如何实现统一的RBAC管理？

3. 如何实现基于时间段的动态角色切换（如值班经理）？

4. 设计一个RBAC审计系统，记录所有权限变更和访问日志。

5. 如何处理权限继承和权限冲突？比如一个用户通过多个角色获得了对同一资源的不同权限。

---

# 模块4：会话管理与CSRF防护

## 知识点13：会话管理

### 1. 会话管理概述

会话管理是Web应用安全的重要组成部分。Spring Security提供了全面的会话管理功能，包括会话创建策略、会话固定攻击防护、并发会话控制、会话超时处理等。

### 2. 会话创建策略

Spring Security支持多种会话创建策略：

#### 2.1 SessionCreationPolicy枚举

```java
public enum SessionCreationPolicy {
    
    // 如果需要就创建Session（默认）
    REQUIRED,
    
    // Spring Security不会创建Session，但如果应用创建了Session，会使用它
    NEVER,
    
    // Spring Security不会创建或使用Session
    STATELESS,
    
    // 总是创建Session
    ALWAYS
}
```

#### 2.2 配置会话策略

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .sessionManagement()
                // 无状态会话（适用于REST API）
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
            .csrf().disable()  // 无状态时通常禁用CSRF
            .authorizeRequests()
                .anyRequest().authenticated();
    }
}

// 有状态的Web应用
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .sessionManagement()
                // 需要时创建Session
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .and()
            .formLogin();
    }
}
```

### 3. 会话固定攻击防护

会话固定攻击（Session Fixation Attack）是一种攻击者固定用户会话ID的攻击方式。攻击者在用户认证前获取一个会话ID，然后诱导用户使用该会话ID进行认证，从而获得对用户会话的访问权限。

#### 3.1 会话固定防护策略

Spring Security提供三种防护策略：

```java
public enum SessionFixationStrategy {
    
    // 不创建新会话（不安全，不推荐）
    NONE,
    
    // 创建新会话但不复制旧会话属性
    NEW_SESSION,
    
    // 创建新会话并复制旧会话属性（默认，推荐）
    MIGRATE_SESSION,
    
    // 创建新会话，复制属性，并使旧会话无效
    CHANGE_SESSION_ID  // Servlet 3.1+
}
```

#### 3.2 配置会话固定防护

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .sessionManagement()
                // 配置会话固定防护
                .sessionFixation()
                    // 使用migrateSession（默认）
                    //.migrateSession()
                    
                    // 或者使用changeSessionId（Servlet 3.1+，更高效）
                    .changeSessionId()
                    
                    // 或者不创建新会话（不安全）
                    //.none()
                    
                    // 或者创建全新的空会话
                    //.newSession()
                .and()
            .formLogin();
    }
}
```

#### 3.3 会话固定防护源码分析

```java
// SessionManagementFilter中的会话固定防护
public class SessionManagementFilter extends GenericFilterBean {
    
    private SessionAuthenticationStrategy sessionAuthenticationStrategy;
    
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        // 检查是否需要认证
        if (request.getAttribute(FILTER_APPLIED) != null) {
            chain.doFilter(request, response);
            return;
        }
        
        request.setAttribute(FILTER_APPLIED, Boolean.TRUE);
        
        // 检查认证是否发生改变
        if (!securityContextRepository.containsContext(request)) {
            Authentication authentication = SecurityContextHolder.getContext()
                    .getAuthentication();
            
            if (authentication != null && !authenticationTrustResolver.isAnonymous(authentication)) {
                // 认证成功后，执行会话策略
                sessionAuthenticationStrategy.onAuthentication(
                    authentication, request, response);
            }
        }
        
        chain.doFilter(request, response);
    }
}

// SessionFixationProtectionStrategy实现
public class SessionFixationProtectionStrategy implements
        SessionAuthenticationStrategy {
    
    // 策略类型
    private final SessionFixationProtectionStrategy.SessionFixationStrategy fixation;
    
    @Override
    public void onAuthentication(Authentication authentication,
            HttpServletRequest request, HttpServletResponse response) {
        
        HttpSession session = request.getSession(false);
        
        if (session == null) {
            return;
        }
        
        switch (fixation) {
            case MIGRATE_SESSION:
                // 创建新会话并复制属性
                HttpSession newSession = request.getSession(true);
                
                // 复制属性
                migrateSessionAttributes(session, newSession);
                
                // 使旧会话无效
                session.invalidate();
                
                break;
                
            case NEW_SESSION:
                // 创建全新的空会话
                request.getSession(true);
                session.invalidate();
                break;
                
            case CHANGE_SESSION_ID:
                // 使用Servlet 3.1的changeSessionId()方法
                request.changeSessionId();
                break;
                
            case NONE:
                // 不做任何事
                break;
        }
    }
    
    private void migrateSessionAttributes(HttpSession source, HttpSession target) {
        // 复制所有属性
        Enumeration<String> attrNames = source.getAttributeNames();
        while (attrNames.hasMoreElements()) {
            String attrName = attrNames.nextElement();
            Object attrValue = source.getAttribute(attrName);
            target.setAttribute(attrName, attrValue);
        }
    }
}
```

### 4. 并发会话控制

并发会话控制允许限制同一用户的最大并发会话数，防止账号被多处同时登录。

#### 4.1 SessionRegistry

`SessionRegistry`是Spring Security会话管理的核心接口。

```java
public interface SessionRegistry {
    
    /**
     * 获取所有已注册的SessionId
     */
    List<String> getAllSessions(Object principal, boolean includeExpiredSessions);
    
    /**
     * 获取SessionInformation
     */
    SessionInformation getSessionInformation(String sessionId);
    
    /**
     * 注册新会话
     */
    void registerNewSession(String sessionId, Object principal);
    
    /**
     * 移除会话
     */
    void removeSessionInformation(String sessionId);
    
    /**
     * 获取所有 principals
     */
    List<Object> getAllPrincipals();
}
```

**SessionInformation包含会话的详细信息：**

```java
public class SessionInformation implements Serializable {
    
    private final String sessionId;           // Session ID
    private final Object principal;            // 认证主体
    private Date lastRequest;                  // 最后请求时间
    private boolean expired = false;           // 是否过期
    
    public SessionInformation(Object principal, String sessionId, Date lastRequest) {
        this.principal = principal;
        this.sessionId = sessionId;
        this.lastRequest = lastRequest;
    }
    
    /**
     * 刷新最后请求时间
     */
    public void refreshLastRequest() {
        this.lastRequest = new Date();
    }
    
    /**
     * 使会话过期
     */
    public void expireNow() {
        this.expired = true;
    }
    
    // getters and setters
}
```

#### 4.2 配置并发会话控制

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .sessionManagement()
                // 启用并发会话控制
                .maximumSessions(1)
                    // 阻止新登录（默认是踢掉旧会话）
                    .maxSessionsPreventsLogin(true)
                    // 设置过期会话处理策略
                    .expiredUrl("/login?expired")
                    // 自定义过期处理器
                    .expiredSessionStrategy(customExpiredSessionStrategy())
                    // SessionRegistry
                    .sessionRegistry(sessionRegistry())
                .and()
            .and()
            .formLogin();
    }
    
    @Bean
    public SessionRegistry sessionRegistry() {
        return new SessionRegistryImpl();
    }
    
    @Bean
    public CustomExpiredSessionStrategy customExpiredSessionStrategy() {
        return new CustomExpiredSessionStrategy();
    }
}

// 自定义过期会话策略
public class CustomExpiredSessionStrategy implements 
        SessionManagementConfigurer.SessionFixationStrategy {
    
    @Override
    public void onExpiredSessionDetected(SessionInformationExpiredEvent event)
            throws IOException, ServletException {
        
        HttpServletRequest request = event.getRequest();
        HttpServletResponse response = event.getResponse();
        
        // 返回JSON响应
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write("{\"code\": 401, \"message\": \"您的账号已在其他地方登录\"}");
    }
}
```

#### 4.3 ConcurrentSessionFilter

`ConcurrentSessionFilter`负责检查会话是否过期。

```java
public class ConcurrentSessionFilter extends GenericFilterBean {
    
    private SessionRegistry sessionRegistry;
    private String expiredUrl;
    private LogoutHandler[] handlers = new LogoutHandler[0];
    
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        
        HttpSession session = request.getSession(false);
        
        if (session != null) {
            // 检查会话信息
            SessionInformation info = sessionRegistry.getSessionInformation(session
                    .getId());
            
            if (info != null) {
                if (info.isExpired()) {
                    // 会话已过期
                    sessionRegistry.removeSessionInformation(session.getId());
                    
                    // 执行登出处理
                    for (LogoutHandler handler : handlers) {
                        handler.logout(request, response, 
                            SecurityContextHolder.getContext().getAuthentication());
                    }
                    
                    // 重定向到过期URL或返回响应
                    if (expiredUrl != null) {
                        response.sendRedirect(expiredUrl);
                        return;
                    } else {
                        response.getWriter().print("This session has been expired");
                        return;
                    }
                }
                
                // 更新最后访问时间
                info.refreshLastRequest();
            }
        }
        
        chain.doFilter(req, res);
    }
}
```

### 5. 会话超时与过期

#### 5.1 配置会话超时

```yaml
# application.yml
server:
  servlet:
    session:
      timeout: 1800  # 30分钟（秒）
```

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Bean
    public EmbeddedServletContainerCustomizer servletContainerCustomizer() {
        return new EmbeddedServletContainerCustomizer() {
            @Override
            public void customize(ConfigurableEmbeddedServletContainer container) {
                // 设置会话超时时间（分钟）
                container.setSessionTimeout(30, TimeUnit.MINUTES);
            }
        };
    }
    
    @Bean
    public HttpSessionListener httpSessionListener() {
        return new HttpSessionListener() {
            @Override
            public void sessionCreated(HttpSessionEvent se) {
                log.info("Session created: {}", se.getSession().getId());
            }
            
            @Override
            public void sessionDestroyed(HttpSessionEvent se) {
                log.info("Session destroyed: {}", se.getSession().getId());
            }
        };
    }
}
```

#### 5.2 自定义会话过期处理器

```java
@Component
public class CustomSessionExpiredStrategy implements 
        SessionInformationExpiredStrategy {
    
    @Override
    public void onExpiredSessionDetected(SessionInformationExpiredEvent event)
            throws IOException, ServletException {
        
        HttpServletRequest request = event.getRequest();
        HttpServletResponse response = event.getResponse();
        
        // 检查是否是AJAX请求
        String requestedWith = request.getHeader("X-Requested-With");
        boolean isAjax = "XMLHttpRequest".equals(requestedWith);
        
        if (isAjax) {
            // 返回JSON响应
            response.setContentType("application/json;charset=UTF-8");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            
            Map<String, Object> data = new HashMap<>();
            data.put("code", 401);
            data.put("message", "会话已过期，请重新登录");
            data.put("timestamp", System.currentTimeMillis());
            
            new ObjectMapper().writeValue(response.getWriter(), data);
        } else {
            // 重定向到登录页
            response.sendRedirect("/login?expired");
        }
    }
}
```

### 6. 分布式会话管理

在分布式系统中，需要将会话存储到共享存储（如Redis）中。

#### 6.1 Spring Session + Redis

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```yaml
# application.yml
spring:
  redis:
    host: localhost
    port: 6379
    password: 
    database: 0
    timeout: 5000
  session:
    store-type: redis
    redis:
      namespace: spring:session
      cleanup-cron: "0 * * * * *"
```

```java
@Configuration
@EnableRedisHttpSession(
    maxInactiveIntervalInSeconds = 1800,  // 30分钟
    redisNamespace = "spring:session"
)
public class HttpSessionConfig {
    
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        LettuceConnectionFactory factory = new LettuceConnectionFactory();
        factory.setHostName("localhost");
        factory.setPort(6379);
        return factory;
    }
    
    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory("localhost", 6379);
    }
}
```

#### 6.2 自定义SessionRepository

```java
@Component
public class CustomSessionRepository implements SessionRepository<Session> {
    
    @Autowired
    private RedisTemplate<String, Session> redisTemplate;
    
    private static final String SESSION_PREFIX = "spring:session:";
    
    @Override
    public Session createSession() {
        MapSession session = new MapSession();
        session.setCreationTime(Instant.now());
        session.setLastAccessedTime(Instant.now());
        session.setMaxInactiveInterval(Duration.ofSeconds(1800));
        return session;
    }
    
    @Override
    public void save(Session session) {
        String key = SESSION_PREFIX + session.getId();
        redisTemplate.opsForValue().set(key, session, 
            Duration.ofSeconds(session.getMaxInactiveInterval().getSeconds()));
    }
    
    @Override
    public Session findById(String id) {
        String key = SESSION_PREFIX + id;
        return redisTemplate.opsForValue().get(key);
    }
    
    @Override
    public void deleteById(String id) {
        String key = SESSION_PREFIX + id;
        redisTemplate.delete(key);
    }
}
```

### 7. FAQ

**Q1: SessionCreationPolicy.STATELESS和SessionCreationPolicy.NEVER有什么区别？**

A: 
- **STATELESS**: Spring Security完全不使用Session，适合REST API
- **NEVER**: Spring Security不会主动创建Session，但如果应用创建了Session，Spring Security会使用它

示例：
```java
// STATELESS - 即使应用创建Session，Spring Security也不使用
.sessionCreationPolicy(SessionCreationPolicy.STATELESS)

// NEVER - Spring Security不创建Session，但如果应用创建了，会使用
.sessionCreationPolicy(SessionCreationPolicy.NEVER)
```

**Q2: 如何在Spring Boot中配置会话超时时间？**

A: 三种方式：

1. **application.yml配置：**
```yaml
server:
  servlet:
    session:
      timeout: 1800  # 秒
```

2. **Java配置：**
```java
@Bean
public WebServerFactoryCustomizer<TomcatServletWebServerFactory> 
        servletContainerCustomizer() {
    return factory -> factory.setSessionTimeout(30, TimeUnit.MINUTES);
}
```

3. **web.xml（如果使用）：**
```xml
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

**Q3: 如何实现单点登录（SSO）？**

A: 单点登录有多种实现方式：

方式1：使用Spring Security的共享Session（分布式会话）

方式2：使用CAS（Central Authentication Service）

方式3：使用OAuth 2.0 / OpenID Connect

方式4：使用JWT令牌

简单的共享Session实现：

```java
@Configuration
@EnableRedisHttpSession
public class HttpSessionConfig {
    
    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory("redis-server", 6379);
    }
}
```

**Q4: 并发会话控制如何与Remember Me一起使用？**

A: 需要注意，Remember Me不会创建Session，只有在明确认证后才会创建Session。

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .sessionManagement()
            .maximumSessions(1)
                .maxSessionsPreventsLogin(true)
                // 不包括Remember Me会话
                .sessionRegistry(sessionRegistry())
        .and()
        .and()
        .rememberMe()
            .key("uniqueAndSecret")
            .tokenRepository(persistentTokenRepository());
}
```

**Q5: 如何实现会话清理（清理过期会话）？**

A: 使用Spring Session的任务调度：

```java
@Configuration
@EnableRedisHttpSession
@EnableScheduling
public class SessionConfig {
    
    @Bean
    public RedisHttpSessionConfiguration redisHttpSessionConfiguration() {
        RedisHttpSessionConfiguration config = new RedisHttpSessionConfiguration();
        config.setMaxInactiveIntervalInSeconds(1800);
        config.setCleanupCron("0 * * * * *");  // 每小时执行一次清理
        return config;
    }
}

// 或者手动实现
@Component
public class SessionCleanupTask {
    
    @Autowired
    private SessionRegistry sessionRegistry;
    
    @Scheduled(cron = "0 0 * * * *")  // 每小时执行
    public void cleanupExpiredSessions() {
        List<Object> allPrincipals = sessionRegistry.getAllPrincipals();
        
        for (Object principal : allPrincipals) {
            List<SessionInformation> sessions = sessionRegistry.getAllSessions(
                principal, false);
            
            for (SessionInformation session : sessions) {
                if (session.isExpired()) {
                    sessionRegistry.removeSessionInformation(session.getSessionId());
                }
            }
        }
    }
}
```

### 8. 思考题

1. 在微服务架构中，如何实现统一的会话管理？

2. 如何设计一个支持多终端登录（如Web、移动端）的会话管理系统？

3. 如何实现会话的实时监控和审计？

4. 在高并发场景下，如何优化会话管理的性能？

5. 如何处理会话的优雅关闭（如用户主动登出、系统关闭等）？

---

## 知识点14：CSRF防护

### 1. CSRF概述

跨站请求伪造（Cross-Site Request Forgery，CSRF）是一种攻击方式，攻击者诱导用户在已认证的Web应用中执行非本意的操作。

#### 1.1 CSRF攻击原理

```
用户已登录银行网站（bank.com）
↓
攻击者诱导用户访问恶意网站（evil.com）
↓
恶意网站向bank.com发送请求（利用用户的登录状态）
↓
银行网站认为是用户的合法操作并执行
```

#### 1.2 CSRF攻击示例

```html
<!-- 恶意网站 evil.com 上的页面 -->
<html>
<body>
  <h1>你中奖了！点击领取</h1>
  <!-- 用户点击后会向bank.com发起转账请求 -->
  <img src="https://bank.com/transfer?to=attacker&amount=10000" />
</body>
</html>
```

### 2. Spring Security的CSRF防护

Spring Security默认启用CSRF防护，使用同步令牌模式（Synchronizer Token Pattern）。

#### 2.1 CsrfFilter

`CsrfFilter`是CSRF防护的核心过滤器。

```java
public final class CsrfFilter extends OncePerRequestFilter {
    
    public static final String CSRF_TOKEN_ATTR_NAME = 
        CsrfToken.class.getName();
    
    private final CsrfTokenRepository tokenRepository;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        
        request.setAttribute(HttpServletResponse.class.getName(), response);
        
        // 获取或生成CSRF Token
        CsrfToken csrfToken = tokenRepository.loadToken(request);
        
        if (csrfToken == null) {
            // 生成新Token
            csrfToken = tokenRepository.generateToken(request);
            tokenRepository.saveToken(csrfToken, request, response);
        }
        
        // 将Token设置到请求属性中
        request.setAttribute(csrfToken.getParameterName(), csrfToken);
        request.setAttribute(CSRF_TOKEN_ATTR_NAME, csrfToken);
        
        // 检查是否需要CSRF保护
        if (!this.requireCsrfProtectionMatcher.matches(request)) {
            // 不需要CSRF保护的请求（如GET、HEAD等）
            filterChain.doFilter(request, response);
            return;
        }
        
        // 从请求中获取Token
        String actualToken = request.getHeader(csrfToken.getHeaderName());
        if (actualToken == null) {
            actualToken = request.getParameter(csrfToken.getParameterName());
        }
        
        // 验证Token
        if (!csrfToken.getToken().equals(actualToken)) {
            // Token不匹配，拒绝访问
            AccessDeniedException exception = new InvalidCsrfTokenException(
                csrfToken, actualToken);
            this.accessDeniedHandler.handle(request, response, exception);
            return;
        }
        
        // Token验证通过
        filterChain.doFilter(request, response);
    }
}
```

#### 2.2 CsrfToken

```java
public interface CsrfToken extends Serializable {
    
    /**
     * 获取Header名称
     */
    String getHeaderName();
    
    /**
     * 获取参数名称
     */
    String getParameterName();
    
    /**
     * 获取Token值
     */
    String getToken();
}

// 默认实现
public class DefaultCsrfToken implements CsrfToken {
    
    private final String headerName;
    private final String parameterName;
    private final String token;
    
    public DefaultCsrfToken(String headerName, String parameterName, String token) {
        this.headerName = headerName;
        this.parameterName = parameterName;
        this.token = token;
    }
    
    @Override
    public String getHeaderName() {
        return headerName;
    }
    
    @Override
    public String getParameterName() {
        return parameterName;
    }
    
    @Override
    public String getToken() {
        return token;
    }
}
```

### 3. CsrfTokenRepository

`CsrfTokenRepository`负责CSRF Token的存储和检索。

#### 3.1 HttpSessionCsrfTokenRepository

```java
public final class HttpSessionCsrfTokenRepository implements CsrfTokenRepository {
    
    // 默认的参数名和Header名
    static final String DEFAULT_CSRF_PARAMETER_NAME = "_csrf";
    static final String DEFAULT_CSRF_HEADER_NAME = "X-CSRF-TOKEN";
    static final String DEFAULT_CSRF_TOKEN_ATTR_NAME = 
        HttpSessionCsrfTokenRepository.class.getName() + ".CSRF_TOKEN";
    
    private String parameterName = DEFAULT_CSRF_PARAMETER_NAME;
    private String headerName = DEFAULT_CSRF_HEADER_NAME;
    private String sessionAttributeName = DEFAULT_CSRF_TOKEN_ATTR_NAME;
    
    @Override
    public CsrfToken generateToken(HttpServletRequest request) {
        return new DefaultCsrfToken(this.headerName, this.parameterName, 
            createNewToken());
    }
    
    @Override
    public void saveToken(CsrfToken token, HttpServletRequest request,
            HttpServletResponse response) {
        
        if (token == null) {
            HttpSession session = request.getSession(false);
            if (session != null) {
                session.removeAttribute(this.sessionAttributeName);
            }
        } else {
            HttpSession session = request.getSession(true);
            session.setAttribute(this.sessionAttributeName, token);
        }
    }
    
    @Override
    public CsrfToken loadToken(HttpServletRequest request) {
        HttpSession session = request.getSession(false);
        if (session == null) {
            return null;
        }
        return (CsrfToken) session.getAttribute(this.sessionAttributeName);
    }
    
    private String createNewToken() {
        return UUID.randomUUID().toString();
    }
}
```

#### 3.2 CookieCsrfTokenRepository

```java
public final class CookieCsrfTokenRepository implements CsrfTokenRepository {
    
    static final String DEFAULT_CSRF_COOKIE_NAME = "XSRF-TOKEN";
    static final String DEFAULT_CSRF_PARAMETER_NAME = "_csrf";
    static final String DEFAULT_CSRF_HEADER_NAME = "X-XSRF-TOKEN";
    
    private String parameterName = DEFAULT_CSRF_PARAMETER_NAME;
    private String headerName = DEFAULT_CSRF_HEADER_NAME;
    private String cookieName = DEFAULT_CSRF_COOKIE_NAME;
    private boolean cookieHttpOnly = true;
    private Boolean secure;
    private int cookieMaxAge = -1;  // Session cookie
    
    @Override
    public CsrfToken generateToken(HttpServletRequest request) {
        return new DefaultCsrfToken(this.headerName, this.parameterName, 
            createNewToken());
    }
    
    @Override
    public void saveToken(CsrfToken token, HttpServletRequest request,
            HttpServletResponse response) {
        
        String tokenValue = token == null ? "" : token.getToken();
        
        Cookie cookie = new Cookie(this.cookieName, tokenValue);
        cookie.setPath(request.getContextPath());
        cookie.setHttpOnly(this.cookieHttpOnly);
        
        if (this.secure != null) {
            cookie.setSecure(this.secure);
        } else if (request.isSecure()) {
            cookie.setSecure(true);
        }
        
        if (token == null) {
            // 删除Cookie
            cookie.setMaxAge(0);
        } else {
            cookie.setMaxAge(this.cookieMaxAge);
        }
        
        response.addCookie(cookie);
    }
    
    @Override
    public CsrfToken loadToken(HttpServletRequest request) {
        Cookie cookie = WebUtils.getCookie(request, this.cookieName);
        if (cookie == null) {
            return null;
        }
        String token = cookie.getValue();
        if (StringUtils.hasLength(token)) {
            return new DefaultCsrfToken(this.headerName, this.parameterName, token);
        } else {
            return null;
        }
    }
    
    private String createNewToken() {
        return UUID.randomUUID().toString();
    }
}
```

### 4. 配置CSRF防护

#### 4.1 基本配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // CSRF配置（默认启用）
            .csrf()
                // 自定义Token Repository
                .csrfTokenRepository(csrfTokenRepository())
                // 忽略某些请求
                .ignoringAntMatchers("/api/public/**")
                // 自定义AccessDeniedHandler
                .accessDeniedHandler(csrfAccessDeniedHandler())
                .and()
            .authorizeRequests()
                .anyRequest().authenticated();
    }
    
    @Bean
    public CsrfTokenRepository csrfTokenRepository() {
        // 使用Cookie存储
        CookieCsrfTokenRepository repository = new CookieCsrfTokenRepository();
        repository.setHeaderName("X-XSRF-TOKEN");
        repository.setCookieName("XSRF-TOKEN");
        repository.setCookieHttpOnly(false);  // 允许JavaScript读取
        
        return repository;
        
        // 或者使用Session存储
        // return new HttpSessionCsrfTokenRepository();
    }
    
    @Bean
    public AccessDeniedHandler csrfAccessDeniedHandler() {
        return new CustomCsrfAccessDeniedHandler();
    }
}
```

#### 4.2 禁用CSRF（不推荐）

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .csrf().disable()  // 禁用CSRF防护
        .authorizeRequests()
            .anyRequest().authenticated();
}
```

### 5. 前端集成CSRF

#### 5.1 表单提交

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form th:action="@{/login}" method="post">
        <!-- Thymeleaf自动插入CSRF Token -->
        <input type="text" name="username" />
        <input type="password" name="password" />
        <button type="submit">登录</button>
    </form>
</body>
</html>
```

生成的HTML：
```html
<form action="/login" method="post">
    <input type="hidden" name="_csrf" value="abc123def456" />
    <input type="text" name="username" />
    <input type="password" name="password" />
    <button type="submit">登录</button>
</form>
```

#### 5.2 AJAX请求

```javascript
// 方式1：从Meta标签获取Token
// 服务器在页面中插入
// <meta name="_csrf" content="abc123def456" />
// <meta name="_csrf_header" content="X-CSRF-TOKEN" />

function sendAjaxRequest() {
    var token = $("meta[name='_csrf']").attr("content");
    var header = $("meta[name='_csrf_header']").attr("content");
    
    $.ajax({
        url: "/api/transfer",
        type: "POST",
        beforeSend: function(xhr) {
            xhr.setRequestHeader(header, token);
        },
        data: {
            to: "user123",
            amount: 100
        },
        success: function(response) {
            console.log("Success:", response);
        }
    });
}

// 方式2：从Cookie获取Token
function sendAjaxRequestWithCookie() {
    function getCookie(name) {
        var value = "; " + document.cookie;
        var parts = value.split("; " + name + "=");
        if (parts.length == 2) return parts.pop().split(";").shift();
    }
    
    var token = getCookie("XSRF-TOKEN");
    
    $.ajax({
        url: "/api/transfer",
        type: "POST",
        headers: {
            "X-XSRF-TOKEN": token
        },
        data: {
            to: "user123",
            amount: 100
        },
        success: function(response) {
            console.log("Success:", response);
        }
    });
}
```

#### 5.3 Vue.js示例

```javascript
// 在main.js中配置Axios
import axios from 'axios';

// 从Meta标签获取CSRF Token
const token = document.querySelector('meta[name="_csrf"]').getAttribute('content');
const header = document.querySelector('meta[name="_csrf_header"]').getAttribute('content');

// 设置默认Header
axios.defaults.headers.common[header] = token;

// 或者使用拦截器
axios.interceptors.request.use(config => {
    const token = document.querySelector('meta[name="_csrf"]').getAttribute('content');
    const header = document.querySelector('meta[name="_csrf_header"]').getAttribute('content');
    config.headers[header] = token;
    return config;
}, error => {
    return Promise.reject(error);
});
```

#### 5.4 React示例

```javascript
// 设置Axios默认Header
import axios from 'axios';

function getCsrfToken() {
    const metaTag = document.querySelector('meta[name="_csrf"]');
    return metaTag ? metaTag.getAttribute('content') : '';
}

function getCsrfHeader() {
    const metaTag = document.querySelector('meta[name="_csrf_header"]');
    return metaTag ? metaTag.getAttribute('content') : 'X-CSRF-TOKEN';
}

// 设置默认Header
axios.defaults.headers.common[getCsrfHeader()] = getCsrfToken();

// 或者使用拦截器
axios.interceptors.request.use(config => {
    config.headers[getCsrfHeader()] = getCsrfToken();
    return config;
});

// 使用示例
function transferFunds(to, amount) {
    axios.post('/api/transfer', { to, amount })
        .then(response => {
            console.log('Transfer successful:', response.data);
        })
        .catch(error => {
            console.error('Transfer failed:', error);
        });
}
```

### 6. REST API的CSRF处理

对于REST API，通常不需要CSRF防护（因为使用JWT或其他方式）。

```java
@Configuration
@EnableWebSecurity
public class ApiSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // API不需要CSRF（使用JWT）
            .csrf().disable()
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeRequests()
                .antMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .addFilter(new JwtAuthenticationFilter(authenticationManager()))
            .addFilter(new JwtAuthorizationFilter(authenticationManager()));
    }
}
```

### 7. 自定义CSRF处理

#### 7.1 自定义CsrfTokenRepository

```java
public class CustomCsrfTokenRepository implements CsrfTokenRepository {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    private static final String CSRF_PREFIX = "csrf:";
    private static final int TOKEN_VALIDITY_SECONDS = 3600;  // 1小时
    
    @Override
    public CsrfToken generateToken(HttpServletRequest request) {
        String token = UUID.randomUUID().toString();
        return new DefaultCsrfToken("X-CSRF-TOKEN", "_csrf", token);
    }
    
    @Override
    public void saveToken(CsrfToken token, HttpServletRequest request,
            HttpServletResponse response) {
        
        String sessionId = request.getSession().getId();
        String key = CSRF_PREFIX + sessionId;
        
        if (token == null) {
            redisTemplate.delete(key);
        } else {
            redisTemplate.opsForValue().set(key, token.getToken(), 
                TOKEN_VALIDITY_SECONDS, TimeUnit.SECONDS);
        }
    }
    
    @Override
    public CsrfToken loadToken(HttpServletRequest request) {
        String sessionId = request.getSession().getId();
        String key = CSRF_PREFIX + sessionId;
        String token = redisTemplate.opsForValue().get(key);
        
        if (token != null) {
            return new DefaultCsrfToken("X-CSRF-TOKEN", "_csrf", token);
        }
        
        return null;
    }
}
```

#### 7.2 自定义AccessDeniedHandler

```java
public class CustomCsrfAccessDeniedHandler implements AccessDeniedHandler {
    
    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response,
            AccessDeniedException accessDeniedException) throws IOException, ServletException {
        
        // 检查是否是AJAX请求
        String requestedWith = request.getHeader("X-Requested-With");
        boolean isAjax = "XMLHttpRequest".equals(requestedWith) || 
                        request.getRequestURI().startsWith("/api/");
        
        if (isAjax) {
            // 返回JSON响应
            response.setStatus(HttpServletResponse.SC_FORBIDDEN);
            response.setContentType("application/json;charset=UTF-8");
            
            Map<String, Object> data = new HashMap<>();
            data.put("code", 403);
            data.put("message", "CSRF Token验证失败");
            data.put("path", request.getRequestURI());
            data.put("timestamp", System.currentTimeMillis());
            
            response.getWriter().write(objectMapper.writeValueAsString(data));
        } else {
            // 重定向到错误页面
            response.sendRedirect("/error/csrf");
        }
    }
}
```

### 8. FAQ

**Q1: 哪些HTTP方法不需要CSRF防护？**

A: 按照HTTP规范，以下方法是安全的，不需要CSRF防护：
- GET
- HEAD
- TRACE
- OPTIONS

需要CSRF防护的方法：
- POST
- PUT
- DELETE
- PATCH

Spring Security默认只对改变状态的方法（POST、PUT、DELETE、PATCH）进行CSRF防护。

**Q2: 如何为特定URL禁用CSRF？**

A: 使用`ignoringAntMatchers()`：

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .csrf()
            .csrfTokenRepository(csrfTokenRepository())
            // 忽略特定URL
            .ignoringAntMatchers("/api/webhook/**", "/api/public/**")
        .and()
        // ...
}
```

**Q3: HttpSessionCsrfTokenRepository和CookieCsrfTokenRepository有什么区别？**

A: 
- **HttpSessionCsrfTokenRepository**: Token存储在Session中，更安全，但需要Session
- **CookieCsrfTokenRepository**: Token存储在Cookie中，更适合无状态应用，但需要配置

选择建议：
- 传统Web应用使用HttpSessionCsrfTokenRepository
- SPA（单页应用）使用CookieCsrfTokenRepository
- REST API通常禁用CSRF，使用JWT

**Q4: 如何在前后端分离的项目中处理CSRF？**

A: 推荐使用CookieCsrfTokenRepository：

```java
@Bean
public CsrfTokenRepository csrfTokenRepository() {
    CookieCsrfTokenRepository repository = new CookieCsrfTokenRepository();
    repository.setCookieHttpOnly(false);  // 允许JavaScript读取
    repository.setCookieName("XSRF-TOKEN");
    repository.setHeaderName("X-XSRF-TOKEN");
    return repository;
}
```

前端JavaScript：
```javascript
// 从Cookie读取Token并发送
function getCookie(name) {
    const value = `; ${document.cookie}`;
    const parts = value.split(`; ${name}=`);
    if (parts.length === 2) return parts.pop().split(';').shift();
}

// Axios配置
axios.defaults.headers.common['X-XSRF-TOKEN'] = getCookie('XSRF-TOKEN');
```

**Q5: CSRF和CORS有什么区别？**

A: 

**CSRF（跨站请求伪造）**：
- 攻击者利用用户的已登录状态
- 用户访问恶意网站，恶意网站向目标网站发起请求
- 防护方式：CSRF Token、SameSite Cookie、验证Referer

**CORS（跨源资源共享）**：
- 是W3C标准，用于控制跨域请求
- 服务器通过HTTP头来控制哪些域可以访问资源
- 是一种正常的功能，不是攻击

示例：
```java
// CSRF防护 - Spring Security配置
http.csrf().csrfTokenRepository(csrfTokenRepository());

// CORS配置 - Spring MVC配置
@Configuration
public class CorsConfig {
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowCredentials(true);
        config.addAllowedOrigin("https://example.com");
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        
        return new CorsFilter(source);
    }
}
```

### 9. 思考题

1. 在微服务架构中，如何统一处理CSRF防护？

2. 如何实现动态的CSRF Token（每次请求都变化）？

3. CSRF Token存储在Cookie中是否存在安全风险？如何缓解？

4. 如何实现CSRF防护的粒度控制（如不同页面使用不同Token）？

5. 分析CSRF防护的性能影响，如何优化？

---

## 知识点15：安全头配置

### 1. 安全头概述

HTTP响应头是增强Web应用安全性的重要手段。Spring Security默认添加了多个安全响应头，并提供灵活的配置选项。

### 2. 常见安全头

#### 2.1 X-Frame-Options（防止点击劫持）

防止网页被嵌入到iframe中，防止点击劫持攻击。

```java
// 配置
http.headers()
    .frameOptions()
        .sameOrigin()  // 只允许同源嵌入
        //.deny()      // 禁止所有嵌入
        //.disable()   // 禁用此保护
```

值说明：
- **DENY**: 禁止所有域嵌入
- **SAMEORIGIN**: 只允许同源嵌入
- **ALLOW-FROM uri**: 允许指定域嵌入（已废弃）

#### 2.2 X-XSS-Protection（浏览器XSS防护）

启用浏览器的XSS过滤功能。

```java
http.headers()
    .xssProtection()
        .headerValue("1; mode=block")  // 启用并阻止页面加载
        //.disable()
```

值说明：
- `0`: 禁用XSS保护
- `1`: 启用XSS保护
- `1; mode=block`: 启用XSS保护，检测到攻击时阻止页面加载

#### 2.3 Content-Security-Policy（内容安全策略）

CSP是强大的安全机制，控制资源加载来源。

```java
http.headers()
    .contentSecurityPolicy("default-src 'self'; script-src 'self' https://cdn.example.com; style-src 'self' 'unsafe-inline'")
        //.and()
        //.contentSecurityPolicyReportOnly("...")  // 报告模式
        .and()
    .headersConfigurer().disable()  // 完全禁用
```

#### 2.4 Strict-Transport-Security（HSTS）

强制浏览器使用HTTPS连接。

```java
http.headers()
    .httpStrictTransportSecurity()
        .includeSubDomains(true)
        .maxAgeInSeconds(31536000)  // 1年
        //.disable()
```

#### 2.5 X-Content-Type-Options

防止MIME类型嗅探。

```java
http.headers()
    .contentTypeOptions()
        //.disable()
```

响应头：`X-Content-Type-Options: nosniff`

### 3. 完整安全头配置

#### 3.1 默认配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // 默认安全头配置
            .headers()
                .and()
            .authorizeRequests()
                .anyRequest().authenticated();
    }
}

// 默认添加的响应头：
// Cache-Control: no-cache, no-store, max-age=0, must-revalidate
// Pragma: no-cache
// Expires: 0
// X-Content-Type-Options: nosniff
// X-Frame-Options: DENY
// X-XSS-Protection: 1; mode=block
// Strict-Transport-Security: max-age=31536000 ; includeSubDomains (仅HTTPS)
```

#### 3.2 自定义配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .headers()
                // 缓存控制
                .cacheControl()
                    //.disable()
                    
                // 内容类型选项
                .contentTypeOptions()
                    //.disable()
                    
                // 框架选项
                .frameOptions()
                    .sameOrigin()
                    //.deny()
                    //.disable()
                    
                // XSS保护
                .xssProtection()
                    .headerValue("1; mode=block")
                    //.disable()
                    
                // HSTS
                .httpStrictTransportSecurity()
                    .includeSubDomains(true)
                    .maxAgeInSeconds(31536000)
                    //.disable()
                    
                // 内容安全策略
                .contentSecurityPolicy("default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://cdn.example.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; img-src 'self' data: https:; connect-src 'self' https://api.example.com")
                    .reportOnly()  // 报告模式（不阻止）
                    //.and().disable()
                    
                // 权限策略
                .permissionsPolicy()
                    .policy("geolocation=(self), fullscreen=(), camera=(self)")
                    
                // Referrer策略
                .referrerPolicy()
                    .policy(ReferrerPolicy.STRICT_ORIGIN_WHEN_CROSS_ORIGIN)
                    
                // 特性策略
                .featurePolicy("geolocation 'self'")
                
                // 添加自定义头
                .addHeaderWriter(new StaticHeaderWriter("X-Application-Name", "MyApp"))
                .and()
            .authorizeRequests()
                .anyRequest().authenticated();
    }
}
```

### 4. 自定义HeaderWriter

#### 4.1 静态头

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .headers()
                // 添加自定义静态头
                .addHeaderWriter(new StaticHeaderWriter("X-Application-Name", "MyApp"))
                .addHeaderWriter(new StaticHeaderWriter("X-API-Version", "v1.0"))
                .addHeaderWriter(new StaticHeaderWriter("Server", "MyServer"))
                .and()
            .authorizeRequests();
    }
}
```

#### 4.2 动态头

```java
public class CustomHeaderWriter implements HeaderWriter {
    
    @Override
    public void writeHeaders(HttpServletRequest request,
            HttpServletResponse response) {
        
        // 根据请求动态添加头
        String userAgent = request.getHeader("User-Agent");
        
        if (userAgent != null && userAgent.contains("MSIE")) {
            // IE浏览器添加特殊头
            response.setHeader("X-IE-Compatibility", "IE=edge");
        }
        
        // 添加请求ID
        String requestId = UUID.randomUUID().toString();
        response.setHeader("X-Request-ID", requestId);
        
        // 添加时间戳
        response.setHeader("X-Response-Time", String.valueOf(System.currentTimeMillis()));
    }
}

// 配置
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .headers()
                .addHeaderWriter(new CustomHeaderWriter())
                .and()
            .authorizeRequests();
    }
}
```

### 5. CSP详细配置

#### 5.1 基本CSP配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .headers()
                .contentSecurityPolicy(getContentSecurityPolicy())
                .and()
            .authorizeRequests();
    }
    
    private String getContentSecurityPolicy() {
        return new StringBuilder()
            // 默认策略：所有资源都只能从同源加载
            .append("default-src 'self'; ")
            
            // 脚本：允许从同源和CDN加载，允许内联脚本
            .append("script-src 'self' https://cdn.example.com 'unsafe-inline' 'unsafe-eval'; ")
            
            // 样式：允许从同源和Google Fonts加载，允许内联样式
            .append("style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; ")
            
            // 字体：允许从同源和Google Fonts加载
            .append("font-src 'self' https://fonts.gstatic.com; ")
            
            // 图片：允许从同源、data URI和任何HTTPS源加载
            .append("img-src 'self' data: https:; ")
            
            // 连接（AJAX、WebSocket等）：只允许连接同源和API服务器
            .append("connect-src 'self' https://api.example.com wss://ws.example.com; ")
            
            // 媒体：只允许从同源加载
            .append("media-src 'self'; ")
            
            // 对象：禁止加载任何插件
            .append("object-src 'none'; ")
            
            // Frame：禁止任何嵌入
            .append("frame-ancestors 'none'; ")
            
            // Base URI：限制base标签的URI
            .append("base-uri 'self'; ")
            
            // Form Action：只允许提交到同源
            .append("form-action 'self'; ")
            
            // Manifest：只允许从同源加载
            .append("manifest-src 'self'; ")
            
            // Report URI：违规时报告到此URL
            .append("report-uri /csp-report-endpoint")
            
            .toString();
    }
}
```

#### 5.2 CSP报告处理器

```java
@RestController
@RequestMapping("/csp-report-endpoint")
public class CspReportController {
    
    private static final Logger logger = LoggerFactory.getLogger(CspReportController.class);
    
    @PostMapping
    public ResponseEntity<Void> handleCspReport(@RequestBody String reportJson) {
        
        // 解析CSP违规报告
        logger.warn("CSP Violation Report: {}", reportJson);
        
        // 可以保存到数据库或发送到监控系统
        cspViolationReportService.saveReport(reportJson);
        
        return ResponseEntity.ok().build();
    }
}
```

### 6. 禁用缓存头

默认情况下，Spring Security会添加禁用缓存的头。对于静态资源，可能需要启用缓存。

#### 6.1 全局配置

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    public void configure(WebSecurity web) throws Exception {
        web
            .ignoring()
                // 忽略静态资源
                .antMatchers("/css/**", "/js/**", "/images/**", "/webjars/**");
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .headers()
                // 禁用缓存控制（不推荐）
                //.cacheControl().disable()
                
                // 或者只对特定路径禁用
                .and()
            .authorizeRequests();
    }
}
```

#### 6.2 资源处理

```java
@Configuration
public class ResourceConfig implements WebMvcConfigurer {
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry
            .addResourceHandler("/static/**")
            .addResourceLocations("classpath:/static/")
            .setCacheControl(CacheControl.maxAge(365, TimeUnit.DAYS));
    }
}
```

### 7. 高级安全头配置

#### 7.1 CORS头

```java
@Configuration
public class CorsConfig {
    
    @Bean
    public FilterRegistrationBean<CorsFilter> corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        
        // 允许的源
        config.setAllowedOrigins(Arrays.asList("https://example.com"));
        
        // 允许的方法
        config.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        
        // 允许的头
        config.setAllowedHeaders(Arrays.asList("*"));
        
        // 允许凭证
        config.setAllowCredentials(true);
        
        // 预检请求缓存时间
        config.setMaxAge(3600L);
        
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        
        FilterRegistrationBean<CorsFilter> bean = new FilterRegistrationBean<>(
            new CorsFilter(source)
        );
        bean.setOrder(Ordered.HIGHEST_PRECEDENCE);
        
        return bean;
    }
}
```

#### 7.2 权限策略（Permissions-Policy）

```java
http.headers()
    .permissionsPolicy()
        .policy(
            "geolocation=(self), " +
            "fullscreen=(self), " +
            "camera=(self), " +
            "microphone=(), " +
            "payment=(), " +
            "usb=()"
        );
```

#### 7.3 Referrer策略

```java
http.headers()
    .referrerPolicy()
        .policy(ReferrerPolicy.NO_REFERRER)  // 不发送Referrer
        //.policy(ReferrerPolicy.SAME_ORIGIN)  // 只在同源请求时发送
        //.policy(ReferrerPolicy.STRICT_ORIGIN_WHEN_CROSS_ORIGIN)  // 跨源HTTPS时发送Origin
```

### 8. 完整示例

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // 安全头配置
            .headers()
                // 缓存控制
                .cacheControl()
                
                // 内容类型选项
                .and()
                .contentTypeOptions()
                
                // 框架选项
                .and()
                .frameOptions()
                    .sameOrigin()
                
                // XSS保护
                .and()
                .xssProtection()
                    .headerValue("1; mode=block")
                
                // HSTS
                .and()
                .httpStrictTransportSecurity()
                    .includeSubDomains(true)
                    .maxAgeInSeconds(31536000)
                
                // CSP
                .and()
                .contentSecurityPolicy(
                    "default-src 'self'; " +
                    "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://cdn.example.com; " +
                    "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; " +
                    "font-src 'self' https://fonts.gstatic.com; " +
                    "img-src 'self' data: https:; " +
                    "connect-src 'self' https://api.example.com; " +
                    "object-src 'none'; " +
                    "frame-ancestors 'none'"
                )
                
                // 权限策略
                .and()
                .permissionsPolicy()
                    .policy(
                        "geolocation=(self), " +
                        "camera=(self), " +
                        "microphone=(), " +
                        "fullscreen=(self)"
                    )
                
                // Referrer策略
                .and()
                .referrerPolicy()
                    .policy(ReferrerPolicy.STRICT_ORIGIN_WHEN_CROSS_ORIGIN)
                
                // 自定义头
                .and()
                .addHeaderWriter(new StaticHeaderWriter("X-Application-Name", "MySecureApp"))
                .addHeaderWriter(new CustomHeaderWriter())
                
                .and()
            // CSRF配置
            .csrf()
                .csrfTokenRepository(csrfTokenRepository())
                .ignoringAntMatchers("/api/public/**")
                .and()
            // 授权配置
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin();
    }
    
    @Bean
    public CsrfTokenRepository csrfTokenRepository() {
        CookieCsrfTokenRepository repository = new CookieCsrfTokenRepository();
        repository.setCookieHttpOnly(false);
        return repository;
    }
}
```

### 9. FAQ

**Q1: Content-Security-Policy中的'unsafe-inline'是什么？为什么需要它？**

A: `'unsafe-inline'`允许使用内联脚本和样式，虽然名称中包含"unsafe"，但在很多情况下是必需的：

```html
<!-- 内联脚本 -->
<script>alert('Hello');</script>
<button onclick="doSomething()">Click</button>

<!-- 内联样式 -->
<div style="color: red;">Red text</div>
```

更安全的替代方案：

1. **使用nonce（一次性数字）**：
```html
<!-- 服务端生成 -->
<script nonce="EDNnf03nceIOfn39fn3e9h3sdfa">
    // 脚本内容
</script>

// CSP
script-src 'self' 'nonce-EDNnf03nceIOfn39fn3e9h3sdfa';
```

2. **使用hash**：
```html
<!-- 计算内容的hash -->
<script>alert('Hello');</script>

// CSP
script-src 'self' 'sha256-qznLcsROx4GACP2dm0UCKCzCG+HiZ1guq6ZZDob/Tng=';
```

3. **将内联内容移到外部文件**（推荐）

**Q2: HSTS如何工作？为什么需要它？**

A: HSTS（HTTP Strict Transport Security）强制浏览器使用HTTPS：

```
1. 首次HTTPS访问，服务器返回Strict-Transport-Security头
2. 浏览器在指定时间内（如max-age=31536000）只使用HTTPS访问该域名
3. 即使用户输入http://，浏览器也会自动转换为https://
4. 防止中间人攻击和SSL剥离攻击
```

注意事项：
- **不要在HTTP上设置HSTS**（因为可能被劫持）
- 首次使用时需要谨慎（可以使用较短的max-age测试）
- 考虑加入HSTS Preload列表（永久生效）

**Q3: X-Frame-Options和CSP的frame-ancestors有什么区别？**

A: 
- **X-Frame-Options**: 旧标准，只控制iframe嵌入
  - `DENY`: 禁止所有嵌入
  - `SAMEORIGIN`: 只允许同源嵌入
  - `ALLOW-FROM`: 已废弃

- **CSP frame-ancestors**: 新标准，功能更强大
  - 可以指定多个允许的源
  - 支持更复杂的规则
  - 更好的未来兼容性

建议：同时使用两者以获得更好的兼容性

```java
http.headers()
    .frameOptions().sameOrigin()
    .and()
    .contentSecurityPolicy("frame-ancestors 'self' https://trusted.example.com");
```

**Q4: 如何实现CSP的渐进式增强？**

A: 使用Content-Security-Policy-Report-Only：

```java
// 第一步：报告模式（不阻止，只报告）
http.headers()
    .contentSecurityPolicyReportOnly(
        "default-src 'self'; script-src 'self' https://cdn.example.com"
    );

// 收集违规报告，调整策略
// 第二步：启用强制模式
http.headers()
    .contentSecurityPolicy(
        "default-src 'self'; script-src 'self' https://cdn.example.com"
    );
```

违规报告处理器：
```java
@PostMapping("/csp-report")
public ResponseEntity<Void> handleReport(@RequestBody String report) {
    // 解析并记录
    CSPViolationReport violation = parseReport(report);
    cspReportService.save(violation);
    return ResponseEntity.ok().build();
}
```

**Q5: 如何测试安全头配置是否正确？**

A: 使用多种工具：

1. **浏览器开发者工具**：
   - Network标签查看响应头

2. **curl命令**：
```bash
curl -I https://example.com
```

3. **在线工具**：
   - Security Headers (https://securityheaders.com/)
   - Mozilla Observatory (https://observatory.mozilla.org/)

4. **OWASP ZAP**：自动扫描安全配置

5. **单元测试**：
```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
public class SecurityHeadersTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    public void testSecurityHeaders() throws Exception {
        mockMvc.perform(get("/secure"))
            .andExpect(status().isOk())
            .andExpect(header().exists("X-Content-Type-Options"))
            .andExpect(header().string("X-Content-Type-Options", "nosniff"))
            .andExpect(header().exists("X-Frame-Options"))
            .andExpect(header().string("X-Frame-Options", "SAMEORIGIN"))
            .andExpect(header().exists("Content-Security-Policy"))
            .andExpect(header().exists("X-XSS-Protection"))
            .andExpect(header().string("X-XSS-Protection", "1; mode=block"));
    }
}
```

### 10. 思考题

1. 设计一个CSP策略生成器，根据应用使用的资源自动生成合适的CSP策略。

2. 在微服务架构中，如何统一管理所有服务的安全头配置？

3. 如何实现动态的安全头配置（如根据环境、用户角色等调整）？

4. 分析安全头对性能的影响，如何平衡安全性和性能？

5. 如何设计一个CSP违规报告系统，帮助开发者快速定位和修复问题？

---

# 总结

本课程讲义第二部分详细介绍了Spring Security的授权机制和会话管理两个核心模块：

## 模块3 - 授权机制详解

**知识点9：授权核心概念**
- AccessDecisionManager的三种实现及其使用场景
- AccessDecisionVoter的投票机制
- SecurityMetadataSource的作用

**知识点10：过滤器链详解**
- FilterChainProxy的工作原理
- 核心过滤器的源码分析
- 自定义过滤器实现

**知识点11：表达式式访问控制**
- Spring EL表达式的使用
- @PreAuthorize、@PostAuthorize等注解
- 自定义安全表达式

**知识点12：基于角色的访问控制**
- RBAC模型设计
- 角色层级配置
- 动态权限管理

## 模块4 - 会话管理与CSRF防护

**知识点13：会话管理**
- 会话创建策略
- 会话固定攻击防护
- 并发会话控制
- 分布式会话管理

**知识点14：CSRF防护**
- CSRF攻击原理
- CsrfFilter源码分析
- CsrfTokenRepository实现
- 前后端集成

**知识点15：安全头配置**
- 各种安全头的作用
- CSP配置详解
- 自定义HeaderWriter
- 完整安全配置示例

每个知识点都包含了：
- 深度的源码分析和注释
- 完整的可运行代码示例
- 常见问题解答
- 思考题和实践建议

希望这份讲义能帮助你深入理解Spring Security的核心机制！
