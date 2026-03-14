# Spring Security 5.3.9.RELEASE 课程讲义（第四部分）

## 模块7：安全测试与监控

### 知识点21：安全测试

#### 21.1 概述

安全测试是确保应用程序安全性的关键环节。Spring Security 提供了完善的测试支持，使开发者能够验证安全配置的正确性、测试认证和授权逻辑，以及确保应用程序在各种安全场景下的行为符合预期。

#### 21.2 核心测试依赖

首先，让我们添加必要的测试依赖：

```xml
<!-- pom.xml -->
<dependencies>
    <!-- Spring Security Test -->
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-test</artifactId>
        <version>5.3.9.RELEASE</version>
        <scope>test</scope>
    </dependency>
    
    <!-- Spring Boot Test (如果使用 Spring Boot) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <version>2.4.5</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

#### 21.3 @WithMockUser 注解详解

`@WithMockUser` 是最常用的测试注解，用于模拟一个认证用户。让我们深入分析其实现原理和使用方式。

**源码分析：WithMockUser**

```java
// org.springframework.security.test.context.support.WithMockUser

@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.METHOD, ElementType.TYPE })
@Documented
@Inherited
@WithSecurityContext(factory = WithMockUserSecurityContextFactory.class)
public @interface WithMockUser {
    
    /**
     * 用户名，默认为 "user"
     * 这个值会被设置为 UsernamePasswordAuthenticationToken 的 principal
     */
    String value() default "user";
    
    /**
     * 用户名，与 value 属性互斥，如果设置了 username，则 value 会被忽略
     */
    String username() default "";
    
    /**
     * 密码，默认为 "password"
     * 注意：这个密码在实际认证中不会被使用，因为已经是认证后的状态
     * 但为了完整性，仍然可以设置
     */
    String password() default "password";
    
    /**
     * 角色列表，会被自动加上 "ROLE_" 前缀
     * 例如: roles = "ADMIN" 会被转换为 GrantedAuthority("ROLE_ADMIN")
     */
    String[] roles() default {};
    
    /**
     * 权限列表，不会添加前缀
     * 例如: authorities = "READ_WRITE" 会被转换为 GrantedAuthority("READ_WRITE")
     */
    String[] authorities() default {};
}
```

**使用示例：**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class WithMockUserExampleTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    // 基本用法 - 使用默认用户
    @Test
    @WithMockUser
    public void testWithDefaultUser() throws Exception {
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.username").value("user"));
    }
    
    // 自定义用户名和角色
    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN", "USER"})
    public void testWithCustomUser() throws Exception {
        mockMvc.perform(get("/api/admin/dashboard"))
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.username").value("admin"));
    }
    
    // 自定义权限（不添加 ROLE_ 前缀）
    @Test
    @WithMockUser(username = "manager", authorities = {"READ", "WRITE"})
    public void testWithCustomAuthorities() throws Exception {
        mockMvc.perform(post("/api/resources"))
               .andExpect(status().isCreated());
    }
    
    // 类级别注解 - 该类下所有测试方法都使用此用户
    @RunWith(SpringRunner.class)
    @SpringBootTest
    @WithMockUser(username = "testuser", roles = "USER")
    public class ClassLevelWithMockUserTest {
        
        @Test
        public void testMethod1() throws Exception {
            // 自动使用 testuser
        }
        
        @Test
        public void testMethod2() throws Exception {
            // 自动使用 testuser
        }
    }
}
```

**源码分析：WithMockUserSecurityContextFactory**

```java
// org.springframework.security.test.context.support.WithMockUserSecurityContextFactory

/**
 * 负责创建 SecurityContext 的工厂类
 * 这是 @WithMockUser 注解真正起作用的地方
 */
final class WithMockUserSecurityContextFactory
        implements WithSecurityContextFactory<WithMockUser> {
    
    /**
     * 核心方法：根据注解创建 SecurityContext
     * 
     * @param withMockUser 注解实例
     * @return 填充好的 SecurityContext
     */
    @Override
    public SecurityContext createSecurityContext(WithMockUser withMockUser) {
        // 1. 创建用户名
        String username = StringUtils.hasLength(withMockUser.username()) 
                ? withMockUser.username() 
                : withMockUser.value();
        
        // 2. 创建密码（虽然认证后不需要密码，但为了完整性）
        String password = withMockUser.password();
        
        // 3. 构建权限列表
        List<GrantedAuthority> grantedAuthorities = new ArrayList<>();
        
        // 处理 roles（自动添加 ROLE_ 前缀）
        for (String role : withMockUser.roles()) {
            if (role.startsWith("ROLE_")) {
                grantedAuthorities.add(new SimpleGrantedAuthority(role));
            } else {
                grantedAuthorities.add(new SimpleGrantedAuthority("ROLE_" + role));
            }
        }
        
        // 处理 authorities（不添加前缀）
        for (String authority : withMockUser.authorities()) {
            grantedAuthorities.add(new SimpleGrantedAuthority(authority));
        }
        
        // 4. 创建 UserDetails 对象
        // 注意：这里使用的是 User 类，是 Spring Security 提供的简单实现
        User principal = new User(username, password, 
                true, true, true, true, // accountExpired, accountLocked, 
                // credentialsExpired, enabled
                grantedAuthorities);
        
        // 5. 创建 Authentication 对象
        // 设置为 authenticated = true，表示已经通过认证
        Authentication authentication = new UsernamePasswordAuthenticationToken(
                principal, principal.getPassword(), principal.getAuthorities());
        
        // 6. 创建并填充 SecurityContext
        SecurityContext context = SecurityContextHolder.createEmptyContext();
        context.setAuthentication(authentication);
        
        return context;
    }
}
```

#### 21.4 @WithUserDetails 注解

当需要使用自定义的 `UserDetails` 实现或从数据库加载用户时，`@WithUserDetails` 更为合适。

**源码分析：WithUserDetails**

```java
// org.springframework.security.test.context.support.WithUserDetails

@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.METHOD, ElementType.TYPE })
@Documented
@Inherited
@WithSecurityContext(factory = WithUserDetailsSecurityContextFactory.class)
public @interface WithUserDetails {
    
    /**
     * 用于查询用户的标识（通常是用户名）
     * 默认为 "user"
     */
    String value() default "user";
    
    /**
     * UserDetailsService bean 的名称
     * 如果不指定，会自动查找容器中的 UserDetailsService bean
     */
    String userDetailsServiceBeanName() default "";
    
    /**
     * 设置为 true 后，会在测试前设置上下文，测试后清除
     */
    boolean setupBefore() default true;
}
```

**使用示例：**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class WithUserDetailsExampleTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    /**
     * 需要先定义一个测试用的 UserDetailsService
     */
    @TestConfiguration
    static class TestConfig {
        
        @Bean
        public UserDetailsService userDetailsService() {
            // 这里可以使用内存中的用户，或者模拟数据库查询
            return new InMemoryUserDetailsManager(
                User.withUsername("admin")
                    .password("{noop}admin123")
                    .roles("ADMIN")
                    .build(),
                User.withUsername("user")
                    .password("{noop}user123")
                    .roles("USER")
                    .build()
            );
        }
    }
    
    // 使用默认的 UserDetailsService
    @Test
    @WithUserDetails("admin")
    public void testWithUserDetails() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
               .andExpect(status().isOk())
               .andExpect(jsonPath("$", hasSize(greaterThan(0))));
    }
    
    // 指定特定的 UserDetailsService
    @Test
    @WithUserDetails(value = "user", 
                     userDetailsServiceBeanName = "customUserDetailsService")
    public void testWithCustomUserDetailsService() throws Exception {
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isOk());
    }
}
```

**源码分析：WithUserDetailsSecurityContextFactory**

```java
// org.springframework.security.test.context.support.WithUserDetailsSecurityContextFactory

final class WithUserDetailsSecurityContextFactory
        implements WithSecurityContextFactory<WithUserDetails> {
    
    private ApplicationContext context;
    
    /**
     * 构造函数注入 ApplicationContext
     * Spring 会自动调用这个构造函数
     */
    public WithUserDetailsSecurityContextFactory(ApplicationContext context) {
        this.context = context;
    }
    
    @Override
    public SecurityContext createSecurityContext(WithUserDetails withUserDetails) {
        // 1. 获取用户名
        String username = withUserDetails.value();
        
        // 2. 获取 UserDetailsService
        // 如果指定了 bean 名称，则按名称获取；否则按类型获取
        UserDetailsService userDetailsService;
        if (StringUtils.hasText(withUserDetails.userDetailsServiceBeanName())) {
            userDetailsService = context.getBean(
                    withUserDetails.userDetailsServiceBeanName(), 
                    UserDetailsService.class);
        } else {
            // 如果容器中有多个 UserDetailsService，会抛出异常
            // 这时需要明确指定 bean 名称
            userDetailsService = context.getBean(UserDetailsService.class);
        }
        
        // 3. 加载 UserDetails
        // 这里会调用我们自定义的 UserDetailsService.loadUserByUsername()
        UserDetails userDetails = userDetailsService.loadUserByUsername(username);
        
        // 4. 创建 Authentication
        Authentication authentication = new UsernamePasswordAuthenticationToken(
                userDetails, 
                userDetails.getPassword(), 
                userDetails.getAuthorities());
        
        // 5. 创建并返回 SecurityContext
        SecurityContext context = SecurityContextHolder.createEmptyContext();
        context.setAuthentication(authentication);
        return context;
    }
}
```

#### 21.5 @WithAnonymousUser 注解

测试匿名访问的场景。

```java
// org.springframework.security.test.context.support.WithAnonymousUser

@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.METHOD, ElementType.TYPE })
@Documented
@Inherited
@WithSecurityContext(factory = WithAnonymousUserSecurityContextFactory.class)
public @interface WithAnonymousUser {
}
```

**使用示例：**

```java
@Test
@WithAnonymousUser
public void testAnonymousAccess() throws Exception {
    mockMvc.perform(get("/api/public/info"))
           .andExpect(status().isOk())
           .andExpect(jsonPath("$.message").value("Welcome, anonymous user!"));
}

@Test
@WithAnonymousUser
public void testAnonymousAccessDenied() throws Exception {
    mockMvc.perform(get("/api/private/data"))
           .andExpect(status().isUnauthorized());
}
```

#### 21.6 MockMvc 安全测试完整示例

```java
package com.example.security.test;

import static org.springframework.security.test.web.servlet.request.SecurityMockMvcRequestBuilders.*;
import static org.springframework.security.test.web.servlet.response.SecurityMockMvcResultMatchers.*;
import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.*;
import static org.hamcrest.Matchers.*;
import static org.mockito.Mockito.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.Import;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

/**
 * 综合安全测试示例
 */
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class ComprehensiveSecurityTest {
    
    @Autowired
    private WebApplicationContext context;
    
    @Autowired
    private FilterChainProxy springSecurityFilterChain;
    
    private MockMvc mockMvc;
    
    /**
     * 在每个测试前设置 MockMvc
     * apply(springSecurity()) 是关键，它会应用 Spring Security 的过滤器链
     */
    @Before
    public void setup() {
        mockMvc = MockMvcBuilders
                .webAppContextSetup(context)
                .apply(springSecurity())  // 应用 Spring Security
                .build();
    }
    
    // ========== 测试 1: 表单登录测试 ==========
    
    @Test
    public void testSuccessfulLogin() throws Exception {
        mockMvc.perform(formLogin()  // 使用 SecurityMockMvcRequestBuilders.formLogin()
                .user("admin")      // 默认字段名是 "username"
                .password("admin123"))  // 默认字段名是 "password"
               .andExpect(authenticated());  // 验证认证成功
        
        // 也可以添加更多断言
        mockMvc.perform(formLogin()
                .user("admin")
                .password("admin123"))
               .andExpect(authenticated().withUsername("admin"))
               .andExpect(authenticated().withRoles("ADMIN"));
    }
    
    @Test
    public void testFailedLogin() throws Exception {
        mockMvc.perform(formLogin()
                .user("wronguser")
                .password("wrongpass"))
               .andExpect(unauthenticated());  // 验证认证失败
    }
    
    // ========== 测试 2: 登出测试 ==========
    
    @Test
    @WithMockUser
    public void testLogout() throws Exception {
        mockMvc.perform(logout())  // SecurityMockMvcRequestBuilders.logout()
               .andExpect(status().is3xxRedirection())
               .andExpect(redirectedUrl("/login?logout"));
    }
    
    // ========== 测试 3: CSRF 保护测试 ==========
    
    @Test
    @WithMockUser
    public void testPostWithoutCsrfToken() throws Exception {
        // 没有 CSRF token 的 POST 请求应该被拒绝
        mockMvc.perform(post("/api/resources")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"test\"}"))
               .andExpect(status().isForbidden());  // CSRF token 缺失
    }
    
    @Test
    @WithMockUser
    public void testPostWithCsrfToken() throws Exception {
        // 使用 with(csrf()) 添加 CSRF token
        mockMvc.perform(post("/api/resources")
                .with(csrf())  // SecurityMockMvcRequestPostProcessors.csrf()
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"test\"}"))
               .andExpect(status().isCreated());
    }
    
    // ========== 测试 4: 方法级安全测试 ==========
    
    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testUserAccessToUserEndpoint() throws Exception {
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isOk());
    }
    
    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testUserAccessDeniedToAdminEndpoint() throws Exception {
        mockMvc.perform(get("/api/admin/dashboard"))
               .andExpect(status().isForbidden());  // 权限不足
    }
    
    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAdminAccessToAdminEndpoint() throws Exception {
        mockMvc.perform(get("/api/admin/dashboard"))
               .andExpect(status().isOk());
    }
    
    // ========== 测试 5: @PreAuthorize 测试 ==========
    
    @Test
    @WithMockUser(username = "owner", roles = "USER")
    public void testPreAuthorizeWithSameUser() throws Exception {
        // 假设方法有 @PreAuthorize("#username == authentication.name")
        mockMvc.perform(get("/api/users/{username}/profile", "owner"))
               .andExpect(status().isOk());
    }
    
    @Test
    @WithMockUser(username = "other", roles = "USER")
    public void testPreAuthorizeWithDifferentUser() throws Exception {
        // 访问其他用户的资源应该被拒绝
        mockMvc.perform(get("/api/users/{username}/profile", "owner"))
               .andExpect(status().isForbidden());
    }
    
    // ========== 测试 6: 安全头测试 ==========
    
    @Test
    public void testSecurityHeaders() throws Exception {
        mockMvc.perform(get("/api/public/data"))
               .andExpect(status().isOk())
               .andExpect(header().exists("X-Content-Type-Options"))
               .andExpect(header().string("X-Content-Type-Options", "nosniff"))
               .andExpect(header().exists("X-Frame-Options"))
               .andExpect(header().string("X-Frame-Options", "DENY"))
               .andExpect(header().exists("X-XSS-Protection"))
               .andExpect(header().string("X-XSS-Protection", "1; mode=block"));
    }
    
    // ========== 测试 7: 匿名访问测试 ==========
    
    @Test
    @WithAnonymousUser
    public void testAnonymousAccessAllowed() throws Exception {
        mockMvc.perform(get("/api/public/info"))
               .andExpect(status().isOk());
    }
    
    @Test
    @WithAnonymousUser
    public void testAnonymousAccessDenied() throws Exception {
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isUnauthorized());  // 或 302 重定向到登录页
    }
    
    // ========== 测试 8: 自定义 UserDetails 测试 ==========
    
    /**
     * 测试使用自定义的 UserDetails 实现
     * 假设我们有 CustomUser 类实现了 UserDetails
     */
    @Test
    public void testWithCustomUserDetails() throws Exception {
        // 创建自定义的 UserDetails
        CustomUser customUser = new CustomUser(
            "customuser",
            "password",
            Collections.singleton(new SimpleGrantedAuthority("ROLE_USER")),
            "custom-attribute-value"
        );
        
        // 使用 withUser() 设置自定义 UserDetails
        mockMvc.perform(get("/api/user/attributes")
                .with(user(customUser)))  // SecurityMockMvcRequestPostProcessors.user()
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.customAttribute").value("custom-attribute-value"));
    }
    
    // ========== 测试 9: Remember Me 测试 ==========
    
    @Test
    public void testRememberMeLogin() throws Exception {
        mockMvc.perform(formLogin()
                .user("admin")
                .password("admin123")
                .rememberMe())  // 启用 remember-me
               .andExpect(authenticated())
               .andExpect(cookie().exists("remember-me"));
    }
    
    // ========== 测试 10: HTTP Basic 认证测试 ==========
    
    @Test
    public void testHttpBasicAuthentication() throws Exception {
        mockMvc.perform(get("/api/user/profile")
                .with(httpBasic("admin", "admin123")))  // HTTP Basic 认证
               .andExpect(status().isOk())
               .andExpect(authenticated());
    }
}
```

#### 21.7 测试安全配置

```java
package com.example.security.config;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

/**
 * 安全配置测试
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class SecurityConfigTest {
    
    @Autowired
    private WebApplicationContext context;
    
    @Test
    public void testSecurityConfiguration() throws Exception {
        // 可以验证安全配置是否正确加载
        // 例如：验证特定的过滤器链
        HttpSecurity httpSecurity = context.getBean(HttpSecurity.class);
        assertNotNull(httpSecurity);
    }
    
    @Test
    @WithMockUser
    public void testCsrfIsEnabled() throws Exception {
        MockMvc mockMvc = MockMvcBuilders
                .webAppContextSetup(context)
                .apply(springSecurity())
                .build();
        
        mockMvc.perform(get("/api/test"))
               .andExpect(status().isOk());
    }
}
```

#### 21.8 SecurityContext 测试工具

```java
package com.example.security.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Collections;

/**
 * SecurityContext 手动设置测试
 */
@RunWith(SpringRunner.class)
public class SecurityContextTest {
    
    @Test
    public void testManualSecurityContextSetup() {
        // 方式 1: 直接设置 SecurityContext
        SecurityContextHolder.getContext().setAuthentication(
            new UsernamePasswordAuthenticationToken(
                "testuser",
                null,
                Collections.singletonList(new SimpleGrantedAuthority("ROLE_USER"))
            )
        );
        
        try {
            // 执行需要认证的操作
            String username = SecurityContextHolder.getContext()
                    .getAuthentication()
                    .getName();
            System.out.println("Current user: " + username);
        } finally {
            // 清理 SecurityContext
            SecurityContextHolder.clearContext();
        }
    }
    
    @Test
    @WithMockUser(username = "testuser", roles = "USER")
    public void testWithAnnotatedSecurityContext() {
        // 使用注解方式设置 SecurityContext
        // 测试方法执行时，SecurityContext 已经设置好
        String username = SecurityContextHolder.getContext()
                .getAuthentication()
                .getName();
        
        assertEquals("testuser", username);
    }
}
```

#### 21.9 集成测试最佳实践

```java
package com.example.security.test;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.security.test.context.support.WithUserDetails;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.context.WebApplicationContext;

import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

/**
 * 集成测试最佳实践
 */
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional  // 测试后回滚事务
public class SecurityIntegrationTest {
    
    @Autowired
    private WebApplicationContext context;
    
    private MockMvc mockMvc;
    
    @Before
    public void setup() {
        mockMvc = MockMvcBuilders
                .webAppContextSetup(context)
                .apply(springSecurity())
                .build();
    }
    
    /**
     * 最佳实践 1: 使用 @Transactional 回滚数据库状态
     * 这样每个测试都有干净的初始状态
     */
    @Test
    @Transactional
    @WithUserDetails("admin")
    public void testWithTransactional() throws Exception {
        // 这个测试执行的所有数据库操作都会被回滚
        mockMvc.perform(post("/api/users")
                .with(csrf())
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"username\":\"temp\",\"password\":\"temp\"}"))
               .andExpect(status().isCreated());
        
        // 测试结束后，这条记录会被回滚，不会污染数据库
    }
    
    /**
     * 最佳实践 2: 测试完整的认证流程
     */
    @Test
    public void testCompleteAuthenticationFlow() throws Exception {
        // 1. 测试未认证访问
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().is3xxRedirection())
               .andExpect(redirectedUrl("http://localhost/login"));
        
        // 2. 测试登录
        mockMvc.perform(formLogin()
                .user("admin")
                .password("admin123"))
               .andExpect(authenticated());
        
        // 3. 测试认证后访问
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isOk());
        
        // 4. 测试登出
        mockMvc.perform(logout())
               .andExpect(status().is3xxRedirection());
        
        // 5. 测试登出后访问
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().is3xxRedirection());
    }
    
    /**
     * 最佳实践 3: 测试权限边界条件
     */
    @Test
    @WithMockUser(roles = "USER")
    public void testPermissionBoundary() throws Exception {
        // 用户角色的权限边界测试
        
        // 应该允许的
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isOk());
        
        // 应该拒绝的
        mockMvc.perform(get("/api/admin/settings"))
               .andExpect(status().isForbidden());
        
        // 不存在的资源 - 应该返回 404，而不是 403
        mockMvc.perform(get("/api/nonexistent"))
               .andExpect(status().isNotFound());
    }
}
```

#### 21.10 测试异常处理

```java
package com.example.security.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

/**
 * 测试安全异常处理
 */
@RunWith(SpringRunner.class)
@WebMvcTest
public class SecurityExceptionHandlerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    @WithMockUser(username = "testuser", roles = "USER")
    public void testAccessDeniedHandler() throws Exception {
        // 模拟用户尝试访问无权限的资源
        mockMvc.perform(get("/api/admin/users"))
               .andExpect(status().isForbidden())
               .andExpect(jsonPath("$.error").value("Access Denied"))
               .andExpect(jsonPath("$.message").exists());
    }
    
    @Test
    public void testAuthenticationEntryPoint() throws Exception {
        // 测试未认证的访问
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isUnauthorized())
               .andExpect(jsonPath("$.error").value("Unauthorized"));
    }
    
    @Test
    @WithMockUser
    public void testAccountExpiredException() throws Exception {
        // 模拟账户过期的情况
        when(userService.loadUserByUsername("expired"))
                .thenThrow(new AccountExpiredException("Account has expired"));
        
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isUnauthorized())
               .andExpect(jsonPath("$.error").value("Account Expired"));
    }
    
    @Test
    @WithMockUser
    public void testCredentialsExpiredException() throws Exception {
        // 模拟凭据过期的情况
        when(userService.loadUserByUsername("user"))
                .thenThrow(new CredentialsExpiredException("Password has expired"));
        
        mockMvc.perform(get("/api/user/profile"))
               .andExpect(status().isUnauthorized())
               .andExpect(jsonPath("$.error").value("Credentials Expired"));
    }
}
```

#### 21.11 性能测试与安全

```java
package com.example.security.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

/**
 * 安全相关的性能测试
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class SecurityPerformanceTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    /**
     * 测试安全过滤器链的性能影响
     */
    @Test
    @WithMockUser
    public void testSecurityFilterPerformance() throws Exception {
        long startTime = System.currentTimeMillis();
        
        for (int i = 0; i < 100; i++) {
            mockMvc.perform(get("/api/public/data"))
                   .andExpect(status().isOk());
        }
        
        long endTime = System.currentTimeMillis();
        long duration = endTime - startTime;
        
        System.out.println("100 requests took: " + duration + "ms");
        System.out.println("Average per request: " + (duration / 100.0) + "ms");
        
        // 确保平均响应时间在合理范围内
        assertTrue(duration / 100.0 < 100);  // 平均每个请求不超过 100ms
    }
    
    /**
     * 测试大量权限检查的性能
     */
    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN", "USER", "MANAGER"})
    public void testPermissionCheckPerformance() throws Exception {
        long startTime = System.currentTimeMillis();
        
        for (int i = 0; i < 1000; i++) {
            mockMvc.perform(get("/api/admin/resource-" + (i % 100)))
                   .andExpect(status().isOk());
        }
        
        long endTime = System.currentTimeMillis();
        System.out.println("1000 permission checks took: " + (endTime - startTime) + "ms");
    }
}
```

#### 21.12 MockMvc 源码深度分析

让我们深入分析 `SecurityMockMvcConfigurers` 的实现原理：

```java
// org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers

/**
 * SecurityMockMvcConfigurers 是应用 Spring Security 到 MockMvc 的核心类
 */
public final class SecurityMockMvcConfigurers {
    
    /**
     * 创建一个 Spring security MockMvc 配置器
     * 
     * 工作原理：
     * 1. 从 WebApplicationContext 中获取 FilterChainProxy bean
     * 2. 将 FilterChainProxy 添加到 MockMvc 的过滤器链中
     * 3. 这样，每个测试请求都会经过完整的 Spring Security 过滤器链
     * 
     * @return MockMvcConfigurer
     */
    public static MockMvcConfigurer springSecurity() {
        return new MockMvcConfigurer() {
            
            private FilterChainProxy springSecurityFilterChain;
            
            /**
             * 在 MockMvc 初始化前调用
             * 获取 Spring Security 的 FilterChainProxy
             */
            @Override
            public void beforeMockMvcCreated(
                    WebApplicationContext context,
                    MockMvcBuilder builder) {
                
                // 从 Spring 容器中获取 FilterChainProxy
                // FilterChainProxy 是 Spring Security 的核心过滤器
                this.springSecurityFilterChain = context.getBean(
                        FilterChainProxy.class);
                
                // 添加 springSecurityFilterChain 到 MockMvc
                // 这样所有测试请求都会经过安全过滤器链
                builder.addFilters(this.springSecurityFilterChain);
            }
            
            /**
             * 可以在这里添加额外的配置
             */
            @Override
            public void afterConfigurerAdded(ConfigurableMockMvcBuilder builder) {
                // 默认实现为空
            }
        };
    }
}
```

**FilterChainProxy 核心分析：**

```java
// org.springframework.security.web.FilterChainProxy

/**
 * FilterChainProxy 是 Spring Security 的核心过滤器
 * 它管理着多个 SecurityFilterChain
 */
public class FilterChainProxy extends GenericFilterBean {
    
    private static final String FILTER_APPLIED = FilterChainProxy.class.getName()
            ".applied";
    
    /**
     * 维护多个 SecurityFilterChain
     * 每个 SecurityFilterChain 可以处理不同的请求模式
     */
    private List<SecurityFilterChain> filterChains;
    
    /**
     * FilterChainProxy 的核心方法
     * 每个 HTTP 请求都会经过这个方法
     * 
     * @param request  HTTP 请求
     * @param response HTTP 响应
     * @param chain    Servlet 过滤器链
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        
        // 防止同一个过滤器被执行多次
        boolean clearContext = isApplyOnce();
        
        if (clearContext) {
            // 标记过滤器已应用
            request.setAttribute(FILTER_APPLIED, Boolean.TRUE);
            
            // 如果需要，在请求开始前清理 SecurityContext
            if (this.filterChainObserver != null) {
                this.filterChainObserver.onFilterChainStarted(request, response);
            }
        }
        
        try {
            // 核心：调用 doFilterInternal 处理请求
            doFilterInternal((HttpServletRequest) request, 
                            (HttpServletResponse) response, chain);
        } finally {
            if (clearContext) {
                // 请求处理完成后清理
                request.removeAttribute(FILTER_APPLIED);
                
                // 清理 SecurityContextHolder
                // 这对于防止内存泄漏非常重要
                SecurityContextHolder.clearContext();
                
                if (this.filterChainObserver != null) {
                    this.filterChainObserver.onFilterChainFinished(request, 
                            response, null);
                }
            }
        }
    }
    
    /**
     * 内部过滤方法
     * 选择合适的 SecurityFilterChain 并执行
     */
    private void doFilterInternal(HttpServletRequest request, 
                                  HttpServletResponse response,
                                  FilterChain chain) 
            throws IOException, ServletException {
        
        // 1. 获取虚拟防火墙（VirtualFilterChain）
        // VirtualFilterChain 是 Spring Security 的一个内部实现
        // 它负责执行实际的过滤器链
        FirewalledRequest fwRequest = firewall.getFirewalledRequest(request);
        HttpServletResponse fwResponse = firewall.getFirewalledResponse(response);
        
        // 2. 选择合适的 SecurityFilterChain
        // 根据请求的 URL 匹配第一个合适的 SecurityFilterChain
        List<Filter> filters = getFilters(fwRequest.getServletPath(),
                fwRequest.getPathInfo());
        
        // 3. 如果没有匹配的过滤器链，直接传递给下一个过滤器
        if (filters == null || filters.size() == 0) {
            if (logger.isDebugEnabled()) {
                logger.debug(Debug.toString(request)
                        + " has an empty filter list");
            }
            
            chain.doFilter(fwRequest, fwResponse);
            return;
        }
        
        // 4. 创建虚拟过滤器链并执行
        VirtualFilterChain vfc = new VirtualFilterChain(fwRequest, chain, filters);
        vfc.doFilter(fwRequest, fwResponse);
    }
    
    /**
     * 根据请求路径获取匹配的过滤器列表
     */
    private List<Filter> getFilters(String servletPath, String pathInfo) {
        // 遍历所有 SecurityFilterChain，找到第一个匹配的
        for (SecurityFilterChain chain : filterChains) {
            if (chain.matches(servletPath, pathInfo)) {
                return chain.getFilters();
            }
        }
        return null;
    }
    
    /**
     * VirtualFilterChain 是 FilterChainProxy 的内部类
     * 它实现了 Servlet 的 FilterChain 接口
     * 
     * VirtualFilterChain 维护了当前执行到的过滤器索引
     * 并按照顺序执行所有过滤器
     */
    private static class VirtualFilterChain implements FilterChain {
        
        private final FilterChain originalChain;
        private final List<Filter> additionalFilters;
        private final FirewalledRequest firewalledRequest;
        private final int size;
        private int currentPosition = 0;
        
        /**
         * VirtualFilterChain 的核心方法
         * 按顺序执行所有过滤器
         */
        @Override
        public void doFilter(ServletRequest request, ServletResponse response)
                throws IOException, ServletException {
            
            // 如果所有过滤器都已执行完毕，则调用原始过滤器链
            if (currentPosition == size) {
                if (logger.isDebugEnabled()) {
                    logger.debug(Debug.toString(request)
                            + " reached end of additional filter chain; "
                            + "proceeding with original chain");
                }
                
                // 调用原始过滤器链的下一个过滤器
                originalChain.doFilter(request, response);
            } else {
                // 获取下一个过滤器
                currentPosition++;
                
                Filter nextFilter = additionalFilters.get(currentPosition - 1);
                
                if (logger.isDebugEnabled()) {
                    logger.debug(Debug.toString(request)
                            + " at position " + currentPosition + " of "
                            + size + " in additional filter chain; "
                            + "firing Filter: '"
                            + nextFilter.getClass().getSimpleName() + "'");
                }
                
                // 执行下一个过滤器
                // 关键：传入 this (VirtualFilterChain 自身)
                // 这样，过滤器完成后会继续调用 VirtualFilterChain.doFilter()
                // 形成链式调用
                nextFilter.doFilter(request, response, this);
            }
        }
    }
}
```

#### 21.13 总结与FAQ

**本节总结：**

1. **测试注解体系**：
   - `@WithMockUser`：快速模拟认证用户
   - `@WithUserDetails`：使用自定义 UserDetailsService
   - `@WithAnonymousUser`：测试匿名访问

2. **MockMvc 集成**：
   - `SecurityMockMvcConfigurers.springSecurity()`：应用安全过滤器链
   - `formLogin()`、`logout()`、`httpBasic()`：便捷的认证方法
   - `with(csrf())`：添加 CSRF token

3. **测试最佳实践**：
   - 使用 `@Transactional` 保持测试数据隔离
   - 测试完整的认证流程
   - 测试权限边界条件
   - 测试异常处理

**FAQ：**

**Q1: @WithMockUser 和 @WithUserDetails 的主要区别是什么？**

A: 
- `@WithMockUser` 创建一个简单的内存用户，不涉及数据库查询，速度快，适合快速测试。
- `@WithUserDetails` 通过 `UserDetailsService` 从数据库加载用户，可以测试真实的用户数据和自定义的 `UserDetails` 实现。

**Q2: 为什么在测试中需要使用 apply(springSecurity())？**

A: 因为 Spring Security 的过滤器链（FilterChainProxy）需要在测试中被应用。`apply(springSecurity())` 会将 `FilterChainProxy` 添加到 MockMvc 的过滤器链中，确保每个测试请求都经过完整的安全过滤。

**Q3: 如何测试 CSRF 保护是否启用？**

A: 发送 POST/PUT/DELETE 请求时不添加 CSRF token，应该返回 403 Forbidden。然后使用 `with(csrf())` 添加 token，应该能够正常访问。

**Q4: 测试完成后 SecurityContext 会自动清理吗？**

A: 是的，FilterChainProxy 会在请求处理完成后自动清理 SecurityContextHolder。但如果手动设置了 SecurityContext，需要在 finally 块中清理。

**Q5: 如何测试方法级安全（如 @PreAuthorize）？**

A: 需要确保测试类上有 `@EnableGlobalMethodSecurity(prePostEnabled = true)`，然后使用 `@WithMockUser` 设置不同权限的用户，验证访问控制是否正确。

**思考题：**

1. 如何编写测试验证密码编码器的正确性？
2. 如何测试会话管理（会话固定攻击防护）？
3. 如何模拟 CSRF token 过期的场景？
4. 如何测试自定义的 AccessDecisionManager？
5. 如何为 REST API 编写安全测试（无状态的 JWT 认证）？

---

### 知识点22：安全审计与监控

#### 22.1 概述

安全审计与监控是企业级应用的重要组成部分。通过记录和分析安全事件，可以：
- 检测潜在的安全威胁
- 满足合规性要求（如 SOX、PCI-DSS）
- 进行事后调查和取证
- 优化安全策略

Spring Security 提供了完善的事件发布机制，我们可以通过监听这些事件来实现安全审计和监控。

#### 22.2 Spring Security 事件体系

**核心事件类层次结构：**

```
ApplicationEvent (Spring Framework)
    ↓
AuthenticationEvent (Spring Security 抽象基类)
    ↓
    ├── AuthenticationSuccessEvent (认证成功)
    ├── AbstractAuthenticationFailureEvent (认证失败抽象类)
    │   ├── AuthenticationFailureBadCredentialsEvent (凭据错误)
    │   ├── AuthenticationFailureDisabledEvent (账户禁用)
    │   ├── AuthenticationFailureExpiredEvent (账户过期)
    │   ├── AuthenticationFailureLockedEvent (账户锁定)
    │   ├── AuthenticationFailureProviderNotFoundEvent (Provider 未找到)
    │   ├── AuthenticationFailureCredentialsExpiredEvent (凭据过期)
    │   ├── AuthenticationFailureProxyUntrustedEvent (代理不受信任)
    │   └── AuthenticationFailureServiceExceptionEvent (服务异常)
    ├── AuthenticationSwitchUserEvent (用户切换)
    ├── InteractiveAuthenticationSuccessEvent (交互式认证成功)
    └── AnonymousAuthenticationEvent (匿名认证)
```

#### 22.3 核心事件源码分析

**AuthenticationSuccessEvent 源码：**

```java
// org.springframework.security.authentication.event.AuthenticationSuccessEvent

/**
 * 认证成功事件
 * 当用户成功认证后发布
 */
public class AuthenticationSuccessEvent extends AuthenticationEvent {
    
    private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
    
    /**
     * 构造函数
     * @param authentication 认证对象，包含认证后的用户信息
     */
    public AuthenticationSuccessEvent(Authentication authentication) {
        super(authentication);
    }
}
```

**AbstractAuthenticationFailureEvent 源码：**

```java
// org.springframework.security.authentication.event.AbstractAuthenticationFailureEvent

/**
 * 认证失败事件的抽象基类
 * 所有认证失败事件都继承自此类
 */
public abstract class AbstractAuthenticationFailureEvent extends AuthenticationEvent {
    
    private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
    
    /**
     * 认证失败的异常信息
     */
    private final AuthenticationException exception;
    
    /**
     * 构造函数
     * @param authentication 认证对象（可能未完全认证）
     * @param exception       导致认证失败的异常
     */
    public AbstractAuthenticationFailureEvent(Authentication authentication,
            AuthenticationException exception) {
        super(authentication);
        this.exception = exception;
    }
    
    /**
     * 获取认证失败的异常
     */
    public AuthenticationException getException() {
        return exception;
    }
}
```

**AuthenticationFailureBadCredentialsEvent 源码：**

```java
// org.springframework.security.authentication.event.AuthenticationFailureBadCredentialsEvent

/**
 * 凭据错误事件
 * 当用户名或密码错误时发布
 */
public class AuthenticationFailureBadCredentialsEvent 
        extends AbstractAuthenticationFailureEvent {
    
    public AuthenticationFailureBadCredentialsEvent(
            Authentication authentication, 
            AuthenticationException exception) {
        super(authentication, exception);
    }
}
```

#### 22.4 事件发布机制源码分析

Spring Security 如何发布这些事件？让我们深入分析：

**AbstractAuthenticationManager（认证管理器基类）：**

```java
// org.springframework.security.authentication.AbstractAuthenticationManager

/**
 * AbstractAuthenticationManager 是认证的核心组件
 * 负责协调 ProviderManager 和事件发布
 */
public abstract class AbstractAuthenticationManager 
        implements AuthenticationManager, MessageSourceAware, InitializingBean {
    
    protected MessageSourceAccessor messages = SpringSecurityMessageSource.getAccessor();
    
    private AuthenticationEventPublisher eventPublisher;
    
    /**
     * 设置事件发布器
     * 默认使用 DefaultAuthenticationEventPublisher
     */
    public void setAuthenticationEventPublisher(
            AuthenticationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }
    
    /**
     * 认证方法
     * 发布认证成功和失败事件
     */
    public final Authentication authenticate(Authentication authentication)
            throws AuthenticationException {
        
        try {
            // 1. 调用子类的 doAuthenticate() 执行实际认证
            Authentication result = doAuthenticate(authentication);
            
            // 2. 认证成功，发布成功事件
            if (eventPublisher != null) {
                eventPublisher.publishAuthenticationSuccess(result);
            }
            
            return result;
            
        } catch (AuthenticationException e) {
            // 3. 认证失败，发布失败事件
            eventPublisher.publishAuthenticationFailure(
                    new BadCredentialsException("Bad credentials"), // 简化示例
                    authentication);
            
            throw e;
        }
    }
    
    /**
     * 抽象方法，由子类实现具体的认证逻辑
     */
    protected abstract Authentication doAuthenticate(Authentication authentication)
            throws AuthenticationException;
}
```

**DefaultAuthenticationEventPublisher（默认事件发布器）：**

```java
// org.springframework.security.authentication.DefaultAuthenticationEventPublisher

/**
 * 默认的认证事件发布器
 * 根据异常类型映射到具体的事件类
 */
public class DefaultAuthenticationEventPublisher 
        implements AuthenticationEventPublisher {
    
    private static final Log logger = LogFactory
            .getLog(DefaultAuthenticationEventPublisher.class);
    
    /**
     * 异常类型到事件类型的映射
     * Key: 异常的类名
     * Value: 事件类的 Constructor
     */
    private final LinkedHashMap<String, Constructor<? extends AbstractAuthenticationFailureEvent>> exceptionMappings 
            = new LinkedHashMap<>();
    
    private final ApplicationEventPublisher applicationEventPublisher;
    
    /**
     * 构造函数，初始化默认的异常映射
     */
    public DefaultAuthenticationEventPublisher(
            ApplicationEventPublisher applicationEventPublisher) {
        this.applicationEventPublisher = applicationEventPublisher;
        
        // 添加默认的异常映射
        // 当认证失败时，根据异常类型选择对应的事件
        addMapping(BadCredentialsException.class.getName(),
                AuthenticationFailureBadCredentialsEvent.class);
        addMapping(AccountExpiredException.class.getName(),
                AuthenticationFailureExpiredEvent.class);
        addMapping(DisabledException.class.getName(),
                AuthenticationFailureDisabledEvent.class);
        addMapping(LockedException.class.getName(),
                AuthenticationFailureLockedEvent.class);
        addMapping(CredentialsExpiredException.class.getName(),
                AuthenticationFailureCredentialsExpiredEvent.class);
        // ... 其他映射
    }
    
    /**
     * 发布认证成功事件
     */
    @Override
    public void publishAuthenticationSuccess(Authentication authentication) {
        if (applicationEventPublisher != null) {
            // 发布 AuthenticationSuccessEvent
            applicationEventPublisher.publishEvent(
                    new AuthenticationSuccessEvent(authentication));
        }
    }
    
    /**
     * 发布认证失败事件
     * 根据异常类型查找对应的事件类并发布
     */
    @Override
    public void publishAuthenticationFailure(
            AuthenticationException exception, 
            Authentication authentication) {
        
        if (applicationEventPublisher == null) {
            return;
        }
        
        // 1. 查找匹配的事件构造器
        Constructor<? extends AbstractAuthenticationFailureEvent> constructor = 
                getEventConstructor(exception);
        
        // 2. 如果找到匹配的事件类，创建并发布
        if (constructor != null) {
            try {
                AbstractAuthenticationFailureEvent event = constructor.newInstance(
                        authentication, exception);
                applicationEventPublisher.publishEvent(event);
            } catch (Exception e) {
                logger.error("Failed to publish authentication failure event", e);
            }
        }
    }
    
    /**
     * 根据异常类型查找对应的事件构造器
     */
    private Constructor<? extends AbstractAuthenticationFailureEvent> 
            getEventConstructor(AuthenticationException exception) {
        
        Class<?> exceptionClass = exception.getClass();
        
        // 遍历映射表，找到第一个匹配的
        for (Map.Entry<String, Constructor<? extends AbstractAuthenticationFailureEvent>> entry 
                : exceptionMappings.entrySet()) {
            try {
                Class<?> clazz = Class.forName(entry.getKey());
                if (clazz.isAssignableFrom(exceptionClass)) {
                    return entry.getValue();
                }
            } catch (ClassNotFoundException e) {
                // 忽略
            }
        }
        
        return null;
    }
    
    /**
     * 添加异常到事件的映射
     */
    public void addMapping(String exceptionClass,
            Class<? extends AbstractAuthenticationFailureEvent> eventClass) {
        try {
            Constructor<? extends AbstractAuthenticationFailureEvent> constructor 
                    = eventClass.getConstructor(Authentication.class, 
                                                AuthenticationException.class);
            exceptionMappings.put(exceptionClass, constructor);
        } catch (NoSuchMethodException e) {
            throw new RuntimeException("Event class must have a constructor "
                    + "that takes Authentication and AuthenticationException", e);
        }
    }
}
```

#### 22.5 实现审计监听器

**基础审计监听器：**

```java
package com.example.security.audit;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.EventListener;
import org.springframework.security.authentication.event.*;
import org.springframework.security.core.Authentication;
import org.springframework.stereotype.Component;

import java.time.Instant;
import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

/**
 * Spring Security 事件审计监听器
 * 记录所有认证相关的安全事件
 */
@Component
public class SecurityAuditEventListener {
    
    private static final Logger log = LoggerFactory
            .getLogger(SecurityAuditEventListener.class);
    
    /**
     * 内存中的事件存储（生产环境应该使用数据库）
     */
    private final Map<String, SecurityEvent> eventStore = new ConcurrentHashMap<>();
    
    /**
     * 监听认证成功事件
     * 当用户登录成功时触发
     */
    @EventListener
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHENTICATION_SUCCESS")
                .username(authentication.getName())
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(authentication))
                .userAgent(getUserAgent(authentication))
                .authorities(authentication.getAuthorities())
                .details(authentication.getDetails())
                .build();
        
        // 记录到日志
        log.info("✅ 用户登录成功: {}", securityEvent);
        
        // 存储事件
        eventStore.put(securityEvent.getId(), securityEvent);
        
        // 可以发送告警、记录到数据库等
        // auditService.save(securityEvent);
    }
    
    /**
     * 监听认证失败事件 - 凭据错误
     * 当用户名或密码错误时触发
     */
    @EventListener
    public void onBadCredentials(AuthenticationFailureBadCredentialsEvent event) {
        Authentication authentication = event.getAuthentication();
        String username = authentication.getName();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHENTICATION_FAILURE")
                .failureType("BAD_CREDENTIALS")
                .username(username)
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(authentication))
                .errorMessage(event.getException().getMessage())
                .build();
        
        log.warn("❌ 用户登录失败 - 凭据错误: {}", securityEvent);
        
        eventStore.put(securityEvent.getId(), securityEvent);
        
        // 检查是否需要锁定账户（连续失败多次）
        checkAccountLock(username, getClientIp(authentication));
    }
    
    /**
     * 监听认证失败事件 - 账户禁用
     */
    @EventListener
    public void onAccountDisabled(AuthenticationFailureDisabledEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHENTICATION_FAILURE")
                .failureType("ACCOUNT_DISABLED")
                .username(authentication.getName())
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(authentication))
                .errorMessage("账户已被禁用")
                .build();
        
        log.warn("🚫 账户禁用登录尝试: {}", securityEvent);
        
        eventStore.put(securityEvent.getId(), securityEvent);
        
        // 发送告警通知
        sendAlert("账户禁用登录尝试: " + authentication.getName());
    }
    
    /**
     * 监听认证失败事件 - 账户过期
     */
    @EventListener
    public void onAccountExpired(AuthenticationFailureExpiredEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHENTICATION_FAILURE")
                .failureType("ACCOUNT_EXPIRED")
                .username(authentication.getName())
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(authentication))
                .errorMessage("账户已过期")
                .build();
        
        log.warn("⏰ 账户过期登录尝试: {}", securityEvent);
        
        eventStore.put(securityEvent.getId(), securityEvent);
    }
    
    /**
     * 监听认证失败事件 - 账户锁定
     */
    @EventListener
    public void onAccountLocked(AuthenticationFailureLockedEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHENTICATION_FAILURE")
                .failureType("ACCOUNT_LOCKED")
                .username(authentication.getName())
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(authentication))
                .errorMessage("账户已被锁定")
                .build();
        
        log.warn("🔒 账户锁定登录尝试: {}", securityEvent);
        
        eventStore.put(securityEvent.getId(), securityEvent);
        
        // 账户被锁定是严重事件，需要立即告警
        sendAlert("账户锁定登录尝试: " + authentication.getName());
    }
    
    /**
     * 监听用户切换事件
     * 管理员切换到其他用户时触发
     */
    @EventListener
    public void onUserSwitch(AuthenticationSwitchUserEvent event) {
        Authentication principal = event.getAuthentication();
        Authentication targetUser = event.getTargetUser();
        
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("USER_SWITCH")
                .username(principal.getName())  // 切换前的管理员
                .targetUser(targetUser.getName())  // 切换到的目标用户
                .timestamp(LocalDateTime.now())
                .ipAddress(getClientIp(principal))
                .details("管理员切换用户: " + principal.getName() + " -> " + targetUser.getName())
                .build();
        
        log.info("🔄 用户切换: {}", securityEvent);
        
        eventStore.put(securityEvent.getId(), securityEvent);
        
        // 用户切换是敏感操作，需要审计
        auditService.logSensitiveOperation(securityEvent);
    }
    
    /**
     * 获取客户端 IP 地址
     * 这里是简化实现，实际应该从 WebAuthenticationDetails 中提取
     */
    private String getClientIp(Authentication authentication) {
        if (authentication.getDetails() instanceof WebAuthenticationDetails) {
            WebAuthenticationDetails details = 
                    (WebAuthenticationDetails) authentication.getDetails();
            return details.getRemoteAddress();
        }
        return "UNKNOWN";
    }
    
    /**
     * 获取 User-Agent
     */
    private String getUserAgent(Authentication authentication) {
        // 从 authentication.getDetails() 中提取
        return "UNKNOWN";
    }
    
    /**
     * 检查账户锁定
     * 如果同一个 IP/用户连续失败多次，锁定账户
     */
    private void checkAccountLock(String username, String ipAddress) {
        // 统计最近 5 分钟内的失败次数
        long failuresInLast5Minutes = eventStore.values().stream()
                .filter(e -> "AUTHENTICATION_FAILURE".equals(e.getEventType()))
                .filter(e -> "BAD_CREDENTIALS".equals(e.getFailureType()))
                .filter(e -> username.equals(e.getUsername()))
                .filter(e -> e.getTimestamp().isAfter(LocalDateTime.now().minusMinutes(5)))
                .count();
        
        if (failuresInLast5Minutes >= 5) {
            // 锁定账户
            log.warn("⚠️ 账户 {} 在 5 分钟内失败 {} 次，建议锁定", 
                    username, failuresInLast5Minutes);
            sendAlert("账户 " + username + " 可能遭受暴力破解攻击");
        }
    }
    
    /**
     * 发送告警通知
     */
    private void sendAlert(String message) {
        // 这里可以集成邮件、短信、钉钉、企业微信等告警渠道
        log.error("🚨 安全告警: {}", message);
    }
    
    /**
     * 获取所有事件
     */
    public Map<String, SecurityEvent> getAllEvents() {
        return new HashMap<>(eventStore);
    }
}
```

**SecurityEvent 实体类：**

```java
package com.example.security.audit;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.security.core.GrantedAuthority;

import java.time.LocalDateTime;
import java.util.Collection;
import java.util.UUID;

/**
 * 安全事件实体类
 */
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class SecurityEvent {
    
    /**
     * 事件唯一 ID
     */
    @Builder.Default
    private String id = UUID.randomUUID().toString();
    
    /**
     * 事件类型
     * AUTHENTICATION_SUCCESS - 认证成功
     * AUTHENTICATION_FAILURE - 认证失败
     * AUTHORIZATION_FAILURE - 授权失败
     * USER_SWITCH - 用户切换
     * LOGOUT - 登出
     */
    private String eventType;
    
    /**
     * 失败类型（仅失败事件）
     * BAD_CREDENTIALS - 凭据错误
     * ACCOUNT_DISABLED - 账户禁用
     * ACCOUNT_EXPIRED - 账户过期
     * ACCOUNT_LOCKED - 账户锁定
     * CREDENTIALS_EXPIRED - 凭据过期
     */
    private String failureType;
    
    /**
     * 用户名
     */
    private String username;
    
    /**
     * 目标用户（用户切换事件）
     */
    private String targetUser;
    
    /**
     * 事件时间戳
     */
    private LocalDateTime timestamp;
    
    /**
     * 客户端 IP 地址
     */
    private String ipAddress;
    
    /**
     * User-Agent
     */
    private String userAgent;
    
    /**
     * 权限列表
     */
    private Collection<? extends GrantedAuthority> authorities;
    
    /**
     * 认证详情
     */
    private Object details;
    
    /**
     * 错误消息
     */
    private String errorMessage;
    
    /**
     * 会话 ID
     */
    private String sessionId;
    
    /**
     * 请求 URL
     */
    private String requestUrl;
    
    /**
     * 请求方法
     */
    private String requestMethod;
    
    /**
     * 额外信息
     */
    private String details;
}
```

#### 22.6 授权失败监听

授权失败（AccessDenied）不是通过 ApplicationEvent 发布的，而是通过 AccessDeniedHandler 处理。我们需要自定义 AccessDeniedHandler 来监听授权失败事件。

**自定义 AccessDeniedHandler：**

```java
package com.example.security.handler;

import com.example.security.audit.SecurityEvent;
import com.example.security.audit.SecurityAuditEventListener;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.stereotype.Component;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.time.LocalDateTime;

/**
 * 自定义访问拒绝处理器
 * 监听授权失败事件并记录审计日志
 */
@Component
public class AuditingAccessDeniedHandler implements AccessDeniedHandler {
    
    private static final Logger log = LoggerFactory
            .getLogger(AuditingAccessDeniedHandler.class);
    
    private final SecurityAuditEventListener auditEventListener;
    
    public AuditingAccessDeniedHandler(SecurityAuditEventListener auditEventListener) {
        this.auditEventListener = auditEventListener;
    }
    
    @Override
    public void handle(HttpServletRequest request, 
                      HttpServletResponse response,
                      AccessDeniedException accessDeniedException) 
            throws IOException, ServletException {
        
        Authentication authentication = SecurityContextHolder.getContext()
                .getAuthentication();
        
        // 构建授权失败事件
        SecurityEvent securityEvent = SecurityEvent.builder()
                .eventType("AUTHORIZATION_FAILURE")
                .username(authentication != null ? authentication.getName() : "ANONYMOUS")
                .timestamp(LocalDateTime.now())
                .ipAddress(request.getRemoteAddr())
                .userAgent(request.getHeader("User-Agent"))
                .requestUrl(request.getRequestURL().toString())
                .requestMethod(request.getMethod())
                .errorMessage(accessDeniedException.getMessage())
                .details("尝试访问受限资源: " + request.getRequestURI())
                .build();
        
        // 记录审计日志
        log.warn("🚫 授权失败: {}", securityEvent);
        
        // 可以检查是否存在暴力攻击模式
        checkForPotentialAttack(authentication, request);
        
        // 返回 403 响应
        response.setStatus(HttpServletResponse.SC_FORBIDDEN);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write("{\"error\": \"Access Denied\", \"message\": \"" 
                + accessDeniedException.getMessage() + "\"}");
    }
    
    /**
     * 检查潜在的攻击模式
     * 例如：同一用户短时间内多次尝试访问无权限的资源
     */
    private void checkForPotentialAttack(Authentication authentication, 
                                        HttpServletRequest request) {
        if (authentication == null) {
            return;
        }
        
        String username = authentication.getName();
        String ip = request.getRemoteAddr();
        
        // 统计该用户/IP 在最近 1 分钟内的授权失败次数
        long failuresInLastMinute = auditEventListener.getAllEvents()
                .values()
                .stream()
                .filter(e -> "AUTHORIZATION_FAILURE".equals(e.getEventType()))
                .filter(e -> username.equals(e.getUsername()))
                .filter(e -> e.getTimestamp().isAfter(LocalDateTime.now().minusMinutes(1)))
                .count();
        
        if (failuresInLastMinute >= 10) {
            log.error("🚨 检测到潜在攻击: 用户 {} 在 1 分钟内尝试授权失败 {} 次", 
                    username, failuresInLastMinute);
            // 可以考虑暂时封禁该 IP 或账户
        }
    }
}
```

**在安全配置中应用：**

```java
package com.example.security.config;

import com.example.security.handler.AuditingAccessDeniedHandler;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Autowired
    private AuditingAccessDeniedHandler accessDeniedHandler;
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/api/public/**").permitAll()
                .antMatchers("/api/user/**").hasRole("USER")
                .antMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            .and()
            .exceptionHandling()
                .accessDeniedHandler(accessDeniedHandler)  // 使用自定义的 AccessDeniedHandler
            .and()
            .formLogin()
            .and()
            .httpBasic();
    }
}
```

#### 22.7 持久化审计日志

在生产环境中，我们需要将审计日志持久化到数据库。这里使用 Spring Data JPA 实现。

**审计日志实体类：**

```java
package com.example.security.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.*;
import java.time.LocalDateTime;

/**
 * 审计日志实体
 */
@Entity
@Table(name = "security_audit_log", indexes = {
    @Index(name = "idx_username", columnList = "username"),
    @Index(name = "idx_event_type", columnList = "eventType"),
    @Index(name = "idx_timestamp", columnList = "timestamp"),
    @Index(name = "idx_ip_address", columnList = "ipAddress")
})
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class SecurityAuditLog {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, length = 50)
    private String username;
    
    @Column(nullable = false, length = 50)
    private String eventType;
    
    @Column(length = 50)
    private String failureType;
    
    @Column(nullable = false)
    private LocalDateTime timestamp;
    
    @Column(length = 50)
    private String ipAddress;
    
    @Column(length = 500)
    private String userAgent;
    
    @Column(length = 1000)
    private String requestUrl;
    
    @Column(length = 20)
    private String requestMethod;
    
    @Column(length = 1000)
    private String errorMessage;
    
    @Column(length = 2000)
    private String details;
    
    @Column(length = 100)
    private String sessionId;
}
```

**Repository 接口：**

```java
package com.example.security.repository;

import com.example.security.entity.SecurityAuditLog;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.util.List;

/**
 * 审计日志 Repository
 */
@Repository
public interface SecurityAuditLogRepository extends JpaRepository<SecurityAuditLog, Long> {
    
    /**
     * 根据用户名查询审计日志
     */
    List<SecurityAuditLog> findByUsername(String username);
    
    /**
     * 根据事件类型查询审计日志
     */
    List<SecurityAuditLog> findByEventType(String eventType);
    
    /**
     * 根据时间范围查询审计日志
     */
    List<SecurityAuditLog> findByTimestampBetween(LocalDateTime start, 
                                                   LocalDateTime end);
    
    /**
     * 查询最近的审计日志
     */
    List<SecurityAuditLog> findTop50ByOrderByTimestampDesc();
    
    /**
     * 查询指定用户的失败登录尝试
     */
    @Query("SELECT e FROM SecurityAuditLog e WHERE e.username = :username " +
           "AND e.eventType = 'AUTHENTICATION_FAILURE' " +
           "AND e.timestamp >= :since ORDER BY e.timestamp DESC")
    List<SecurityAuditLog> findFailedLoginAttempts(
            @Param("username") String username, 
            @Param("since") LocalDateTime since);
    
    /**
     * 统计指定时间范围内的失败登录次数
     */
    @Query("SELECT COUNT(e) FROM SecurityAuditLog e WHERE e.eventType = " +
           "'AUTHENTICATION_FAILURE' AND e.timestamp >= :since")
    long countFailedLoginsSince(@Param("since") LocalDateTime since);
}
```

**审计服务类：**

```java
package com.example.security.service;

import com.example.security.entity.SecurityAuditLog;
import com.example.security.repository.SecurityAuditLogRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDateTime;
import java.util.List;

/**
 * 审计日志服务
 */
@Service
public class AuditService {
    
    private static final Logger log = LoggerFactory
            .getLogger(AuditService.class);
    
    @Autowired
    private SecurityAuditLogRepository auditLogRepository;
    
    /**
     * 异步保存审计日志
     * 使用 @Async 避免阻塞主线程
     */
    @Async
    @Transactional
    public void saveAuditLog(SecurityAuditLog auditLog) {
        try {
            auditLogRepository.save(auditLog);
            log.debug("审计日志已保存: {}", auditLog);
        } catch (Exception e) {
            log.error("保存审计日志失败", e);
            // 可以考虑降级存储到本地文件或其他存储
        }
    }
    
    /**
     * 查询用户的审计历史
     */
    @Transactional(readOnly = true)
    public List<SecurityAuditLog> getUserAuditHistory(String username) {
        return auditLogRepository.findByUsername(username);
    }
    
    /**
     * 查询最近的审计日志
     */
    @Transactional(readOnly = true)
    public List<SecurityAuditLog> getRecentAuditLogs(int limit) {
        return auditLogRepository.findTop50ByOrderByTimestampDesc();
    }
    
    /**
     * 检查账户是否应该被锁定
     * 如果 5 分钟内失败次数 >= 5，返回 true
     */
    @Transactional(readOnly = true)
    public boolean shouldLockAccount(String username) {
        LocalDateTime since = LocalDateTime.now().minusMinutes(5);
        List<SecurityAuditLog> failedAttempts = 
                auditLogRepository.findFailedLoginAttempts(username, since);
        return failedAttempts.size() >= 5;
    }
    
    /**
     * 获取安全统计信息
     */
    @Transactional(readOnly = true)
    public SecurityStatistics getSecurityStatistics(LocalDateTime since) {
        long totalAttempts = auditLogRepository.count();
        long failedAttempts = auditLogRepository.countFailedLoginsSince(since);
        long successAttempts = totalAttempts - failedAttempts;
        
        return SecurityStatistics.builder()
                .totalAttempts(totalAttempts)
                .successAttempts(successAttempts)
                .failedAttempts(failedAttempts)
                .failureRate(totalAttempts > 0 ? (double) failedAttempts / totalAttempts : 0)
                .build();
    }
}
```

**更新后的审计监听器（支持持久化）：**

```java
package com.example.security.audit;

import com.example.security.entity.SecurityAuditLog;
import com.example.security.service.AuditService;
import org.springframework.context.event.EventListener;
import org.springframework.scheduling.annotation.Async;
import org.springframework.security.authentication.event.AuthenticationFailureBadCredentialsEvent;
import org.springframework.security.authentication.event.AuthenticationSuccessEvent;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.WebAuthenticationDetails;
import org.springframework.stereotype.Component;

/**
 * 支持持久化的审计监听器
 */
@Component
public class PersistentSecurityAuditListener {
    
    private final AuditService auditService;
    
    public PersistentSecurityAuditListener(AuditService auditService) {
        this.auditService = auditService;
    }
    
    /**
     * 监听认证成功事件
     */
    @EventListener
    @Async  // 异步处理，避免阻塞认证流程
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityAuditLog auditLog = SecurityAuditLog.builder()
                .username(authentication.getName())
                .eventType("AUTHENTICATION_SUCCESS")
                .timestamp(java.time.LocalDateTime.now())
                .ipAddress(extractIp(authentication))
                .userAgent(extractUserAgent(authentication))
                .build();
        
        auditService.saveAuditLog(auditLog);
    }
    
    /**
     * 监听认证失败事件
     */
    @EventListener
    @Async
    public void onAuthenticationFailure(AuthenticationFailureBadCredentialsEvent event) {
        Authentication authentication = event.getAuthentication();
        
        SecurityAuditLog auditLog = SecurityAuditLog.builder()
                .username(authentication.getName())
                .eventType("AUTHENTICATION_FAILURE")
                .failureType("BAD_CREDENTIALS")
                .timestamp(java.time.LocalDateTime.now())
                .ipAddress(extractIp(authentication))
                .errorMessage(event.getException().getMessage())
                .build();
        
        auditService.saveAuditLog(auditLog);
    }
    
    /**
     * 从 Authentication 中提取 IP 地址
     */
    private String extractIp(Authentication authentication) {
        if (authentication.getDetails() instanceof WebAuthenticationDetails) {
            WebAuthenticationDetails details = 
                    (WebAuthenticationDetails) authentication.getDetails();
            return details.getRemoteAddress();
        }
        return "UNKNOWN";
    }
    
    /**
     * 从 Authentication 中提取 User-Agent
     */
    private String extractUserAgent(Authentication authentication) {
        // 实际实现需要从 authentication.getDetails() 中提取
        return "UNKNOWN";
    }
}
```

#### 22.8 实时监控与告警

**使用 Spring WebSocket 实现实时监控：**

```java
package com.example.security.monitoring;

import com.example.security.entity.SecurityAuditLog;
import com.example.security.service.AuditService;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;
import java.util.List;

/**
 * 实时安全监控
 */
@Component
public class SecurityMonitoringService {
    
    private final AuditService auditService;
    private final SimpMessagingTemplate messagingTemplate;
    
    private LocalDateTime lastCheckTime = LocalDateTime.now();
    
    public SecurityMonitoringService(AuditService auditService,
                                    SimpessagingTemplate messagingTemplate) {
        this.auditService = auditService;
        this.messagingTemplate = messagingTemplate;
    }
    
    /**
     * 每 10 秒检查一次新的安全事件
     * 并推送到前端监控面板
     */
    @Scheduled(fixedDelay = 10000)
    public void checkSecurityEvents() {
        LocalDateTime now = LocalDateTime.now();
        
        // 查询从上次检查到现在的新事件
        List<SecurityAuditLog> newEvents = 
                auditService.findAuditLogsBetween(lastCheckTime, now);
        
        if (!newEvents.isEmpty()) {
            // 推送到 WebSocket 客户端
            messagingTemplate.convertAndSend("/topic/security-events", newEvents);
            
            // 检查是否有严重事件需要告警
            for (SecurityAuditLog event : newEvents) {
                if (isCriticalEvent(event)) {
                    sendAlert(event);
                }
            }
        }
        
        lastCheckTime = now;
    }
    
    /**
     * 判断是否为严重事件
     */
    private boolean isCriticalEvent(SecurityAuditLog event) {
        return "AUTHENTICATION_FAILURE".equals(event.getEventType()) &&
               ("ACCOUNT_DISABLED".equals(event.getFailureType()) ||
                "ACCOUNT_LOCKED".equals(event.getFailureType()));
    }
    
    /**
     * 发送告警通知
     */
    private void sendAlert(SecurityAuditLog event) {
        // 可以集成邮件、短信、钉钉、企业微信等
        String message = String.format(
                "🚨 安全告警: 用户 %s 触发 %s 事件",
                event.getUsername(),
                event.getFailureType()
        );
        
        // 推送到告警主题
        messagingTemplate.convertAndSend("/topic/security-alerts", message);
    }
}
```

**WebSocket 配置：**

```java
package com.example.security.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.*;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        // 启用简单的消息代理，用于向客户端推送消息
        config.enableSimpleBroker("/topic");
        config.setApplicationDestinationPrefixes("/app");
    }
    
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        // 注册 STOMP 端点
        registry.addEndpoint("/ws-security")
                .setAllowedOriginPatterns("*")
                .withSockJS();
    }
}
```

#### 22.9 集成第三方监控系统

**集成 Prometheus：**

```java
package com.example.security.monitoring;

import io.micrometer.core.instrument.Counter;
import io.micrometer.core.instrument.MeterRegistry;
import io.micrometer.core.instrument.Timer;
import org.springframework.security.authentication.event.AbstractAuthenticationFailureEvent;
import org.springframework.security.authentication.event.AuthenticationSuccessEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

/**
 * 集成 Prometheus 监控
 */
@Component
public class PrometheusSecurityMetrics {
    
    private final Counter authenticationSuccessCounter;
    private final Counter authenticationFailureCounter;
    private final Counter badCredentialsCounter;
    private final Counter accountDisabledCounter;
    private final Counter accountLockedCounter;
    private final Counter accessDeniedCounter;
    private final Timer authenticationTimer;
    
    public PrometheusSecurityMetrics(MeterRegistry registry) {
        // 定义计数器
        authenticationSuccessCounter = Counter.builder("security.authentication.success")
                .description("认证成功次数")
                .register(registry);
        
        authenticationFailureCounter = Counter.builder("security.authentication.failure")
                .description("认证失败次数")
                .register(registry);
        
        badCredentialsCounter = Counter.builder("security.authentication.failure.bad_credentials")
                .description("凭据错误次数")
                .register(registry);
        
        accountDisabledCounter = Counter.builder("security.authentication.failure.account_disabled")
                .description("账户禁用次数")
                .register(registry);
        
        accountLockedCounter = Counter.builder("security.authentication.failure.account_locked")
                .description("账户锁定次数")
                .register(registry);
        
        accessDeniedCounter = Counter.builder("security.authorization.denied")
                .description("授权拒绝次数")
                .register(registry);
        
        // 定义计时器
        authenticationTimer = Timer.builder("security.authentication.duration")
                .description("认证耗时")
                .register(registry);
    }
    
    /**
     * 监听认证成功事件
     */
    @EventListener
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        authenticationSuccessCounter.increment();
    }
    
    /**
     * 监听认证失败事件
     */
    @EventListener
    public void onAuthenticationFailure(AbstractAuthenticationFailureEvent event) {
        authenticationFailureCounter.increment();
        
        // 根据失败类型分类计数
        String exceptionType = event.getException().getClass().getSimpleName();
        switch (exceptionType) {
            case "BadCredentialsException":
                badCredentialsCounter.increment();
                break;
            case "DisabledException":
                accountDisabledCounter.increment();
                break;
            case "LockedException":
                accountLockedCounter.increment();
                break;
            default:
                break;
        }
    }
}
```

#### 22.10 总结与FAQ

**本节总结：**

1. **事件体系**：
   - AuthenticationSuccessEvent：认证成功
   - AbstractAuthenticationFailureEvent：认证失败（有多种子类型）
   - AuthenticationSwitchUserEvent：用户切换

2. **审计实现**：
   - 使用 `@EventListener` 监听安全事件
   - 自定义 `AccessDeniedHandler` 监听授权失败
   - 持久化审计日志到数据库
   - 实时监控与告警

3. **监控集成**：
   - WebSocket 实时推送
   - Prometheus 指标收集
   - 自定义告警规则

**FAQ：**

**Q1: Spring Security 事件是同步还是异步的？**

A: 默认是同步的，事件监听器会阻塞认证流程。建议使用 `@Async` 注解将审计处理异步化，避免影响用户体验。

**Q2: 如何防止审计日志被篡改？**

A: 
- 使用数据库约束（如只读权限）
- 定期备份数据库
- 使用区块链或不可变日志存储
- 对敏感日志进行数字签名

**Q3: 审计日志应该保留多久？**

A: 根据合规要求和业务需求：
- 一般审计日志：6-12 个月
- 敏感操作日志：永久保留
- 可以使用归档策略，将旧日志迁移到低成本存储

**Q4: 如何处理审计日志的隐私问题？**

A: 
- 不记录敏感信息（密码、令牌）
- 对敏感字段进行脱敏或加密
- 遵守 GDPR 等隐私法规
- 提供日志查询审计（谁查询了日志）

**Q5: 如何检测暴力破解攻击？**

A: 
- 统计同一 IP/用户的失败登录次数
- 使用滑动窗口算法（如 5 分钟内失败 5 次）
- 结合速率限制（Rate Limiting）
- 使用 CAPTCHA 防止自动化攻击

**思考题：**

1. 如何设计一个分布式的审计日志系统？
2. 如何使用机器学习检测异常登录行为？
3. 如何实现审计日志的实时分析（如使用 ELK Stack）？
4. 如何设计审计日志的查询 API？
5. 如何满足不同行业的合规性要求（如金融、医疗）？

---

## 模块8：综合实战项目

### 知识点23：企业级安全系统实战

#### 23.1 项目概述

本模块将构建一个完整的企业级安全系统，涵盖前面学习的所有知识点。项目采用前后端分离架构，包含以下核心模块：

1. **用户管理模块**：用户 CRUD、密码管理
2. **权限管理模块**：角色、权限、资源管理
3. **JWT 认证模块**：无状态认证
4. **OAuth2 第三方登录**：社交登录
5. **安全审计模块**：操作日志、审计报告
6. **前后端分离集成**：CORS、CSRF 处理

#### 23.2 项目架构设计

**技术栈：**

```
后端：
- Spring Boot 2.4.5
- Spring Security 5.3.9.RELEASE
- Spring Data JPA
- JWT (jjwt 0.9.1)
- MySQL 8.0
- Redis 6.0（缓存、会话存储）
- Maven

前端：
- Vue 3
- Vue Router
- Axios
- Element Plus

开发工具：
- Git
- Postman
- Docker
```

**项目结构：**

```
enterprise-security-system/
├── enterprise-security-backend/        # 后端项目
│   ├── src/main/java/com/example/enterprise/
│   │   ├── config/                    # 配置类
│   │   ├── controller/                # REST API
│   │   ├── service/                   # 业务逻辑
│   │   ├── repository/                # 数据访问
│   │   ├── entity/                    # 实体类
│   │   ├── dto/                       # 数据传输对象
│   │   ├── security/                  # 安全相关
│   │   ├── exception/                 # 异常处理
│   │   └── util/                      # 工具类
│   └── pom.xml
├── enterprise-security-frontend/       # 前端项目
│   ├── src/
│   │   ├── api/                       # API 调用
│   │   ├── components/                # 组件
│   │   ├── views/                     # 页面
│   │   ├── router/                    # 路由
│   │   ├── store/                     # Vuex
│   │   └── utils/                     # 工具
│   └── package.json
└── docker-compose.yml                 # Docker 编排
```

#### 23.3 用户管理模块

**用户实体类：**

```java
package com.example.enterprise.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.*;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Size;
import java.time.LocalDateTime;
import java.util.HashSet;
import java.util.Set;

/**
 * 用户实体类
 */
@Entity
@Table(name = "sys_user", indexes = {
    @Index(name = "idx_username", columnList = "username", unique = true),
    @Index(name = "idx_email", columnList = "email", unique = true),
    @Index(name = "idx_phone", columnList = "phone")
})
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    /**
     * 用户名（唯一）
     */
    @NotBlank(message = "用户名不能为空")
    @Size(min = 3, max = 50, message = "用户名长度必须在3-50之间")
    @Column(unique = true, nullable = false, length = 50)
    private String username;
    
    /**
     * 密码（加密存储）
     */
    @NotBlank(message = "密码不能为空")
    @Column(nullable = false)
    private String password;
    
    /**
     * 电子邮箱（唯一）
     */
    @Email(message = "邮箱格式不正确")
    @Column(unique = true, length = 100)
    private String email;
    
    /**
     * 手机号
     */
    @Column(length = 20)
    private String phone;
    
    /**
     * 真实姓名
     */
    @Column(length = 50)
    private String realName;
    
    /**
     * 头像 URL
     */
    @Column(length = 500)
    private String avatar;
    
    /**
     * 账户状态
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean enabled = true;
    
    /**
     * 账户是否过期
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean accountNonExpired = true;
    
    /**
     * 凭据是否过期
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean credentialsNonExpired = true;
    
    /**
     * 账户是否锁定
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean accountNonLocked = true;
    
    /**
     * 用户角色（多对多关系）
     */
    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(
        name = "sys_user_role",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    @Builder.Default
    private Set<Role> roles = new HashSet<>();
    
    /**
     * 最后登录时间
     */
    @Column
    private LocalDateTime lastLoginTime;
    
    /**
     * 最后登录 IP
     */
    @Column(length = 50)
    private String lastLoginIp;
    
    /**
     * 登录失败次数
     */
    @Column
    @Builder.Default
    private Integer loginFailCount = 0;
    
    /**
     * 账户锁定时间
     */
    @Column
    private LocalDateTime lockTime;
    
    /**
     * 创建时间
     */
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    private LocalDateTime createTime;
    
    /**
     * 更新时间
     */
    @UpdateTimestamp
    @Column(nullable = false)
    private LocalDateTime updateTime;
    
    /**
     * 备注
     */
    @Column(length = 500)
    private String remark;
}
```

**角色实体类：**

```java
package com.example.enterprise.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.HashSet;
import java.util.Set;

/**
 * 角色实体类
 */
@Entity
@Table(name = "sys_role", indexes = {
    @Index(name = "idx_role_code", columnList = "code", unique = true)
})
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Role {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    /**
     * 角色名称
     */
    @Column(nullable = false, length = 50)
    private String name;
    
    /**
     * 角色代码（唯一，如 ROLE_ADMIN）
     */
    @Column(unique = true, nullable = false, length = 50)
    private String code;
    
    /**
     * 角色描述
     */
    @Column(length = 200)
    private String description;
    
    /**
     * 角色状态
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean enabled = true;
    
    /**
     * 排序号
     */
    @Column
    @Builder.Default
    private Integer sortOrder = 0;
    
    /**
     * 角色权限（多对多关系）
     */
    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(
        name = "sys_role_permission",
        joinColumns = @JoinColumn(name = "role_id"),
        inverseJoinColumns = @JoinColumn(name = "permission_id")
    )
    @Builder.Default
    private Set<Permission> permissions = new HashSet<>();
    
    /**
     * 创建时间
     */
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    private LocalDateTime createTime;
}
```

**权限实体类：**

```java
package com.example.enterprise.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;

import javax.persistence.*;
import java.time.LocalDateTime;

/**
 * 权限实体类
 */
@Entity
@Table(name = "sys_permission", indexes = {
    @Index(name = "idx_permission_code", columnList = "code", unique = true),
    @Index(name = "idx_resource_type", columnList = "resourceType")
})
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Permission {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    /**
     * 权限名称
     */
    @Column(nullable = false, length = 50)
    private String name;
    
    /**
     * 权限代码（唯一，如 user:create）
     */
    @Column(unique = true, nullable = false, length = 100)
    private String code;
    
    /**
     * 资源类型（MENU、BUTTON、API）
     */
    @Column(nullable = false, length = 20)
    private String resourceType;
    
    /**
     * 资源路径（URL 或按钮标识）
     */
    @Column(length = 200)
    private String resourcePath;
    
    /**
     * 请求方法（GET、POST、PUT、DELETE、*）
     */
    @Column(length = 20)
    private String method;
    
    /**
     * 父权限 ID
     */
    @Column
    private Long parentId;
    
    /**
     * 权限描述
     */
    @Column(length = 200)
    private String description;
    
    /**
     * 排序号
     */
    @Column
    @Builder.Default
    private Integer sortOrder = 0;
    
    /**
     * 权限状态
     */
    @Column(nullable = false)
    @Builder.Default
    private Boolean enabled = true;
    
    /**
     * 创建时间
     */
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    private LocalDateTime createTime;
}
```

**UserDetails 实现类：**

```java
package com.example.enterprise.security;

import com.example.enterprise.entity.User;
import lombok.Getter;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.Set;
import java.util.stream.Collectors;

/**
 * 自定义 UserDetails 实现
 * 将 User 实体适配到 Spring Security 的 UserDetails 接口
 */
@Getter
public class CustomUserDetails implements UserDetails {
    
    private final User user;
    private final Set<? extends GrantedAuthority> authorities;
    
    public CustomUserDetails(User user) {
        this.user = user;
        // 从用户的角色中提取权限
        this.authorities = user.getRoles().stream()
                .flatMap(role -> role.getPermissions().stream())
                .map(permission -> new SimpleGrantedAuthority(permission.getCode()))
                .collect(Collectors.toSet());
    }
    
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }
    
    @Override
    public String getPassword() {
        return user.getPassword();
    }
    
    @Override
    public String getUsername() {
        return user.getUsername();
    }
    
    @Override
    public boolean isAccountNonExpired() {
        return user.getAccountNonExpired();
    }
    
    @Override
    public boolean isAccountNonLocked() {
        return user.getAccountNonLocked();
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        return user.getCredentialsNonExpired();
    }
    
    @Override
    public boolean isEnabled() {
        return user.getEnabled();
    }
    
    /**
     * 获取用户 ID
     */
    public Long getUserId() {
        return user.getId();
    }
}
```

**UserDetailsService 实现：**

```java
package com.example.enterprise.security;

import com.example.enterprise.entity.User;
import com.example.enterprise.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDateTime;

/**
 * 自定义 UserDetailsService 实现
 * 从数据库加载用户信息
 */
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {
    
    private final UserRepository userRepository;
    
    /**
     * 根据用户名加载用户
     * 同时更新用户的登录信息
     */
    @Override
    @Transactional(readOnly = true)
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 1. 从数据库查询用户
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException(
                        "用户不存在: " + username));
        
        // 2. 检查账户是否被锁定
        if (user.getLockTime() != null && 
            user.getLockTime().isAfter(LocalDateTime.now())) {
            throw new UsernameNotFoundException("账户已被锁定");
        }
        
        // 3. 创建 CustomUserDetails
        return new CustomUserDetails(user);
    }
    
    /**
     * 更新最后登录信息
     */
    @Transactional
    public void updateLastLogin(Long userId, String ip) {
        userRepository.findById(userId).ifPresent(user -> {
            user.setLastLoginTime(LocalDateTime.now());
            user.setLastLoginIp(ip);
            user.setLoginFailCount(0);  // 重置失败次数
            userRepository.save(user);
        });
    }
    
    /**
     * 增加登录失败次数
     * 如果失败次数达到阈值，锁定账户
     */
    @Transactional
    public void increaseLoginFailCount(Long userId) {
        userRepository.findById(userId).ifPresent(user -> {
            int failCount = user.getLoginFailCount() + 1;
            user.setLoginFailCount(failCount);
            
            // 失败 5 次，锁定 30 分钟
            if (failCount >= 5) {
                user.setAccountNonLocked(false);
                user.setLockTime(LocalDateTime.now().plusMinutes(30));
            }
            
            userRepository.save(user);
        });
    }
}
```

**Repository 接口：**

```java
package com.example.enterprise.repository;

import com.example.enterprise.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.JpaSpecificationExecutor;
import org.springframework.stereotype.Repository;

import java.util.Optional;

/**
 * 用户 Repository
 */
@Repository
public interface UserRepository extends JpaRepository<User, Long>, 
                                      JpaSpecificationExecutor<User> {
    
    /**
     * 根据用户名查询用户
     */
    Optional<User> findByUsername(String username);
    
    /**
     * 根据邮箱查询用户
     */
    Optional<User> findByEmail(String email);
    
    /**
     * 根据手机号查询用户
     */
    Optional<User> findByPhone(String phone);
    
    /**
     * 检查用户名是否存在
     */
    boolean existsByUsername(String username);
    
    /**
     * 检查邮箱是否存在
     */
    boolean existsByEmail(String email);
}
```

**用户服务类：**

```java
package com.example.enterprise.service;

import com.example.enterprise.dto.UserDTO;
import com.example.enterprise.entity.User;
import com.example.enterprise.exception.BusinessException;
import com.example.enterprise.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * 用户服务类
 */
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    
    /**
     * 创建用户
     */
    @Transactional
    public User createUser(UserDTO userDTO) {
        // 1. 检查用户名是否已存在
        if (userRepository.existsByUsername(userDTO.getUsername())) {
            throw new BusinessException("用户名已存在");
        }
        
        // 2. 检查邮箱是否已存在
        if (userDTO.getEmail() != null && 
            userRepository.existsByEmail(userDTO.getEmail())) {
            throw new BusinessException("邮箱已存在");
        }
        
        // 3. 加密密码
        String encodedPassword = passwordEncoder.encode(userDTO.getPassword());
        
        // 4. 创建用户
        User user = User.builder()
                .username(userDTO.getUsername())
                .password(encodedPassword)
                .email(userDTO.getEmail())
                .phone(userDTO.getPhone())
                .realName(userDTO.getRealName())
                .enabled(true)
                .accountNonExpired(true)
                .credentialsNonExpired(true)
                .accountNonLocked(true)
                .build();
        
        return userRepository.save(user);
    }
    
    /**
     * 更新用户
     */
    @Transactional
    public User updateUser(Long id, UserDTO userDTO) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        // 更新用户信息
        if (userDTO.getEmail() != null) {
            user.setEmail(userDTO.getEmail());
        }
        if (userDTO.getPhone() != null) {
            user.setPhone(userDTO.getPhone());
        }
        if (userDTO.getRealName() != null) {
            user.setRealName(userDTO.getRealName());
        }
        if (userDTO.getAvatar() != null) {
            user.setAvatar(userDTO.getAvatar());
        }
        
        return userRepository.save(user);
    }
    
    /**
     * 修改密码
     */
    @Transactional
    public void changePassword(Long userId, String oldPassword, String newPassword) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        // 验证旧密码
        if (!passwordEncoder.matches(oldPassword, user.getPassword())) {
            throw new BusinessException("旧密码不正确");
        }
        
        // 加密新密码
        String encodedPassword = passwordEncoder.encode(newPassword);
        user.setPassword(encodedPassword);
        
        userRepository.save(user);
    }
    
    /**
     * 重置密码
     */
    @Transactional
    public void resetPassword(Long userId, String newPassword) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        String encodedPassword = passwordEncoder.encode(newPassword);
        user.setPassword(encodedPassword);
        
        userRepository.save(user);
    }
    
    /**
     * 禁用用户
     */
    @Transactional
    public void disableUser(Long userId) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        user.setEnabled(false);
        userRepository.save(user);
    }
    
    /**
     * 启用用户
     */
    @Transactional
    public void enableUser(Long userId) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        user.setEnabled(true);
        userRepository.save(user);
    }
    
    /**
     * 锁定用户
     */
    @Transactional
    public void lockUser(Long userId, int lockMinutes) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        user.setAccountNonLocked(false);
        user.setLockTime(java.time.LocalDateTime.now().plusMinutes(lockMinutes));
        
        userRepository.save(user);
    }
    
    /**
     * 解锁用户
     */
    @Transactional
    public void unlockUser(Long userId) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        user.setAccountNonLocked(true);
        user.setLockTime(null);
        user.setLoginFailCount(0);
        
        userRepository.save(user);
    }
    
    /**
     * 分页查询用户
     */
    @Transactional(readOnly = true)
    public Page<User> getUsers(String keyword, Pageable pageable) {
        if (keyword == null || keyword.trim().isEmpty()) {
            return userRepository.findAll(pageable);
        }
        
        // 使用 Specification 动态查询
        return userRepository.findAll((root, query, cb) -> {
            String pattern = "%" + keyword + "%";
            return cb.or(
                    cb.like(root.get("username"), pattern),
                    cb.like(root.get("email"), pattern),
                    cb.like(root.get("realName"), pattern)
            );
        }, pageable);
    }
    
    /**
     * 根据 ID 获取用户
     */
    @Transactional(readOnly = true)
    public User getUserById(Long id) {
        return userRepository.findById(id)
                .orElseThrow(() -> new BusinessException("用户不存在"));
    }
    
    /**
     * 删除用户
     */
    @Transactional
    public void deleteUser(Long id) {
        if (!userRepository.existsById(id)) {
            throw new BusinessException("用户不存在");
        }
        userRepository.deleteById(id);
    }
}
```

#### 23.4 JWT 认证模块

**JWT 工具类：**

```java
package com.example.enterprise.util;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.stereotype.Component;

import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.util.Date;
import java.util.stream.Collectors;

/**
 * JWT 工具类
 * 负责生成、解析、验证 JWT Token
 */
@Slf4j
@Component
public class JwtTokenUtil {
    
    @Value("${jwt.secret:your-256-bit-secret-key-that-must-be-at-least-32-characters-long}")
    private String secret;
    
    @Value("${jwt.expiration:86400000}")  // 默认 24 小时
    private Long expiration;
    
    @Value("${jwt.refresh-expiration:604800000}")  // 默认 7 天
    private Long refreshExpiration;
    
    /**
     * 生成访问 Token
     * 
     * @param authentication 认证信息
     * @return JWT Token
     */
    public String generateAccessToken(Authentication authentication) {
        return generateToken(authentication, expiration);
    }
    
    /**
     * 生成刷新 Token
     * 
     * @param authentication 认证信息
     * @return JWT Token
     */
    public String generateRefreshToken(Authentication authentication) {
        return generateToken(authentication, refreshExpiration);
    }
    
    /**
     * 生成 Token
     * 
     * @param authentication 认证信息
     * @param expiration     过期时间（毫秒）
     * @return JWT Token
     */
    private String generateToken(Authentication authentication, Long expiration) {
        // 1. 获取用户名
        String username = authentication.getName();
        
        // 2. 获取权限列表
        String authorities = authentication.getAuthorities().stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.joining(","));
        
        // 3. 获取当前时间
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + expiration);
        
        // 4. 构建 Token
        return Jwts.builder()
                .setSubject(username)                    // 主题（用户名）
                .claim("authorities", authorities)       // 自定义声明：权限
                .claim("type", "access")                 // Token 类型
                .setIssuedAt(now)                        // 签发时间
                .setExpiration(expiryDate)               // 过期时间
                .signWith(getSigningKey(), SignatureAlgorithm.HS512)  // 签名
                .compact();
    }
    
    /**
     * 从 Token 中获取用户名
     */
    public String getUsernameFromToken(String token) {
        Claims claims = getClaimsFromToken(token);
        return claims.getSubject();
    }
    
    /**
     * 从 Token 中获取权限
     */
    public String getAuthoritiesFromToken(String token) {
        Claims claims = getClaimsFromToken(token);
        return claims.get("authorities", String.class);
    }
    
    /**
     * 验证 Token 是否有效
     */
    public boolean validateToken(String token) {
        try {
            Jwts.parserBuilder()
                    .setSigningKey(getSigningKey())
                    .build()
                    .parseClaimsJws(token);
            return true;
        } catch (SecurityException ex) {
            log.error("Invalid JWT signature: {}", ex.getMessage());
        } catch (MalformedJwtException ex) {
            log.error("Invalid JWT token: {}", ex.getMessage());
        } catch (ExpiredJwtException ex) {
            log.error("Expired JWT token: {}", ex.getMessage());
        } catch (UnsupportedJwtException ex) {
            log.error("Unsupported JWT token: {}", ex.getMessage());
        } catch (IllegalArgumentException ex) {
            log.error("JWT claims string is empty: {}", ex.getMessage());
        }
        return false;
    }
    
    /**
     * 获取 Token 的过期时间
     */
    public Date getExpirationDateFromToken(String token) {
        Claims claims = getClaimsFromToken(token);
        return claims.getExpiration();
    }
    
    /**
     * 判断 Token 是否过期
     */
    public boolean isTokenExpired(String token) {
        Date expiration = getExpirationDateFromToken(token);
        return expiration.before(new Date());
    }
    
    /**
     * 从 Token 中获取 Claims
     */
    private Claims getClaimsFromToken(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
    
    /**
     * 获取签名密钥
     */
    private SecretKey getSigningKey() {
        byte[] keyBytes = secret.getBytes(StandardCharsets.UTF_8);
        return Keys.hmacShaKeyFor(keyBytes);
    }
}
```

**JWT 认证过滤器：**

```java
package com.example.enterprise.security;

import com.example.enterprise.util.JwtTokenUtil;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.stream.Collectors;

/**
 * JWT 认证过滤器
 * 从请求头中提取 JWT Token 并验证
 */
@Slf4j
@Component
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    private final JwtTokenUtil jwtTokenUtil;
    
    private static final String AUTHORIZATION_HEADER = "Authorization";
    private static final String BEARER_PREFIX = "Bearer ";
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        try {
            // 1. 从请求头中提取 Token
            String jwt = extractJwtFromRequest(request);
            
            // 2. 验证 Token 并设置认证信息
            if (StringUtils.hasText(jwt) && jwtTokenUtil.validateToken(jwt)) {
                // 2.1 从 Token 中提取用户名
                String username = jwtTokenUtil.getUsernameFromToken(jwt);
                
                // 2.2 从 Token 中提取权限
                String authorities = jwtTokenUtil.getAuthoritiesFromToken(jwt);
                
                // 2.3 创建认证对象
                UsernamePasswordAuthenticationToken authentication = 
                        new UsernamePasswordAuthenticationToken(
                                username,
                                null,
                                authorities.split(",").stream()
                                        .map(SimpleGrantedAuthority::new)
                                        .collect(Collectors.toList())
                        );
                
                // 2.4 设置认证详情（包含 IP、SessionId 等）
                authentication.setDetails(
                        new WebAuthenticationDetailsSource().buildDetails(request)
                );
                
                // 2.5 将认证对象设置到 SecurityContext
                SecurityContextHolder.getContext().setAuthentication(authentication);
                
                log.debug("Set Authentication for user: {}", username);
            }
        } catch (Exception ex) {
            log.error("Could not set user authentication in security context", ex);
        }
        
        // 3. 继续过滤器链
        filterChain.doFilter(request, response);
    }
    
    /**
     * 从请求头中提取 JWT Token
     */
    private String extractJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader(AUTHORIZATION_HEADER);
        
        if (StringUtils.hasText(bearerToken) && 
            bearerToken.startsWith(BEARER_PREFIX)) {
            return bearerToken.substring(BEARER_PREFIX.length());
        }
        
        return null;
    }
}
```

**JWT 认证入口点：**

```java
package com.example.enterprise.security;

import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.MediaType;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

/**
 * JWT 认证入口点
 * 当用户未认证时返回 401 响应
 */
@Slf4j
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    
    @Override
    public void commence(HttpServletRequest request,
                        HttpServletResponse response,
                        AuthenticationException authException)
            throws IOException {
        
        log.error("Unauthorized error: {}", authException.getMessage());
        
        // 设置响应头
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setCharacterEncoding("UTF-8");
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        
        // 构建响应体
        Map<String, Object> body = new HashMap<>();
        body.put("timestamp", System.currentTimeMillis());
        body.put("status", HttpServletResponse.SC_UNAUTHORIZED);
        body.put("error", "Unauthorized");
        body.put("message", "认证失败，请重新登录");
        body.put("path", request.getServletPath());
        
        // 写入响应
        new ObjectMapper().writeValue(response.getOutputStream(), body);
    }
}
```

**JWT 访问拒绝处理器：**

```java
package com.example.enterprise.security;

import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.MediaType;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

/**
 * JWT 访问拒绝处理器
 * 当用户权限不足时返回 403 响应
 */
@Slf4j
@Component
public class JwtAccessDeniedHandler implements AccessDeniedHandler {
    
    @Override
    public void handle(HttpServletRequest request,
                      HttpServletResponse response,
                      AccessDeniedException accessDeniedException)
            throws IOException {
        
        log.error("Access denied: {}", accessDeniedException.getMessage());
        
        // 设置响应头
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setCharacterEncoding("UTF-8");
        response.setStatus(HttpServletResponse.SC_FORBIDDEN);
        
        // 构建响应体
        Map<String, Object> body = new HashMap<>();
        body.put("timestamp", System.currentTimeMillis());
        body.put("status", HttpServletResponse.SC_FORBIDDEN);
        body.put("error", "Forbidden");
        body.put("message", "权限不足，无法访问该资源");
        body.put("path", request.getServletPath());
        
        // 写入响应
        new ObjectMapper().writeValue(response.getOutputStream(), body);
    }
}
```

**登录控制器：**

```java
package com.example.enterprise.controller;

import com.example.enterprise.dto.LoginRequest;
import com.example.enterprise.dto.LoginResponse;
import com.example.enterprise.security.CustomUserDetails;
import com.example.enterprise.security.CustomUserDetailsService;
import com.example.enterprise.util.JwtTokenUtil;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

/**
 * 认证控制器
 * 处理登录、登出、Token 刷新
 */
@Slf4j
@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {
    
    private final AuthenticationManager authenticationManager;
    private final JwtTokenUtil jwtTokenUtil;
    private final CustomUserDetailsService userDetailsService;
    
    /**
     * 用户登录
     * 
     * @param loginRequest 登录请求（用户名、密码）
     * @return 登录响应（包含 Token）
     */
    @PostMapping("/login")
    public ResponseEntity<?> login(@Valid @RequestBody LoginRequest loginRequest) {
        try {
            // 1. 认证用户
            Authentication authentication = authenticationManager.authenticate(
                    new UsernamePasswordAuthenticationToken(
                            loginRequest.getUsername(),
                            loginRequest.getPassword()
                    )
            );
            
            // 2. 生成 Token
            String accessToken = jwtTokenUtil.generateAccessToken(authentication);
            String refreshToken = jwtTokenUtil.generateRefreshToken(authentication);
            
            // 3. 更新最后登录信息
            CustomUserDetails userDetails = 
                    (CustomUserDetails) authentication.getPrincipal();
            userDetailsService.updateLastLogin(
                    userDetails.getUserId(),
                    getClientIp()
            );
            
            // 4. 构建响应
            LoginResponse response = LoginResponse.builder()
                    .accessToken(accessToken)
                    .refreshToken(refreshToken)
                    .tokenType("Bearer")
                    .username(userDetails.getUsername())
                    .userId(userDetails.getUserId())
                    .build();
            
            log.info("用户登录成功: {}", loginRequest.getUsername());
            
            return ResponseEntity.ok(response);
            
        } catch (AuthenticationException ex) {
            log.error("用户登录失败: {}", loginRequest.getUsername());
            
            // 更新失败次数
            // userDetailsService.increaseLoginFailCount(loginRequest.getUsername());
            
            return ResponseEntity.status(401)
                    .body(Map.of("message", "用户名或密码错误"));
        }
    }
    
    /**
     * 刷新 Token
     * 
     * @param refreshToken 刷新 Token
     * @return 新的访问 Token
     */
    @PostMapping("/refresh")
    public ResponseEntity<?> refreshToken(@RequestParam String refreshToken) {
        try {
            // 1. 验证刷新 Token
            if (!jwtTokenUtil.validateToken(refreshToken)) {
                return ResponseEntity.status(401)
                        .body(Map.of("message", "刷新 Token 无效"));
            }
            
            // 2. 从 Token 中提取用户名
            String username = jwtTokenUtil.getUsernameFromToken(refreshToken);
            
            // 3. 加载用户详情
            CustomUserDetails userDetails = 
                    (CustomUserDetails) userDetailsService.loadUserByUsername(username);
            
            // 4. 生成新的访问 Token
            Authentication authentication = new UsernamePasswordAuthenticationToken(
                    userDetails, null, userDetails.getAuthorities()
            );
            
            String newAccessToken = jwtTokenUtil.generateAccessToken(authentication);
            
            LoginResponse response = LoginResponse.builder()
                    .accessToken(newAccessToken)
                    .tokenType("Bearer")
                    .username(userDetails.getUsername())
                    .build();
            
            return ResponseEntity.ok(response);
            
        } catch (Exception ex) {
            log.error("刷新 Token 失败", ex);
            return ResponseEntity.status(401)
                    .body(Map.of("message", "刷新 Token 失败"));
        }
    }
    
    /**
     * 用户登出
     * JWT 是无状态的，登出只需要客户端删除 Token
     * 如果需要强制失效，可以使用 Redis 存储黑名单
     */
    @PostMapping("/logout")
    public ResponseEntity<?> logout() {
        // 可以将 Token 加入 Redis 黑名单
        // String token = extractTokenFromRequest();
        // redisTemplate.opsForValue().set("token:blacklist:" + token, "1", tokenExpiration);
        
        return ResponseEntity.ok(Map.of("message", "登出成功"));
    }
    
    /**
     * 获取当前用户信息
     */
    @GetMapping("/me")
    public ResponseEntity<?> getCurrentUser(Authentication authentication) {
        CustomUserDetails userDetails = 
                (CustomUserDetails) authentication.getPrincipal();
        
        return ResponseEntity.ok(Map.of(
                "userId", userDetails.getUserId(),
                "username", userDetails.getUsername(),
                "authorities", userDetails.getAuthorities()
        ));
    }
    
    /**
     * 获取客户端 IP 地址
     */
    private String getClientIp() {
        // 简化实现，实际应该从请求中提取
        return "127.0.0.1";
    }
}
```

**Spring Security 配置：**

```java
package com.example.enterprise.config;

import com.example.enterprise.security.CustomUserDetailsService;
import com.example.enterprise.security.JwtAccessDeniedHandler;
import com.example.enterprise.security.JwtAuthenticationEntryPoint;
import com.example.enterprise.security.JwtAuthenticationFilter;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

/**
 * Spring Security 配置
 */
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    private final CustomUserDetailsService userDetailsService;
    private final JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;
    private final JwtAccessDeniedHandler jwtAccessDeniedHandler;
    private final JwtAuthenticationFilter jwtAuthenticationFilter;
    
    /**
     * 密码编码器
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    /**
     * 认证管理器
     */
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
    
    /**
     * 配置认证管理器
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService)
                .passwordEncoder(passwordEncoder());
    }
    
    /**
     * 配置 HTTP 安全
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                // 禁用 CSRF（JWT 不需要 CSRF 保护）
                .csrf().disable()
                
                // 禁用 CORS（如果使用自定义 CORS 配置）
                .cors().disable()
                
                // 配置会话管理：无状态
                .sessionManagement()
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                
                // 配置异常处理
                .exceptionHandling()
                        .authenticationEntryPoint(jwtAuthenticationEntryPoint)
                        .accessDeniedHandler(jwtAccessDeniedHandler)
                .and()
                
                // 配置授权规则
                .authorizeRequests()
                        // 公开接口
                        .antMatchers("/api/auth/**").permitAll()
                        .antMatchers("/api/public/**").permitAll()
                        
                        // 管理员接口
                        .antMatchers("/api/admin/**").hasRole("ADMIN")
                        
                        // 其他接口需要认证
                        .anyRequest().authenticated()
                .and()
                
                // 添加 JWT 过滤器
                .addFilterBefore(jwtAuthenticationFilter, 
                        UsernamePasswordAuthenticationFilter.class);
    }
}
```

#### 23.5 完整项目结构

为了节省篇幅，这里只展示核心文件结构，完整的代码请参考项目仓库：

```
enterprise-security-backend/
├── src/main/java/com/example/enterprise/
│   ├── EnterpriseSecurityApplication.java
│   ├── config/
│   │   ├── SecurityConfig.java
│   │   ├── RedisConfig.java
│   │   └── SwaggerConfig.java
│   ├── controller/
│   │   ├── AuthController.java
│   │   ├── UserController.java
│   │   ├── RoleController.java
│   │   └── PermissionController.java
│   ├── service/
│   │   ├── UserService.java
│   │   ├── RoleService.java
│   │   ├── PermissionService.java
│   │   └── AuthService.java
│   ├── repository/
│   │   ├── UserRepository.java
│   │   ├── RoleRepository.java
│   │   └── PermissionRepository.java
│   ├── entity/
│   │   ├── User.java
│   │   ├── Role.java
│   │   └── Permission.java
│   ├── dto/
│   │   ├── LoginRequest.java
│   │   ├── LoginResponse.java
│   │   ├── UserDTO.java
│   │   └── RoleDTO.java
│   ├── security/
│   │   ├── CustomUserDetails.java
│   │   ├── CustomUserDetailsService.java
│   │   ├── JwtAuthenticationFilter.java
│   │   ├── JwtAuthenticationEntryPoint.java
│   │   └── JwtAccessDeniedHandler.java
│   ├── exception/
│   │   ├── BusinessException.java
│   │   └── GlobalExceptionHandler.java
│   └── util/
│       ├── JwtTokenUtil.java
│       └── ResponseUtil.java
└── pom.xml
```

#### 23.6 总结与FAQ

**本节总结：**

1. **用户管理**：
   - User、Role、Permission 三表关联
   - 自定义 UserDetails 实现
   - 密码加密、账户锁定

2. **JWT 认证**：
   - JWT Token 生成与验证
   - 无状态认证过滤器
   - Token 刷新机制

3. **权限控制**：
   - 基于角色的访问控制（RBAC）
   - 方法级安全（@PreAuthorize）
   - 自定义权限表达式

**FAQ：**

**Q1: JWT Token 泄露怎么办？**

A: 
- 使用 HTTPS 加密传输
- 设置合理的过期时间
- 使用刷新 Token 机制
- 将失效 Token 加入黑名单（Redis）
- 敏感操作要求重新认证

**Q2: 如何实现 Token 的主动失效？**

A: 
- 使用 Redis 存储 Token 黑名单
- 在登出时将 Token 加入黑名单
- 每次请求检查 Token 是否在黑名单中

**Q3: 如何处理 Token 的续期？**

A: 
- 使用短期访问 Token（如 30 分钟）+ 长期刷新 Token（如 7 天）
- 访问 Token 过期后使用刷新 Token 获取新的访问 Token
- 刷新 Token 只能使用一次

**Q4: 前后端分离如何处理 CORS？**

A: 
- 后端配置 CORS（允许的域名、方法、头）
- 前端携带认证头（Authorization: Bearer <token>）
- 预检请求（OPTIONS）处理

**Q5: 如何实现单点登录（SSO）？**

A: 
- 使用 OAuth2 或 SAML 协议
- 统一的认证中心
- 各系统信任认证中心颁发的 Token
- 会话同步机制

**思考题：**

1. 如何设计一个支持多租户的 SaaS 安全系统？
2. 如何实现细粒度的数据权限控制（如只能看到自己部门的数据）？
3. 如何使用 Spring Security 实现社交登录（GitHub、Google）？
4. 如何设计一个安全的 API 网关？
5. 如何实现操作审计的完整性和不可篡改性？

---

## 结语

至此，我们完成了 Spring Security 5.3.9.RELEASE 课程讲义的第四部分——**模块7-8：安全测试与监控 + 综合实战项目**。

### 内容回顾

**模块7 - 安全测试与监控**：
- 安全测试：@WithMockUser、@WithUserDetails、MockMvc 集成测试
- 源码分析：WithMockUserSecurityContextFactory、FilterChainProxy
- 安全审计：事件监听、审计日志、持久化方案
- 监控告警：WebSocket 实时监控、Prometheus 集成

**模块8 - 综合实战项目**：
- 完整的企业级安全系统架构
- 用户管理模块（User、Role、Permission）
- JWT 认证模块（无状态认证、Token 刷新）
- 前后端分离集成（CORS、CSRF 处理）
- 生产级代码示例

### 核心知识点

1. **测试体系**：
   - 使用注解快速模拟认证用户
   - MockMvc 集成安全过滤器链
   - 测试认证、授权、CSRF 保护

2. **审计监控**：
   - 监听 Spring Security 事件
   - 持久化审计日志
   - 实时监控与告警

3. **实战应用**：
   - JWT 无状态认证
   - 基于角色的权限控制（RBAC）
   - 企业级安全系统设计

### 学习建议

1. **动手实践**：
   - 运行完整项目代码
   - 编写单元测试和集成测试
   - 实现自定义的审计监听器

2. **深入源码**：
   - 阅读 FilterChainProxy 源码
   - 理解事件发布机制
   - 研究 JWT 认证流程

3. **扩展应用**：
   - 集成 OAuth2 社交登录
   - 实现单点登录（SSO）
   - 添加多因素认证（MFA）

希望这份讲义能帮助你深入理解 Spring Security 的核心原理和实战应用。如有疑问，欢迎随时交流！

---

**课程讲义全文完** 🎉
