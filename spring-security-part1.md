# Spring Security 5.3.9.RELEASE 课程讲义（第一部分）

**课程名称**: Spring Security  
**版本**: 5.3.9.RELEASE  
**层次**: 本科高级  
**重点**: 理论深度 + 实战应用  

---

## 目录

- [模块1 - Spring Security 快速入门](#模块1---spring-security-快速入门)
  - [知识点1: Spring Security 是什么？](#知识点1-spring-security-是什么)
  - [知识点2: 第一个 Spring Security 应用](#知识点2-第一个-spring-security-应用)
  - [知识点3: 核心架构概念](#知识点3-核心架构概念)
  - [知识点4: 配置方式详解](#知识点4-配置方式详解)
- [模块2 - 认证机制深度剖析](#模块2---认证机制深度剖析)
  - [知识点5: 认证核心原理](#知识点5-认证核心原理)
  - [知识点6: 用户详情服务](#知识点6-用户详情服务)
  - [知识点7: 密码编码机制](#知识点7-密码编码机制)
  - [知识点8: 常用认证方式](#知识点8-常用认证方式)

---

## 模块1 - Spring Security 快速入门

### 知识点1: Spring Security 是什么？

#### 1.1 Spring Security 概述

Spring Security 是一个功能强大且高度可定制的身份验证和访问控制框架，它是保护基于 Spring 的应用程序的事实标准。Spring Security 专注于为 Java 应用程序提供身份验证和授权功能，同时广泛支持各种认证和授权标准。

**核心特性：**

1. **全面且可扩展的身份验证和授权支持**
   - 支持多种认证方式（表单登录、HTTP Basic、Digest、X.509、LDAP 等）
   - 基于角色的访问控制（RBAC）
   - 支持自定义的认证和授权逻辑

2. **防护常见攻击**
   - CSRF（跨站请求伪造）防护
   - 会话固定攻击防护
   - 点击劫持防护
   - 安全头部配置（XSS、CSP、HSTS 等）

3. **与 Spring 生态系统无缝集成**
   - 与 Spring Framework、Spring Boot、Spring Data 等深度集成
   - 支持 Servlet API 和 Reactive 两种编程模型

4. **企业级特性**
   - 支持单点登录（SSO）
   - 支持 OAuth 2.0 和 OpenID Connect
   - 支持 JWT（JSON Web Token）

#### 1.2 历史演进

Spring Security 最初被称为 "Acegi Security"，于 2003 年由 Ben Alex 开发。2007 年底，Acegi Security 正式成为 Spring Portfolio 项目，并更名为 Spring Security。以下是主要版本的演进：

- **Spring Security 2.x**: 引入命名空间配置，简化配置
- **Spring Security 3.x**: 引入 SpEL（Spring Expression Language）支持
- **Spring Security 4.x**: 引入 WebSocket 安全支持
- **Spring Security 5.x**: 引入响应式安全支持、OAuth 2.0 支持、密码编码现代化
- **Spring Security 5.3.9.RELEASE**: 本课程使用的版本，是 5.3 系列的稳定版本

#### 1.3 核心设计理念

Spring Security 的设计基于以下几个核心理念：

**1.3.1 安全过滤器链（Filter Chain）**

Spring Security 通过 Servlet 过滤器链来实现安全功能。所有请求都会经过一系列过滤器，每个过滤器负责特定的安全功能。

```java
// 核心过滤器链示例（概念图）
请求 → FilterChainProxy
         → SecurityContextPersistenceFilter  // 加载 SecurityContext
         → UsernamePasswordAuthenticationFilter  // 处理表单登录
         → ExceptionTranslationFilter  // 处理安全异常
         → FilterSecurityInterceptor  // 最终的访问决策
         → 应用程序
```

**1.3.2 认证（Authentication）与授权（Authorization）分离**

- **认证（Authentication）**: 解决"你是谁"的问题，验证用户身份
- **授权（Authorization）**: 解决"你能做什么"的问题，控制用户访问权限

**1.3.3 SecurityContext 持有者模式**

使用 `SecurityContextHolder` 来存储当前认证用户的安全上下文信息。

#### 1.4 源码分析：核心类详解

##### 1.4.1 SecurityContextHolder 类

`SecurityContextHolder` 是 Spring Security 的核心类之一，它负责存储当前认证用户的安全上下文。

```java
package org.springframework.security.core.context;

/**
 * SecurityContextHolder 将 SecurityContext 与当前执行线程关联。
 * 
 * 这个类使用了策略模式，通过不同的策略实现来决定如何存储 SecurityContext：
 * - MODE_THREADLOCAL: 使用 ThreadLocal 存储默认策略）
 * - MODE_INHERITABLETHREADLOCAL: 使用 InheritableThreadLocal 存储子线程可继承）
 * - MODE_GLOBAL: 使用全局静态变量存储
 */
public class SecurityContextHolder {

    // ~ Static fields/initializers
    // =====================================================================================

    /**
     * 系统属性名，用于指定存储策略
     */
    public static final String MODE_THREADLOCAL = "MODE_THREADLOCAL";
    public static final MODE_INHERITABLETHREADLOCAL = "MODE_INHERITABLETHREADLOCAL";
    public static final String MODE_GLOBAL = "MODE_GLOBAL";

    private static String strategyName = System.getProperty("spring.security.strategy", MODE_THREADLOCAL);

    /**
     * SecurityContextHolderStrategy 实例，负责实际的存储操作
     */
    private static SecurityContextHolderStrategy strategy;

    // ~ Methods
    // ========================================================================================================

    /**
     * 初始化策略，根据 strategyName 创建相应的策略实例
     */
    static {
        initialize();
    }

    /**
     * 设置当前的安全上下文
     * @param context 要设置的安全上下文
     */
    public static void setContext(SecurityContext context) {
        strategy.setContext(context);
    }

    /**
     * 获取当前的安全上下文
     * @return 当前的 SecurityContext
     */
    public static SecurityContext getContext() {
        return strategy.getContext();
    }

    /**
     * 清除当前的安全上下文
     */
    public static void clearContext() {
        strategy.clearContext();
    }

    /**
     * 创建一个新的空安全上下文
     * @return 新的 SecurityContext 实例
     */
    public static SecurityContext createEmptyContext() {
        return strategy.createEmptyContext();
    }

    /**
     * 设置存储策略
     * @param strategyName 策略名称
     */
    public static void setStrategyName(String strategyName) {
        SecurityContextHolder.strategyName = strategyName;
        initialize();
    }

    /**
     * 获取当前策略名称
     * @return 策略名称
     */
    public static String getStrategyName() {
        return strategyName;
    }

    /**
     * 获取当前策略实例
     * @return SecurityContextHolderStrategy 实例
     */
    public static SecurityContextHolderStrategy getStrategy() {
        return strategy;
    }

    /**
     * 初始化策略实例
     */
    private static void initialize() {
        if (MODE_THREADLOCAL.equals(strategyName)) {
            strategy = new ThreadLocalSecurityContextHolderStrategy();
        }
        else if (MODE_INHERITABLETHREADLOCAL.equals(strategyName)) {
            strategy = new InheritableThreadLocalSecurityContextHolderStrategy();
        }
        else if (MODE_GLOBAL.equals(strategyName)) {
            strategy = new GlobalSecurityContextHolderStrategy();
        }
        else {
            // 尝试加载自定义策略类
            try {
                Class<?> clazz = Class.forName(strategyName);
                Constructor<?> customStrategy = clazz.getConstructor();
                strategy = (SecurityContextHolderStrategy) customStrategy.newInstance();
            }
            catch (Exception ex) {
                throw new IllegalArgumentException("Could not instantiate SecurityContextHolderStrategy of type " + strategyName, ex);
            }
        }
    }
}
```

**源码分析要点：**

1. **策略模式的应用**: `SecurityContextHolder` 使用策略模式，通过 `SecurityContextHolderStrategy` 接口来实现不同的存储策略。这种设计使得框架非常灵活，可以根据不同的应用场景选择最合适的存储方式。

2. **三种默认策略**:
   - `MODE_THREADLOCAL`: 使用 `ThreadLocal` 存储安全上下文，这是默认策略，适用于普通的 Web 应用
   - `MODE_INHERITABLETHREADLOCAL`: 使用 `InheritableThreadLocal`，允许子线程访问父线程的安全上下文
   - `MODE_GLOBAL`: 使用全局静态变量，适用于独立应用程序或需要跨线程共享的场景

3. **扩展性**: 支持自定义策略，可以通过设置系统属性 `spring.security.strategy` 来指定自定义的策略类名。

4. **静态方法设计**: 所有方法都是静态的，这使得在应用程序的任何地方都可以方便地访问和操作安全上下文。

##### 1.4.2 SecurityContext 接口

`SecurityContext` 接口表示当前执行线程的安全上下文，主要持有认证对象。

```java
package org.springframework.security.core.context;

import java.io.Serializable;

/**
 * SecurityContext 接口定义了安全上下文的基本操作
 * 
 * 主要职责：
 * 1. 持有 Authentication 对象（认证信息）
 * 2. 提供认证信息的访问和修改方法
 * 
 * 实现：SecurityContext 接口的主要实现类是 SecurityContextImpl
 */
public interface SecurityContext extends Serializable {

    /**
     * 获取当前认证对象
     * @return Authentication 对象，如果未认证则返回 null
     */
    Authentication getAuthentication();

    /**
     * 设置当前认证对象
     * @param authentication 要设置的认证对象
     */
    void setAuthentication(Authentication authentication);
}
```

**源码分析要点：**

1. **简单而强大的设计**: `SecurityContext` 接口非常简单，只有两个方法，但它是整个 Spring Security 安全模型的核心。

2. **持有 Authentication 对象**: `SecurityContext` 本质上是一个 `Authentication` 对象的容器，它提供了类型安全的访问方式。

3. **可序列化**: 接口继承了 `Serializable`，这使得安全上下文可以被序列化，对于会话复制和集群环境非常重要。

##### 1.4.3 Authentication 接口

`Authentication` 接口是 Spring Security 中表示认证请求或认证主体的核心接口。

```java
package org.springframework.security.core;

import java.io.Serializable;
import java.security.Principal;
import java.util.Collection;
import java.util.Collections;
import java.util.Set;

/**
 * Authentication 接口用于表示认证请求或已认证的主体
 * 
 * 在 Spring Security 中，Authentication 对象有两个生命周期：
 * 1. 认证前：代表认证请求，包含用户提供的认证信息（用户名、密码等）
 * 2. 认证后：代表已认证的主体，包含用户的详细信息、权限等
 * 
 * 主要实现类：
 * - UsernamePasswordAuthenticationToken: 基于用户名密码的认证
 * - RememberMeAuthenticationToken: 记住我功能的认证
 * - AnonymousAuthenticationToken: 匿名用户的认证
 * - TestingAuthenticationToken: 用于测试的认证
 */
public interface Authentication extends Principal, Serializable {

    /**
     * 获取用户的权限集合
     * 
     * 注意：如果对象尚未认证，应该返回空集合，而不是 null
     * 如果对象已认证，通常应该返回不可修改的集合
     * 
     * @return 权限集合，永远不为 null
     */
    Collection<? extends GrantedAuthority> getAuthorities();

    /**
     * 获取认证凭据
     * 
     * 在认证前，通常是用户提供的密码
     * 在认证后，出于安全考虑，通常应该将凭据清除（设为 null）
     * 
     * @return 认证凭据，可能是密码或其他凭据
     */
    Object getCredentials();

    /**
     * 获取认证主体的详细信息
     * 
     * 可能是用户详情对象、用户 ID、用户名等
     * 在大多数实现中，这是 UserDetails 的实现类
     * 
     * @return 认证主体的详细信息
     */
    Object getDetails();

    /**
     * 获取认证主体的标识
     * 
     * 在大多数情况下，这是用户名
     * 不要返回代表用户隐私的敏感信息（如密码）
     * 
     * @return 认证主体的标识
     */
    Object getPrincipal();

    /**
     * 检查是否已认证
     * 
     * @return 如果已认证返回 true，否则返回 false
     */
    boolean isAuthenticated();

    /**
     * 设置认证状态
     * 
     * @param isAuthenticated true 表示已认证，false 表示未认证
     * @throws IllegalArgumentException 如果设置为 true 但提供的数据无效
     */
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;

    /**
     * 获取认证对象的名称
     * 
     * 默认实现返回 principal 的字符串表示
     * 
     * @return 认证对象的名称
     */
    @Override
    String getName();
}
```

**源码分析要点：**

1. **双向生命周期**: `Authentication` 对象在认证前后扮演不同的角色：
   - **认证前**：代表用户的认证请求，包含用户名、密码等原始认证信息
   - **认证后**：代表已认证的主体，包含完整的用户信息和权限

2. **五个核心方法**:
   - `getAuthorities()`: 返回用户的权限集合
   - `getCredentials()`: 返回认证凭据（通常是密码）
   - `getDetails()`: 返回额外的认证详情（如 IP 地址、会话 ID 等）
   - `getPrincipal()`: 返回主体的标识（通常是用户名或 UserDetails 对象）
   - `isAuthenticated()`: 返回认证状态

3. **安全性考虑**:
   - 认证后应该清除敏感凭据（如密码）
   - `getAuthorities()` 永远不应该返回 null
   - `getName()` 不应该返回敏感信息

4. **继承 Principal 接口**: `Authentication` 继承了 Java 标准的 `Principal` 接口，这使得它可以与 Java 安全框架集成。

##### 1.4.4 GrantedAuthority 接口

`GrantedAuthority` 接口表示授予用户的权限。

```java
package org.springframework.security.core;

import java.io.Serializable;

/**
 * GrantedAuthority 接口表示授予用户的权限
 * 
 * 在 Spring Security 中，权限分为两类：
 * 1. 角色（Role）: 如 ROLE_ADMIN, ROLE_USER
 * 2. 权限（Permission）: 如 READ_PRIVILEGES, WRITE_PRIVILEGES
 * 
 * 注意：Spring Security 框架本身不区分角色和权限，
 * 它们都作为 GrantedAuthority 处理。
 * 但是按照约定，角色通常以 "ROLE_" 前缀命名。
 * 
 * 常见实现类：
 * - SimpleGrantedAuthority: 最简单的实现，只存储权限字符串
 */
public interface GrantedAuthority extends Serializable {

    /**
     * 获取权限字符串
     * 
     * @return 权限字符串，不能为 null
     */
    String getAuthority();
}
```

**源码分析要点：**

1. **简单的设计**: `GrantedAuthority` 接口只有一个方法 `getAuthority()`，返回权限的字符串表示。

2. **角色与权限**: Spring Security 不区分角色和权限，它们都是 `GrantedAuthority`。通过约定，角色通常以 `ROLE_` 前缀命名。

3. **常见实现**: `SimpleGrantedAuthority` 是最常用的实现类，它简单地存储权限字符串。

#### 1.5 Spring Security 与其他安全框架对比

| 特性 | Spring Security | Apache Shiro | JAAS |
|------|----------------|--------------|------|
| **学习曲线** | 陡峭 | 相对平缓 | 陡峭 |
| **Spring 集成** | 原生集成 | 需要额外配置 | Java 标准 |
| **灵活性** | 高 | 高 | 中 |
| **社区支持** | 强大 | 较弱 | 弱 |
| **认证方式** | 丰富 | 较丰富 | 有限 |
| **企业级特性** | 完善 | 一般 | 基础 |

**选择 Spring Security 的理由：**

1. **Spring 生态系统的原生支持**: 如果你已经使用 Spring，Spring Security 是最自然的选择
2. **企业级特性**: 支持单点登录、OAuth 2.0、JWT 等企业级功能
3. **活跃的社区和持续的更新**: Spring Security 拥有庞大的社区和持续的版本更新
4. **强大的扩展性**: 几乎所有组件都可以自定义和扩展

#### 1.6 Spring Security 5.3.9.RELEASE 新特性

Spring Security 5.3.9.RELEASE 是 5.3 系列的一个维护版本，主要修复了一些关键 bug。以下是 5.3 系列引入的一些重要特性：

1. **OAuth 2.0 支持**: 对 OAuth 2.0 客户端、资源服务器和授权服务器的支持
2. **密码编码的现代化**: 引入了新的密码编码器 API
3. **SAML 2.0 支持**: 增强了对 SAML 2.0 的支持
4. **Reactive 安全**: 完全支持 Spring WebFlux 的响应式安全
5. **改进的测试支持**: 更好的测试工具和方法

#### 1.7 实际应用场景

Spring Security 适用于以下场景：

1. **企业内部系统**: 需要 RBAC（基于角色的访问控制）的企业应用
2. **SaaS 应用**: 需要多租户隔离和细粒度权限控制
3. **API 服务**: RESTful API 的认证和授权
4. **单点登录（SSO）**: 企业内部多个应用的统一认证
5. **社交登录**: 集成第三方社交账号登录
6. **微服务安全**: 分布式系统的安全通信

#### 1.8 常见问题（FAQ）

**Q1: Spring Security 和 Shiro 有什么区别？我应该选择哪一个？**

A: 主要区别：
- **Spring Security**: 功能更强大，Spring 生态原生支持，学习曲线陡峭
- **Shiro**: 更简单直观，学习曲线平缓，功能相对简单

选择建议：
- 如果你的项目基于 Spring，优先选择 Spring Security
- 如果需要复杂的企业级功能（如 SSO、OAuth 2.0），选择 Spring Security
- 如果项目简单，快速上手更重要，可以考虑 Shiro

**Q2: Spring Security 是否会显著降低应用性能？**

A: Spring Security 的性能影响主要来自：
- 过滤器链的处理：每个请求都会经过多个过滤器
- 数据库查询：每次认证都需要查询用户信息

优化建议：
- 使用缓存（如 Redis）缓存用户信息
- 合理配置会话管理策略
- 避免不必要的数据库查询
- 使用异步安全处理（对于响应式应用）

**Q3: ROLE_ 前缀是必须的吗？**

A: `ROLE_` 前缀不是必须的，但这是一个强烈推荐的约定。

- Spring Security 的一些表达式（如 `hasRole()`）会自动添加 `ROLE_` 前缀
- 使用 `hasAuthority()` 可以避免自动添加前缀
- 保持约定有助于代码的可读性和维护性

示例：
```java
// hasRole() 会自动添加 ROLE_ 前缀
http.authorizeRequests()
    .antMatchers("/admin/**").hasRole("ADMIN");  // 实际上是 ROLE_ADMIN

// hasAuthority() 不会添加前缀
http.authorizeRequests()
    .antMatchers("/admin/**").hasAuthority("ADMIN");  // 需要 ADMIN 权限
```

**Q4: 在微服务架构中，每个服务都需要 Spring Security 吗？**

A: 这取决于你的安全架构设计：

**方案 1: API 网关集中认证**
- API 网关负责认证，内部服务信任网关
- 内部服务只需要验证 JWT Token
- 简化内部服务配置

**方案 2: 每个服务独立认证**
- 每个服务都配置 Spring Security
- 服务间调用也需要认证
- 更安全，但配置更复杂

推荐：大多数情况下使用方案 1，在 API 网关集中处理认证。

**Q5: Spring Security 可以用于非 Web 应用吗？**

A: 是的，Spring Security 不仅限于 Web 应用。

- **独立应用**: 可以使用 `AuthenticationManager` 进行编程式认证
- **消息驱动应用**: 可以集成 Spring Security 进行消息认证
- **响应式应用**: Spring Security 5+ 完全支持响应式安全

示例：
```java
// 在非 Web 应用中使用 Spring Security
public class StandaloneAuthExample {
    public static void main(String[] args) {
        AuthenticationManager authManager = ...;
        
        Authentication auth = new UsernamePasswordAuthenticationToken(
            "user", "password"
        );
        
        Authentication result = authManager.authenticate(auth);
        
        if (result.isAuthenticated()) {
            System.out.println("认证成功！");
        }
    }
}
```

#### 1.9 思考题

1. **理解 SecurityContextHolder 的存储策略**:
   - 问题：在什么情况下应该使用 `MODE_INHERITABLETHREADLOCAL` 而不是默认的 `MODE_THREADLOCAL`？
   - 提示：考虑异步任务执行、子线程创建等场景

2. **认证对象的生命周期**:
   - 问题：在认证过程中，Authentication 对象的 `credentials` 字段为什么会从密码变为 null？
   - 提示：考虑内存安全和会话持久化

3. **权限与角色的设计**:
   - 问题：如何设计一个既支持角色（如 ADMIN）又支持细粒度权限（如 USER:READ）的权限模型？
   - 提示：考虑 `GrantedAuthority` 的实现方式

4. **性能优化**:
   - 问题：在高并发场景下，如何优化 Spring Security 的性能？
   - 提示：考虑缓存、异步处理、过滤器优化等

5. **扩展性设计**:
   - 问题：如果需要支持多种认证方式（用户名密码、手机验证码、第三方登录），如何设计架构？
   - 提示：考虑 Provider 模式和认证链模式

---

### 知识点2: 第一个 Spring Security 应用

#### 2.1 环境准备

在开始之前，我们需要准备开发环境。

**2.1.1 系统要求**

- JDK 8 或更高版本（推荐 JDK 11）
- Maven 3.6+ 或 Gradle 6+
- IDE（推荐 IntelliJ IDEA 或 Eclipse）

**2.1.2 创建 Spring Boot 项目**

使用 Spring Initializr（https://start.spring.io/）创建项目：

**项目配置**:
- **Project**: Maven Project
- **Language**: Java
- **Spring Boot**: 2.7.18（兼容 Spring Security 5.3.9）
- **Group**: com.example
- **Artifact**: spring-security-demo
- **Name**: Spring Security Demo
- **Description**: First Spring Security Application
- **Package Name**: com.example.security
- **Packaging**: Jar
- **Java**: 11

**添加依赖**:
- Spring Web
- Spring Security
- Thymeleaf（用于视图）
- Lombok（可选，简化代码）
- Spring Boot DevTools（可选，热重载）

**2.1.3 Maven 依赖配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.18</version>
        <relativePath/>
    </parent>
    
    <groupId>com.example</groupId>
    <artifactId>spring-security-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-security-demo</name>
    <description>First Spring Security Application</description>
    
    <properties>
        <java.version>11</java.version>
        <spring-security.version>5.3.9.RELEASE</spring-security.version>
    </properties>
    
    <dependencies>
        <!-- Spring Web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- Spring Security -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        
        <!-- Thymeleaf 模板引擎 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        
        <!-- Thymeleaf Spring Security 集成 -->
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        </dependency>
        
        <!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        
        <!-- Spring Boot DevTools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        
        <!-- 测试依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**依赖说明**:

1. **spring-boot-starter-web**: 提供了 Spring MVC 的支持，包含 Tomcat 和 Spring Web
2. **spring-boot-starter-security**: Spring Security 的核心依赖
3. **spring-boot-starter-thymeleaf**: Thymeleaf 模板引擎
4. **thymeleaf-extras-springsecurity5**: Thymeleaf 与 Spring Security 的集成，允许在模板中访问安全信息

#### 2.2 创建第一个应用

**2.2.1 主应用类**

```java
package com.example.security;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * Spring Boot 应用主类
 * 
 * @SpringBootApplication 注解是一个组合注解，包含：
 * - @Configuration: 标识为配置类
 * - @EnableAutoConfiguration: 启用自动配置
 * - @ComponentScan: 组件扫描
 */
@SpringBootApplication
public class SpringSecurityDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringSecurityDemoApplication.class, args);
    }
}
```

**2.2.2 创建控制器**

创建一个简单的控制器来测试安全功能。

```java
package com.example.security.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

/**
 * 首页控制器
 */
@Controller
public class HomeController {

    /**
     * 首页
     * @return 首页视图名称
     */
    @GetMapping("/")
    public String home() {
        return "index";
    }
    
    /**
     * 关于页面
     * @return 关于页面视图名称
     */
    @GetMapping("/about")
    public String about() {
        return "about";
    }
    
    /**
     * 管理员页面
     * @return 管理员页面视图名称
     */
    @GetMapping("/admin")
    public String admin() {
        return "admin";
    }
}
```

**2.2.3 创建视图模板**

在 `src/main/resources/templates/` 目录下创建视图文件。

**index.html**:
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
<head>
    <meta charset="UTF-8">
    <title>首页 - Spring Security Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 40px;
        }
        .info {
            background-color: #e7f3fe;
            border-left: 6px solid #2196F3;
            padding: 15px;
            margin: 20px 0;
        }
        .warning {
            background-color: #ffffcc;
            border-left: 6px solid #ffeb3b;
            padding: 15px;
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <h1>欢迎来到 Spring Security Demo</h1>
    
    <div class="info">
        <h2>当前用户信息</h2>
        <p>用户名: <span sec:authentication="name">未登录</span></p>
        <p>角色: <span sec:authentication="principal.authorities">无</span></p>
        <p>认证状态: <span sec:authentication="authenticated ? '已认证' : '未认证'">未认证</span></p>
    </div>
    
    <div class="warning" sec:authorize="isAnonymous()">
        <h3>您还未登录</h3>
        <p>部分功能需要登录才能访问。</p>
    </div>
    
    <div class="info" sec:authorize="isAuthenticated()">
        <h3>您已成功登录</h3>
        <p>欢迎回来，<span sec:authentication="name"></span>！</p>
        
        <!-- 登出表单 -->
        <form th:action="@{/logout}" method="post">
            <button type="submit">登出</button>
        </form>
    </div>
    
    <nav>
        <h2>导航</h2>
        <ul>
            <li><a href="/">首页</a></li>
            <li><a href="/about">关于</a></li>
            <li><a href="/admin">管理员页面</a></li>
        </ul>
    </nav>
</body>
</html>
```

**about.html**:
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>关于 - Spring Security Demo</title>
</head>
<body>
    <h1>关于本项目</h1>
    <p>这是一个 Spring Security 学习示例项目。</p>
    <p>版本: Spring Security 5.3.9.RELEASE</p>
    <a href="/">返回首页</a>
</body>
</html>
```

**admin.html**:
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
<head>
    <meta charset="UTF-8">
    <title>管理员页面 - Spring Security Demo</title>
</head>
<body>
    <h1>管理员页面</h1>
    
    <div sec:authorize="hasRole('ADMIN')">
        <h2>欢迎，管理员！</h2>
        <p>这里是管理员专区。</p>
    </div>
    
    <div sec:authorize="!hasRole('ADMIN')">
        <h2>访问拒绝</h2>
        <p>您没有权限访问此页面。</p>
    </div>
    
    <a href="/">返回首页</a>
</body>
</html>
```

**2.2.4 应用配置文件**

创建 `application.yml` 配置文件。

```yaml
# application.yml

server:
  port: 8080
  servlet:
    context-path: /

spring:
  application:
    name: spring-security-demo
  
  # Thymeleaf 配置
  thymeleaf:
    cache: false  # 开发时禁用缓存，方便调试
    prefix: classpath:/templates/
    suffix: .html
  
  # 安全配置（Spring Boot 2.x）
  security:
    user:
      name: user  # 默认用户名
      password: 123456  # 默认密码（生产环境请使用更安全的密码）
      roles: USER  # 默认角色

# 日志配置
logging:
  level:
    org.springframework.security: DEBUG  # 开发时开启 DEBUG 日志
    com.example.security: DEBUG
```

**或者使用 `application.properties`**:

```properties
# application.properties

server.port=8080
spring.application.name=spring-security-demo

# Thymeleaf 配置
spring.thymeleaf.cache=false
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html

# Spring Security 默认用户配置
spring.security.user.name=user
spring.security.user.password=123456
spring.security.user.roles=USER

# 日志配置
logging.level.org.springframework.security=DEBUG
logging.level.com.example.security=DEBUG
```

#### 2.3 运行和测试

**2.3.1 启动应用**

在 IDE 中运行 `SpringSecurityDemoApplication` 类，或在命令行中执行：

```bash
mvn spring-boot:run
```

或者先打包再运行：

```bash
mvn clean package
java -jar target/spring-security-demo-0.0.1-SNAPSHOT.jar
```

**2.3.2 测试默认行为**

1. **访问首页**:
   - 打开浏览器访问 `http://localhost:8080/`
   - 你会看到页面显示"未登录"状态

2. **访问管理员页面**:
   - 访问 `http://localhost:8080/admin`
   - Spring Security 会自动重定向到登录页面
   - 默认登录页面 URL: `http://localhost:8080/login`

3. **登录**:
   - 用户名: `user`
   - 密码: `123456`
   - 登录成功后会重定向到之前访问的页面

**2.3.3 观察控制台日志**

在 DEBUG 模式下，你会看到详细的安全日志：

```
2024-03-14 10:00:00.000 DEBUG 12345 --- [main] o.s.s.web.DefaultSecurityFilterChain     : Creating filter chain: any request, []
2024-03-14 10:00:00.001 DEBUG 12345 --- [main] s.s.w.c.SecurityContextPersistenceFilter : Set SecurityContextHolder to empty SecurityContext
2024-03-14 10:00:00.002 DEBUG 12345 --- [main] o.s.s.w.a.UsernamePasswordAuthenticationFilter : Request is to process authentication
```

#### 2.4 源码分析：Spring Boot 自动配置

**2.4.1 SpringBootWebSecurityConfiguration 类**

Spring Boot 通过 `SpringBootWebSecurityConfiguration` 类提供 Spring Security 的默认配置。

```java
package org.springframework.boot.autoconfigure.security.servlet;

/**
 * Spring Boot 的 Spring Security 默认配置
 * 
 * 这个类负责在没有自定义 SecurityConfiguration 时提供默认配置：
 * 1. 创建默认的 FilterChainProxy
 * 2. 配置基本的认证和授权规则
 * 3. 设置默认的用户详情服务
 */
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnDefaultWebSecurity
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
@ConditionalOnClass({SecurityFilterChain.class, HttpSecurity.class})
@EnableConfigurationProperties({SecurityProperties.class})
@Order(100)
public class SpringBootWebSecurityConfiguration {

    /**
     * 默认的安全过滤器链配置
     * 
     * 创建条件：
     * 1. 没有自定义的 SecurityFilterChain Bean
     * 2. 没有自定义的 WebSecurityConfigurerAdapter（已过时）
     */
    @Bean
    @Order(SecurityProperties.BASIC_AUTH_ORDER)
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 创建默认的过滤器链配置
        DefaultSecurityFilterChain builder = http
            .authorizeRequests()
                .anyRequest().authenticated()  // 所有请求都需要认证
                .and()
            .formLogin()  // 启用表单登录
                .and()
            .httpBasic();  // 启用 HTTP Basic 认证
        
        return builder.build();
    }

    /**
     * 默认的用户详情服务
     * 
     * 从 application.yml/properties 读取配置：
     * - spring.security.user.name
     * - spring.security.user.password
     * - spring.security.user.roles
     */
    @Bean
    @ConditionalOnMissingBean(
        type = "org.springframework.security.authentication.AuthenticationEventPublisher"
    )
    public defaultAuthenticationEventPublisher(AuthenticationEventPublisher authenticationEventPublisher) {
        return new DefaultAuthenticationEventPublisher(applicationEventPublisher);
    }
}
```

**源码分析要点**:

1. **自动配置的条件**: 
   - `@ConditionalOnDefaultWebSecurity`: 在默认 Web 安全配置下生效
   - `@ConditionalOnMissingBean`: 当没有自定义的 `SecurityFilterChain` Bean 时生效

2. **默认的安全规则**:
   - 所有请求都需要认证（`anyRequest().authenticated()`）
   - 支持表单登录（`formLogin()`）
   - 支持 HTTP Basic 认证（`httpBasic()`）

3. **用户详情服务**: Spring Boot 会根据配置文件创建一个默认的 `UserDetailsManager`

**2.4.2 UserDetailsServiceAutoConfiguration 类**

```java
package org.springframework.boot.autoconfigure.security.servlet;

/**
 * 用户详情服务自动配置
 * 
 * 负责创建默认的 UserDetailsService 实现
 */
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnClass({AuthenticationManager.class})
@ConditionalOnMissingBean(
    type = {"org.springframework.security.authentication.AuthenticationManager", 
            "org.springframework.security.config.annotation.web.configuration.EnableWebSecurity"}
)
@EnableConfigurationProperties({SecurityProperties.class})
public class UserDetailsServiceAutoConfiguration {

    /**
     * 创建默认的 InMemoryUserDetailsManager
     * 
     * 从配置文件读取用户信息：
     * - spring.security.user.name
     * - spring.security.user.password
     * - spring.security.user.roles
     */
    @Bean
    @ConditionalOnMissingBean(
        type = {"org.springframework.security.core.userdetails.UserDetailsService", 
                "org.springframework.security.provisioning.UserDetailsManager"}
    )
    public InMemoryUserDetailsManager inMemoryUserDetailsManager(SecurityProperties properties) {
        // 获取配置的用户属性
        SecurityProperties.User user = properties.getUser();
        
        // 创建用户详情列表
        List<String> roles = user.getRoles();
        List<GrantedAuthority> authorities = new ArrayList<>();
        for (String role : roles) {
            authorities.add(new SimpleGrantedAuthority("ROLE_" + role));
        }
        
        // 创建用户详情
        UserDetails userDetails = User.builder()
            .username(user.getName())
            .password(user.getPassword())
            .authorities(authorities)
            .build();
        
        // 创建内存中的用户详情管理器
        return new InMemoryUserDetailsManager(Collections.singletonList(userDetails));
    }
}
```

#### 2.5 自定义配置

虽然默认配置很有用，但在实际应用中，我们通常需要自定义配置。让我们创建一个自定义的安全配置类。

**2.5.1 创建配置类**

```java
package com.example.security.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

/**
 * Spring Security 配置类
 * 
 * @EnableWebSecurity 注解的作用：
 * 1. 导入 WebSecurityConfiguration 配置类
 * 2. 启用 @EnableGlobalAuthentication
 * 3. 启用 WebSecurityCustomizer Bean
 * 
 * 注意：Spring Security 5.7+ 推荐使用基于 Bean 的配置方式，
 * 而不是继承 WebSecurityConfigurerAdapter（已过时）
 */
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    /**
     * 配置安全过滤器链
     * 
     * 这个 Bean 替代了传统的 WebSecurityConfigurerAdapter.configure(HttpSecurity) 方法
     * 
     * @param http HttpSecurity 配置对象
     * @return SecurityFilterChain 安全过滤器链
     * @throws Exception 配置异常
     */
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            // 授权配置
            .authorizeHttpRequests(authorize -> authorize
                // 首页和关于页面允许所有人访问
                .requestMatchers("/", "/about", "/webjars/**", "/css/**", "/js/**").permitAll()
                // 管理员页面需要 ADMIN 角色
                .requestMatchers("/admin/**").hasRole("ADMIN")
                // 其他所有请求都需要认证
                .anyRequest().authenticated()
            )
            // 表单登录配置
            .formLogin(form -> form
                // 自定义登录页面
                .loginPage("/login")
                // 登录处理 URL（默认是 /login）
                .loginProcessingUrl("/login")
                // 登录成功后的默认页面
                .defaultSuccessUrl("/", true)
                // 登录失败 URL
                .failureUrl("/login?error=true")
                // 允许所有人访问登录页面
                .permitAll()
            )
            // 登出配置
            .logout(logout -> logout
                // 登出 URL（默认是 /logout）
                .logoutUrl("/logout")
                // 登出成功后跳转的页面
                .logoutSuccessUrl("/login?logout=true")
                // 使会话失效
                .invalidateHttpSession(true)
                // 清除认证信息
                .clearAuthentication(true)
                // 允许所有人访问登出功能
                .permitAll()
            )
            // HTTP Basic 认证配置
            .httpBasic(Customizer.withDefaults());

        return http.build();
    }

    /**
     * 配置密码编码器
     * 
     * BCryptPasswordEncoder 是 Spring Security 推荐的密码编码器：
     * - 使用 BCrypt 哈希算法
     * - 自动加盐
     * - 可以调整计算强度（4-31，默认 10）
     * 
     * @return PasswordEncoder 密码编码器
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    /**
     * 配置用户详情服务
     * 
     * 这里使用内存中的用户详情管理器（仅用于演示）
     * 生产环境应该使用数据库或 LDAP 等持久化存储
     * 
     * @return UserDetailsService 用户详情服务
     */
    @Bean
    public UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
        // 创建普通用户
        UserDetails user = User.builder()
            .username("user")
            .password(passwordEncoder.encode("123456"))
            .roles("USER")
            .build();

        // 创建管理员用户
        UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder.encode("admin123"))
            .roles("ADMIN", "USER")
            .build();

        // 创建内存中的用户详情管理器
        return new InMemoryUserDetailsManager(user, admin);
    }
}
```

**配置说明**:

1. **@EnableWebSecurity**: 启用 Web 安全功能
2. **SecurityFilterChain**: 配置安全过滤器链
3. **PasswordEncoder**: 密码编码器，使用 BCrypt
4. **UserDetailsService**: 用户详情服务，从内存加载用户

**2.5.2 创建自定义登录页面**

```java
package com.example.security.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

/**
 * 登录控制器
 */
@Controller
public class LoginController {

    /**
     * 显示登录页面
     * 
     * @param error 登录失败标志
     * @param logout 登出标志
     * @param model 模型对象
     * @return 登录页面视图
     */
    @GetMapping("/login")
    public String login(
            @RequestParam(value = "error", required = false) String error,
            @RequestParam(value = "logout", required = false) String logout,
            Model model) {
        
        if (error != null) {
            model.addAttribute("error", "用户名或密码错误");
        }
        
        if (logout != null) {
            model.addAttribute("message", "您已成功登出");
        }
        
        return "login";
    }
}
```

**login.html**:
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>登录 - Spring Security Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f5f5f5;
        }
        .login-container {
            background-color: white;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            width: 350px;
        }
        .login-container h2 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            color: #666;
        }
        .form-group input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .btn-login {
            width: 100%;
            padding: 12px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        .btn-login:hover {
            background-color: #45a049;
        }
        .alert {
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 4px;
        }
        .alert-error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        .alert-success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        .info {
            text-align: center;
            margin-top: 20px;
            color: #666;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div class="login-container">
        <h2>用户登录</h2>
        
        <div th:if="${error}" class="alert alert-error" th:text="${error}"></div>
        <div th:if="${message}" class="alert alert-success" th:text="${message}"></div>
        
        <form th:action="@{/login}" method="post">
            <div class="form-group">
                <label for="username">用户名:</label>
                <input type="text" id="username" name="username" required autofocus>
            </div>
            
            <div class="form-group">
                <label for="password">密码:</label>
                <input type="password" id="password" name="password" required>
            </div>
            
            <button type="submit" class="btn-login">登录</button>
        </form>
        
        <div class="info">
            <p>测试账号：</p>
            <p>普通用户: user / 123456</p>
            <p>管理员: admin / admin123</p>
        </div>
    </div>
</body>
</html>
```

#### 2.6 测试自定义配置

**2.6.1 测试流程**

1. **启动应用**
2. **访问首页**: `http://localhost:8080/`
   - 应该可以直接访问（不需要登录）
3. **访问管理员页面**: `http://localhost:8080/admin`
   - 应该重定向到登录页面
4. **使用 user 账号登录**:
   - 用户名: `user`
   - 密码: `123456`
   - 登录后应该看到"访问拒绝"（user 没有 ADMIN 角色）
5. **登出后再用 admin 账号登录**:
   - 用户名: `admin`
   - 密码: `admin123`
   - 应该可以成功访问管理员页面

**2.6.2 验证密码编码**

在控制台日志中，你会看到密码已经被编码：

```
2024-03-14 10:00:00.000  INFO 12345 --- [main] o.s.b.a.e.UserDetailsServiceAutoConfiguration : 
Using generated security password: 123456

Encoded password: $2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy
```

#### 2.7 常见问题（FAQ）

**Q1: 为什么我访问任何页面都会自动跳转到登录页面？**

A: 这是 Spring Security 的默认行为。在你的配置中，如果你使用了 `anyRequest().authenticated()`，所有未匹配的请求都需要认证。

解决方法：
- 使用 `permitAll()` 允许特定页面不需要认证
- 示例：
```java
http.authorizeHttpRequests(auth -> auth
    .requestMatchers("/public/**", "/login").permitAll()  // 公开页面
    .anyRequest().authenticated()  // 其他页面需要认证
);
```

**Q2: 如何禁用默认的自动生成的密码？**

A: Spring Boot 会在控制台打印一个默认密码（如果只配置了用户名）。要禁用这个行为：

方法 1: 在配置文件中明确设置密码
```yaml
spring:
  security:
    user:
      name: user
      password: your-secure-password  # 明确设置密码
      roles: USER
```

方法 2: 创建自己的 UserDetailsService Bean（如本示例所示）

**Q3: 登录后如何获取当前登录用户的信息？**

A: 有多种方式：

**方法 1: 使用 SecurityContextHolder**
```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
if (authentication != null && authentication.isAuthenticated()) {
    String username = authentication.getName();
    Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();
}
```

**方法 2: 在控制器中使用 Principal 参数**
```java
@GetMapping("/profile")
public String profile(Principal principal) {
    String username = principal.getName();
    // ...
}
```

**方法 3: 使用 @AuthenticationPrincipal 注解**
```java
@GetMapping("/profile")
public String profile(@AuthenticationPrincipal UserDetails userDetails) {
    String username = userDetails.getUsername();
    // ...
}
```

**方法 4: 在 Thymeleaf 模板中**
```html
<div sec:authorize="isAuthenticated()">
    用户名: <span sec:authentication="name"></span>
</div>
```

**Q4: 如何实现"记住我"功能？**

A: Spring Security 提供了内置的"记住我"功能：

```java
http
    .rememberMe(rememberMe -> rememberMe
        .key("uniqueAndSecret")  // 密钥，必须唯一
        .tokenValiditySeconds(86400)  // Token 有效期（秒），这里设置为 1 天
        .userDetailsService(userDetailsService)  // 用户详情服务
    );
```

在登录表单中添加复选框：
```html
<input type="checkbox" name="remember-me"> 记住我
```

**Q5: 如何配置多个不同的安全规则？**

A: 使用多个 `requestMatchers()` 来配置不同的规则：

```java
http
    .authorizeHttpRequests(auth -> auth
        // 静态资源 - 所有人可访问
        .requestMatchers("/css/**", "/js/**", "/images/**").permitAll()
        
        // 公开页面 - 所有人可访问
        .requestMatchers("/", "/home", "/about", "/register").permitAll()
        
        // 管理员页面 - 需要 ADMIN 角色
        .requestMatchers("/admin/**").hasRole("ADMIN")
        
        // 用户页面 - 需要 USER 角色
        .requestMatchers("/user/**").hasRole("USER")
        
        // API 接口 - 需要特定权限
        .requestMatchers("/api/**").hasAuthority("API_ACCESS")
        
        // 其他所有请求 - 需要认证
        .anyRequest().authenticated()
    );
```

#### 2.8 思考题

1. **密码存储策略**:
   - 问题：为什么不能使用明文存储密码？
   - 提示：考虑数据库泄露、彩虹表攻击等安全风险

2. **会话管理**:
   - 问题：如何防止会话固定攻击？
   - 提示：研究 `sessionManagement()` 配置和会话迁移策略

3. **CSRF 防护**:
   - 问题：CSRF 是什么？Spring Security 如何防护 CSRF 攻击？
   - 提示：研究 CSRF Token 的生成和验证机制

4. **权限细粒度控制**:
   - 问题：如何实现方法级别的权限控制？
   - 提示：研究 `@PreAuthorize` 和 `@Secured` 注解

5. **自定义认证提供者**:
   - 问题：如果用户信息存储在数据库中，如何自定义认证逻辑？
   - 提示：实现 `UserDetailsService` 接口

---

### 知识点3: 核心架构概念

#### 3.1 过滤器链架构

Spring Security 的核心是基于 Servlet 过滤器链的安全机制。理解这个架构对于掌握 Spring Security 至关重要。

**3.1.1 Servlet 过滤器基础**

Servlet 过滤器是 Java Web 应用的标准组件，它可以在请求到达 Servlet 之前和响应返回给客户端之后进行拦截和处理。

```java
/**
 * Servlet 过滤器接口
 * 
 * 过滤器的生命周期：
 * 1. init(): 初始化，容器启动时调用
 * 2. doFilter(): 每次请求都会调用
 * 3. destroy(): 销毁，容器关闭时调用
 */
public interface Filter {
    default void init(FilterConfig filterConfig) throws ServletException {}
    
    void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException;
    
    default void destroy() {}
}
```

**Spring Security 的过滤器链示例：**

```
HTTP Request
    ↓
[FilterChainProxy]
    ↓
┌─────────────────────────────────────────────────┐
│  Security Filters (Spring Security)              │
│  ┌──────────────────────────────────────────┐   │
│  │ 1. ChannelProcessingFilter                │   │
│  │ 2. WebAsyncManagerIntegrationFilter       │   │
│  │ 3. SecurityContextPersistenceFilter       │   │
│  │ 4. HeaderWriterFilter                     │   │
│  │ 5. CorsFilter                             │   │
│  │ 6. CsrfFilter                             │   │
│  │ 7. LogoutFilter                           │   │
│  │ 8. UsernamePasswordAuthenticationFilter   │   │
│  │ 9. ConcurrentSessionFilter               │   │
│  │ 10. RequestCacheAwareFilter              │   │
│  │ 11. SecurityContextHolderAwareRequestFilter│  │
│  │ 12. AnonymousAuthenticationFilter        │   │
│  │ 13. SessionManagementFilter              │   │
│  │ 14. ExceptionTranslationFilter           │   │
│  │ 15. FilterSecurityInterceptor            │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
    ↓
[Application Filters (如果有)]
    ↓
[Servlet/Controller]
    ↓
HTTP Response
```

**3.1.2 FilterChainProxy 源码分析**

`FilterChainProxy` 是 Spring Security 过滤器链的核心类。

```java
package org.springframework.security.web;

/**
 * FilterChainProxy 是 Spring Security 的主入口点
 * 
 * 主要职责：
 * 1. 管理多个 SecurityFilterChain
 * 2. 根据请求选择合适的过滤器链
 * 3. 将请求委托给选中的过滤器链处理
 * 
 * 设计模式：
 * - 代理模式：代理实际的过滤器链
 * - 责任链模式：按顺序调用多个过滤器
 * - 策略模式：根据请求选择不同的过滤器链
 */
public class FilterChainProxy extends GenericFilterBean {
    
    /**
     * 安全过滤器链列表
     * 
     * Spring Security 允许配置多个 SecurityFilterChain，
     * 每个链可以处理不同的请求模式（如 /api/** 和 /**）
     */
    private final List<SecurityFilterChain> filterChains;
    
    /**
     * 过滤器链代理的虚拟过滤器链（用于包装实际的过滤器链）
     */
    private FilterChainValidator filterChainValidator = new NullFilterChainValidator();
    
    /**
     * HTTP 防火墙，用于防止恶意请求
     */
    private HttpFirewall httpFirewall = new StrictHttpFirewall();
    
    /**
     * 构造函数
     * @param filterChains 安全过滤器链列表
     */
    public FilterChainProxy(List<SecurityFilterChain> filterChains) {
        this.filterChains = filterChains;
    }

    /**
     * 核心方法：处理过滤请求
     * 
     * 执行流程：
     * 1. 检查请求是否有效（通过 HTTP 防火墙）
     * 2. 选择合适的 SecurityFilterChain
     * 3. 使用虚拟过滤器链执行实际的过滤器
     * 
     * @param request Servlet 请求
     * @param response Servlet 响应
     * @param chain 过滤器链
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
            FilterChain chain) throws IOException, ServletException {
        
        // 确保是 HTTP 请求/响应
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        
        // 通过 HTTP 防火墙检查请求
        if (this.httpFirewall != null) {
            httpRequest = this.httpFirewall.getValidRequest(httpRequest);
            httpResponse = this.httpFirewall.getValidResponse(httpResponse);
        }
        
        // 获取第一个匹配的 SecurityFilterChain
        List<SecurityFilterChain> chains = this.filterChains;
        for (SecurityFilterChain filterChain : chains) {
            if (filterChain.matches(httpRequest)) {
                // 使用虚拟过滤器链执行
                VirtualFilterChain virtualFilterChain = new VirtualFilterChain(
                    httpRequest, httpResponse, chain, filterChain
                );
                virtualFilterChain.doFilter(httpRequest, httpResponse);
                return;
            }
        }
        
        // 没有匹配的过滤器链，继续执行原始链
        if (this.filterChainValidator != null) {
            this.filterChainValidator.validate(chain);
        }
        chain.doFilter(httpRequest, httpResponse);
    }

    /**
     * 获取所有安全过滤器链
     * @return 安全过滤器链列表
     */
    public List<SecurityFilterChain> getFilterChains() {
        return this.filterChains;
    }

    /**
     * 设置 HTTP 防火墙
     * @param httpFirewall HTTP 防火墙
     */
    public void setHttpFirewall(HttpFirewall httpFirewall) {
        this.httpFirewall = httpFirewall;
    }

    /**
     * VirtualFilterChain 是一个内部类，实现了实际的过滤器链执行逻辑
     * 
     * 主要职责：
     * 1. 管理 SecurityFilterChain 中的所有过滤器
     * 2. 按顺序执行过滤器
     * 3. 在所有过滤器执行完毕后，继续执行原始的过滤器链
     */
    private static class VirtualFilterChain implements FilterChain {
        
        private final FilterChain originalChain;
        private final List<Filter> additionalFilters;
        private final HttpServletRequest request;
        private final HttpServletResponse response;
        
        /**
         * 当前过滤器位置的指针
         */
        private int currentPosition = 0;

        /**
         * 构造函数
         * 
         * @param request HTTP 请求
         * @param response HTTP 响应
         * @param originalChain 原始过滤器链
         * @param chain 安全过滤器链
         */
        private VirtualFilterChain(HttpServletRequest request, HttpServletResponse response,
                FilterChain originalChain, SecurityFilterChain chain) {
            this.request = request;
            this.response = response;
            this.originalChain = originalChain;
            this.additionalFilters = chain.getFilters();
        }

        /**
         * 执行过滤器链
         * 
         * @param request 请求
         * @param response 响应
         */
        @Override
        public void doFilter(ServletRequest request, ServletResponse response)
                throws IOException, ServletException {
            
            if (this.currentPosition == this.additionalFilters.size()) {
                // 所有安全过滤器执行完毕，继续执行原始链
                if (logger.isDebugEnabled()) {
                    logger.debug(LogMessage.of(() -> 
                        "Secured " + request.getRequestURL()));
                }
                this.originalChain.doFilter(request, response);
                return;
            }
            
            // 获取下一个过滤器
            this.currentPosition++;
            Filter nextFilter = this.additionalFilters.get(this.currentPosition - 1);
            
            if (logger.isTraceEnabled()) {
                logger.trace(LogMessage.of(() -> 
                    "Invoking filter (" + this.currentPosition + "/" + 
                    this.additionalFilters.size() + "): " + nextFilter));
            }
            
            // 执行过滤器
            if (nextFilter instanceof OncePerRequestFilter) {
                // OncePerRequestFilter 确保过滤器在每个请求中只执行一次
                doFilter((OncePerRequestFilter) nextFilter, request, response);
            } 
            else {
                // 普通过滤器
                nextFilter.doFilter(request, response, this);
            }
        }
        
        /**
         * 执行 OncePerRequestFilter
         * 
         * @param filter OncePerRequestFilter 实例
         * @param request 请求
         * @param response 响应
         */
        private void doFilter(OncePerRequestFilter filter, ServletRequest request, 
                ServletResponse response) throws IOException, ServletException {
            filter.doFilter(request, response, this);
        }
    }
}
```

**源码分析要点**:

1. **过滤器链选择**: `FilterChainProxy` 可以管理多个 `SecurityFilterChain`，根据请求的特征（如 URL 路径）选择合适的过滤器链。

2. **虚拟过滤器链**: 使用内部类 `VirtualFilterChain` 来管理实际的过滤器执行顺序，确保过滤器按正确顺序执行。

3. **HTTP 防火墙**: 使用 `HttpFirewall` 来防止恶意请求，如攻击者尝试通过构造特殊的请求来绕过安全检查。

4. **性能考虑**: 过滤器链的执行是按顺序进行的，每个过滤器可以决定是否继续执行链中的下一个过滤器。

**3.1.3 核心过滤器详解**

**1. SecurityContextPersistenceFilter（或 SecurityContextHolderFilter）**

```java
package org.springframework.security.web.context;

/**
 * SecurityContextPersistenceFilter
 * 
 * 主要职责：
 * 1. 在请求开始时，从存储库（如 Session）中加载 SecurityContext
 * 2. 在请求结束时，将 SecurityContext 保存回存储库
 * 3. 清理 SecurityContext（防止内存泄漏）
 * 
 * 注意：在 Spring Security 5.7+ 中，这个过滤器已被 SecurityContextHolderFilter 替代
 * 
 * @deprecated 使用 SecurityContextHolderFilter 代替
 */
@Deprecated
public class SecurityContextPersistenceFilter extends OncePerRequestFilter {
    
    /**
     * SecurityContext 仓库
     * 
     * 负责从不同来源（如 HTTP Session）加载和保存 SecurityContext
     */
    private final SecurityContextRepository repo;
    
    /**
     * 在请求结束时是否清除 SecurityContext
     * 
     * 默认为 true，防止内存泄漏
     */
    private boolean clearOnLogout = true;

    /**
     * 构造函数
     * @param repo SecurityContext 仓库
     */
    public SecurityContextPersistenceFilter(SecurityContextRepository repo) {
        this.repo = repo;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        
        // 1. 在请求开始时，从仓库加载 SecurityContext
        //    如果仓库中没有（如首次访问），则创建一个空的 SecurityContext
        SecurityContext contextBeforeChainExecution = this.repo.loadContext(
            new HttpRequestResponseHolder(request, response)
        );
        
        try {
            // 2. 将加载的 SecurityContext 设置到 SecurityContextHolder 中
            //    这样后续的过滤器就可以访问当前用户的安全信息
            SecurityContextHolder.setContext(contextBeforeChainExecution);
            
            // 3. 继续执行过滤器链
            filterChain.doFilter(request, response);
        } 
        finally {
            // 4. 在请求结束时，获取可能被修改的 SecurityContext
            SecurityContext contextAfterChainExecution = SecurityContextHolder.getContext();
            
            // 5. 清除 SecurityContextHolder（防止内存泄漏）
            SecurityContextHolder.clearContext();
            
            // 6. 将 SecurityContext 保存回仓库
            //    如果用户已认证，保存到 Session
            //    如果用户已登出，从 Session 中移除
            this.repo.saveContext(contextAfterChainExecution, request, response);
        }
    }
}
```

**源码分析要点**:

1. **请求前的操作**: 从 `SecurityContextRepository`（如 HTTP Session）加载 `SecurityContext`，并将其设置到 `SecurityContextHolder` 中。

2. **请求后的操作**: 在请求处理完成后，将可能被修改的 `SecurityContext` 保存回仓库，并清除 `SecurityContextHolder`。

3. **防止内存泄漏**: 使用 `try-finally` 块确保 `SecurityContextHolder` 一定会被清除。

4. **会话管理**: 这个过滤器是 Spring Security 会话管理机制的核心，它负责在请求之间保持用户的认证状态。

**2. UsernamePasswordAuthenticationFilter**

```java
package org.springframework.security.web.authentication;

/**
 * UsernamePasswordAuthenticationFilter
 * 
 * 主要职责：
 * 1. 拦截登录表单提交的请求（默认是 POST /login）
 * 2. 从请求中提取用户名和密码
 * 3. 创建 Authentication 对象
 * 4. 委托给 AuthenticationManager 进行认证
 * 5. 认证成功后，将 Authentication 对象设置到 SecurityContext
 * 6. 认证失败后，处理失败情况
 * 
 * 默认配置：
 * - 登录处理 URL: /login (POST)
 * - 用户名参数: username
 * - 密码参数: password
 */
public class UsernamePasswordAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    
    /**
     * 用户名参数名
     */
    private String usernameParameter = "username";
    
    /**
     * 密码参数名
     */
    private String passwordParameter = "password";
    
    /**
     * 是否只支持 POST 方法
     */
    private boolean postOnly = true;

    /**
     * 构造函数
     * 
     * 默认只处理 POST /login 请求
     */
    public UsernamePasswordAuthenticationFilter() {
        super(new AntPathRequestMatcher("/login", "POST"));
    }

    /**
     * 核心方法：执行认证
     * 
     * @param request HTTP 请求
     * @return Authentication 认证对象
     * @throws AuthenticationException 认证异常
     */
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, 
            HttpServletResponse response) throws AuthenticationException {
        
        // 1. 检查是否只支持 POST 方法
        if (this.postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                "Authentication method not supported: " + request.getMethod());
        }
        
        // 2. 从请求中提取用户名和密码
        String username = obtainUsername(request);
        username = (username != null) ? username.trim() : "";
        String password = obtainPassword(request);
        password = (password != null) ? password : "";
        
        // 3. 创建未认证的 Authentication 对象
        UsernamePasswordAuthenticationToken authRequest = 
            new UsernamePasswordAuthenticationToken(username, password);
        
        // 4. 设置认证详情（如 IP 地址、会话 ID 等）
        setDetails(request, authRequest);
        
        // 5. 委托给 AuthenticationManager 进行认证
        //    AuthenticationManager 会遍历所有的 AuthenticationProvider，
        //    直到找到能够处理该认证请求的 Provider
        return this.getAuthenticationManager().authenticate(authRequest);
    }

    /**
     * 从请求中获取用户名
     * @param request HTTP 请求
     * @return 用户名
     */
    @Nullable
    protected String obtainUsername(HttpServletRequest request) {
        return request.getParameter(this.usernameParameter);
    }

    /**
     * 从请求中获取密码
     * @param request HTTP 请求
     * @return 密码
     */
    @Nullable
    protected String obtainPassword(HttpServletRequest request) {
        return request.getParameter(this.passwordParameter);
    }

    /**
     * 设置认证详情
     * 
     * @param request HTTP 请求
     * @param authRequest Authentication 对象
     */
    protected void setDetails(HttpServletRequest request, 
            UsernamePasswordAuthenticationToken authRequest) {
        authRequest.setDetails(this.authenticationDetailsSource.buildDetails(request));
    }

    /**
     * 设置用户名参数名
     * @param usernameParameter 用户名参数名
     */
    public void setUsernameParameter(String usernameParameter) {
        this.usernameParameter = usernameParameter;
    }

    /**
     * 设置密码参数名
     * @param passwordParameter 密码参数名
     */
    public void setPasswordParameter(String passwordParameter) {
        this.passwordParameter = passwordParameter;
    }

    /**
     * 设置是否只支持 POST 方法
     * @param postOnly 是否只支持 POST
     */
    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }
}
```

**源码分析要点**:

1. **认证流程**: 这个过滤器是表单登录的核心，它负责提取用户凭据、创建认证请求、委托给 `AuthenticationManager` 进行认证。

2. **灵活的配置**: 可以自定义用户名和密码的参数名，默认是 `username` 和 `password`。

3. **只支持 POST**: 默认情况下，登录请求必须是 POST 方法，这是出于安全考虑（防止 CSRF 攻击）。

4. **认证详情**: 使用 `AuthenticationDetailsSource` 来构建额外的认证详情，如 IP 地址、会话 ID 等。

**3. ExceptionTranslationFilter**

```java
package org.springframework.security.web.access;

/**
 * ExceptionTranslationFilter
 * 
 * 主要职责：
 * 1. 捕获过滤器链中抛出的安全异常
 * 2. 根据异常类型进行不同的处理
 * 3. 认证异常（AuthenticationException）→ 重定向到登录页面
 * 4. 访问拒绝异常（AccessDeniedException）→ 返回 403 错误
 * 
 * 这个过滤器是异常处理的核心，确保安全异常能够被正确处理
 */
public class ExceptionTranslationFilter extends GenericFilterBean {
    
    /**
     * 认证入口点
     * 
     * 处理认证异常，通常重定向到登录页面
     */
    private final AuthenticationEntryPoint authenticationEntryPoint;
    
    /**
     * 访问拒绝处理器
     * 
     * 处理访问拒绝异常，通常返回 403 错误
     */
    private final AccessDeniedHandler accessDeniedHandler;
    
    /**
     * 请求缓存
     * 
     * 保存用户在认证前尝试访问的请求，认证成功后可以重定向回原请求
     */
    private final RequestCache requestCache;

    /**
     * 构造函数
     * 
     * @param authenticationEntryPoint 认证入口点
     * @param requestCache 请求缓存
     */
    public ExceptionTranslationFilter(AuthenticationEntryPoint authenticationEntryPoint, 
            RequestCache requestCache) {
        this.authenticationEntryPoint = authenticationEntryPoint;
        this.requestCache = requestCache;
        this.accessDeniedHandler = new AccessDeniedHandlerImpl();
    }

    /**
     * 核心方法：处理过滤请求
     * 
     * @param request 请求
     * @param response 响应
     * @param chain 过滤器链
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
            FilterChain chain) throws IOException, ServletException {
        
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        
        try {
            // 1. 尝试执行过滤器链
            chain.doFilter(request, response);
        } 
        catch (IOException ex) {
            throw ex;
        } 
        catch (Exception ex) {
            // 2. 捕获异常并分析类型
            Throwable[] causeChain = this.throwableAnalyzer.determineCauseChain(ex);
            RuntimeException securityException = 
                (RuntimeException) this.throwableAnalyzer.getFirstThrowableOfType(
                    RuntimeException.class, causeChain);
            
            if (securityException == null) {
                rethrow(ex);
            }
            
            // 3. 处理安全异常
            handleException(httpRequest, httpResponse, chain, securityException);
        }
    }

    /**
     * 处理安全异常
     * 
     * @param request 请求
     * @param response 响应
     * @param chain 过滤器链
     * @param exception 安全异常
     */
    private void handleException(HttpServletRequest request, 
            HttpServletResponse response, FilterChain chain, 
            RuntimeException exception) throws IOException, ServletException {
        
        if (exception instanceof AuthenticationException) {
            // 4. 处理认证异常
            //    通常重定向到登录页面
            handleAuthenticationException((AuthenticationException) exception, 
                request, response);
        } 
        else if (exception instanceof AccessDeniedException) {
            // 5. 处理访问拒绝异常
            //    通常返回 403 错误
            handleAccessDeniedException((AccessDeniedException) exception, 
                request, response);
        } 
        else {
            // 6. 其他异常，继续抛出
            throw exception;
        }
    }

    /**
     * 处理认证异常
     * 
     * @param exception 认证异常
     * @param request 请求
     * @param response 响应
     */
    private void handleAuthenticationException(AuthenticationException exception, 
            HttpServletRequest request, HttpServletResponse response) 
            throws IOException, ServletException {
        
        // 1. 保存当前请求到缓存（认证成功后可以重定向回来）
        this.requestCache.saveRequest(request, response);
        
        // 2. 委托给认证入口点处理
        //    默认实现是重定向到登录页面
        this.authenticationEntryPoint.commence(request, response, exception);
    }

    /**
     * 处理访问拒绝异常
     * 
     * @param exception 访问拒绝异常
     * @param request 请求
     * @param response 响应
     */
    private void handleAccessDeniedException(AccessDeniedException exception, 
            HttpServletRequest request, HttpServletResponse response) 
            throws IOException, ServletException {
        
        // 委托给访问拒绝处理器
        //    默认实现是返回 403 错误
        this.accessDeniedHandler.handle(request, response, exception);
    }
}
```

**源码分析要点**:

1. **异常捕获**: 使用 try-catch 块捕获过滤器链中抛出的所有异常，并分析异常类型。

2. **两种异常类型**:
   - **AuthenticationException（认证异常）**: 用户未认证或认证失败，重定向到登录页面
   - **AccessDeniedException（访问拒绝异常）**: 用户已认证但权限不足，返回 403 错误

3. **请求缓存**: 当用户尝试访问受保护的资源时，如果未认证，会先保存当前请求到缓存，认证成功后可以重定向回原请求。

4. **委托模式**: 将异常的实际处理委托给 `AuthenticationEntryPoint` 和 `AccessDeniedHandler`，这使得异常处理策略可以灵活配置。

**4. FilterSecurityInterceptor**

```java
package org.springframework.security.web.access.intercept;

/**
 * FilterSecurityInterceptor
 * 
 * 主要职责：
 * 1. 是过滤器链中最后一个安全过滤器
 * 2. 在请求到达实际的 Servlet/Controller 之前进行最终的访问决策
 * 3. 使用 SecurityMetadataSource 获取请求所需的权限
 * 4. 使用 AccessDecisionManager 进行访问决策
 * 5. 如果决策通过，继续执行过滤器链；否则抛出 AccessDeniedException
 * 
 * 这个过滤器是授权的核心，决定了用户是否有权访问特定的资源
 */
public class FilterSecurityInterceptor extends AbstractSecurityInterceptor 
        implements Filter {
    
    /**
     * 安全元数据源
     * 
     * 负责从配置中获取请求所需的权限
     * 例如：/admin/** 需要 ROLE_ADMIN 权限
     */
    private final FilterInvocationSecurityMetadataSource securityMetadataSource;
    
    /**
     * 是否观察一旦发布后（观察者模式）
     */
    private boolean observeOncePerRequest = true;

    /**
     * 构造函数
     * @param securityMetadataSource 安全元数据源
     */
    public FilterSecurityInterceptor(FilterInvocationSecurityMetadataSource 
            securityMetadataSource) {
        this.securityMetadataSource = securityMetadataSource;
    }

    /**
     * 核心方法：执行过滤
     * 
     * @param request 请求
     * @param response 响应
     * @param chain 过滤器链
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
            FilterChain chain) throws IOException, ServletException {
        
        // 1. 创建 FilterInvocation 对象，封装请求、响应和链
        FilterInvocation fi = new FilterInvocation(request, response, chain);
        
        // 2. 执行访问决策
        invoke(fi);
    }

    /**
     * 执行访问决策
     * 
     * @param fi FilterInvocation 对象
     * @throws IOException IO 异常
     * @throws ServletException Servlet 异常
     */
    public void invoke(FilterInvocation fi) throws IOException, ServletException {
        
        if ((fi.getRequest() != null) 
                && ((fi.getRequest().getAttribute(
                    FilterChainProxy.APPLIED_FILTER) == null) 
                    || this.observeOncePerRequest)) {
            
            // 1. 标记过滤器已应用（防止重复执行）
            fi.getRequest().setAttribute(FilterChainProxy.APPLIED_FILTER, Boolean.TRUE);
            
            // 2. 执行访问决策前的逻辑
            InterceptorStatusToken token = super.beforeInvocation(fi);
            
            try {
                // 3. 继续执行过滤器链（最终到达实际的 Servlet/Controller）
                fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
            } 
            finally {
                // 4. 执行访问决策后的逻辑（通常是清理）
                super.finallyInvocation(token);
            }
            
            // 5. 完成访问决策
            super.afterInvocation(token, null);
        } 
        else {
            // 过滤器已经应用过，直接执行链
            fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
        }
    }

    /**
     * 获取安全元数据源
     * @return 安全元数据源
     */
    @Override
    public SecurityMetadataSource obtainSecurityMetadataSource() {
        return this.securityMetadataSource;
    }

    /**
     * 获取类对象
     * @return Class 对象
     */
    @Override
    public Class<?> getSecureObjectClass() {
        return FilterInvocation.class;
    }
}
```

**源码分析要点**:

1. **最终访问决策**: 这个过滤器在过滤器链的最后执行，在请求到达实际的 Servlet/Controller 之前进行最终的访问决策。

2. **访问决策流程**:
   - 从 `SecurityMetadataSource` 获取请求所需的权限
   - 调用 `AccessDecisionManager` 进行访问决策
   - 如果决策通过，继续执行过滤器链
   - 如果决策失败，抛出 `AccessDeniedException`，由 `ExceptionTranslationFilter` 处理

3. **防止重复执行**: 使用 `APPLIED_FILTER` 标记来防止过滤器在同一个请求中被重复执行。

4. **前后置处理**: 使用 `beforeInvocation()` 和 `afterInvocation()` 方法来在访问决策前后执行额外的逻辑（如日志记录、审计等）。

#### 3.2 认证流程详解

Spring Security 的认证流程是一个复杂但优雅的过程，涉及多个核心组件。

**3.2.1 认证流程图**

```
用户提交登录表单
       ↓
UsernamePasswordAuthenticationFilter
       ↓
   提取用户名和密码
       ↓
   创建 Authentication 对象（未认证状态）
       ↓
   调用 AuthenticationManager.authenticate()
       ↓
   遍历 AuthenticationProvider 列表
       ↓
   找到支持该认证类型的 Provider
       ↓
   Provider 调用 UserDetailsService.loadUserByUsername()
       ↓
   从数据库/内存/LDAP 等加载用户信息
       ↓
   Provider 使用 PasswordEncoder 验证密码
       ↓
   密码匹配？
   ├─ 是 → 创建已认证的 Authentication 对象
   │         设置权限和用户详情
   │         清除密码（安全）
   │         返回给 Filter
   └─ 否 → 抛出 BadCredentialsException
             ↓
       ExceptionTranslationFilter 捕获
             ↓
       重定向到登录页面（带错误信息）
```

**3.2.2 AuthenticationManager 源码分析**

```java
package org.springframework.security.authentication;

/**
 * AuthenticationManager 接口
 * 
 * 主要职责：
 * 1. 处理认证请求
 * 2. 遍历 AuthenticationProvider 列表
 * 3. 将认证请求委托给合适的 Provider
 * 
 * 设计模式：
 * - 策略模式：不同的 Provider 处理不同类型的认证
 * - 责任链模式：遍历 Provider 列表，直到找到能处理的 Provider
 * 
 * 常见实现类：
 * - ProviderManager: 默认实现，维护 Provider 列表
 */
public interface AuthenticationManager {

    /**
     * 认证方法
     * 
     * @param authentication 认证请求（包含用户凭据）
     * @return 已认证的 Authentication 对象
     * @throws AuthenticationException 认证失败
     */
    Authentication authenticate(Authentication authentication) 
            throws AuthenticationException;
}
```

**ProviderManager 源码分析**:

```java
package org.springframework.security.authentication;

/**
 * ProviderManager
 * 
 * AuthenticationManager 的默认实现，维护 AuthenticationProvider 列表
 * 
 * 主要职责：
 * 1. 管理多个 AuthenticationProvider
 * 2. 遍历 Provider 列表，找到能处理当前认证请求的 Provider
 * 3. 处理认证结果，包括成功、失败和忽略的情况
 * 4. 管理父级 AuthenticationManager（可选）
 * 
 * 灵活性：
 * - 可以配置多个 Provider 来支持不同的认证方式
 * - 可以配置父级 AuthenticationManager 来提供备用认证逻辑
 */
public class ProviderManager implements AuthenticationManager, MessageSourceAware, InitializingBean {

    /**
     * 认证提供者列表
     * 
     * 这些 Provider 按顺序遍历，直到找到能处理当前认证请求的 Provider
     * 
     * 常见的 Provider：
     * - DaoAuthenticationProvider: 基于数据库的用户名密码认证
     * - AnonymousAuthenticationProvider: 匿名认证
     * - RememberMeAuthenticationProvider: 记住我功能认证
     * - LdapAuthenticationProvider: LDAP 认证
     */
    private List<AuthenticationProvider> providers = Collections.emptyList();
    
    /**
     * 父级 AuthenticationManager
     * 
     * 如果所有的 Provider 都无法处理认证请求，会委托给父级 AuthenticationManager
     * 
     * 使用场景：
     * - 提供默认的认证逻辑
     * - 实现认证的层级管理
     */
    private AuthenticationManager parent;
    
    /**
     * 当所有 Provider 都忽略认证请求时，是否清除凭据
     * 
     * 默认为 false
     */
    private boolean eraseCredentialsAfterAuthentication = true;

    /**
     * 核心方法：执行认证
     * 
     * @param authentication 认证请求
     * @return 已认证的 Authentication 对象
     * @throws AuthenticationException 认证失败
     */
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        Class<? extends Authentication> toTest = authentication.getClass();
        AuthenticationException lastException = null;
        AuthenticationException parentException = null;
        Authentication result = null;
        
        // 1. 遍历所有的 AuthenticationProvider
        for (AuthenticationProvider provider : getProviders()) {
            // 2. 检查 Provider 是否支持当前认证类型
            if (!provider.supports(toTest)) {
                continue;
            }
            
            if (logger.isTraceEnabled()) {
                logger.trace(LogMessage.format("Authenticating request with %s (%d/%d)",
                    provider.getClass().getSimpleName(), 
                    ++currentPosition, 
                    providers.size()));
            }
            
            try {
                // 3. 委托给 Provider 进行认证
                result = provider.authenticate(authentication);
                
                if (result != null) {
                    // 4. 认证成功
                    copyDetails(authentication, result);
                    break;
                }
            } 
            catch (AccountStatusException | InternalAuthenticationServiceException ex) {
                // 账户状态异常（如账户被锁定、过期等）
                prepareException(ex, authentication);
                throw ex;
            } 
            catch (AuthenticationException ex) {
                // 认证失败，记录异常，继续尝试下一个 Provider
                lastException = ex;
            }
        }
        
        // 5. 如果所有的 Provider 都无法处理（都返回 null），尝试父级 AuthenticationManager
        if (result == null && this.parent != null) {
            try {
                // 6. 委托给父级 AuthenticationManager
                result = this.parent.authenticate(authentication);
            } 
            catch (ProviderNotFoundException ex) {
                // 父级也无法处理
            } 
            catch (AuthenticationException ex) {
                // 父级认证失败
                parentException = ex;
                lastException = ex;
            }
        }
        
        // 7. 处理认证结果
        if (result != null) {
            // 认证成功
            if (this.eraseCredentialsAfterAuthentication 
                    && (result instanceof CredentialsContainer)) {
                // 清除凭据（安全考虑）
                ((CredentialsContainer) result).eraseCredentials();
            }
            
            // 发布认证成功事件
            eventPublisher.publishAuthenticationSuccess(result);
            
            return result;
        }
        
        // 8. 认证失败，抛出异常
        if (lastException == null) {
            // 没有任何 Provider 支持该认证类型
            throw new ProviderNotFoundException(
                "No AuthenticationProvider found for " + toTest.getName());
        }
        
        // 准备异常
        prepareException(lastException, authentication);
        
        // 抛出异常
        throw lastException;
    }
}
```

**源码分析要点**:

1. **责任链模式**: `ProviderManager` 遍历 `AuthenticationProvider` 列表，直到找到能处理当前认证请求的 Provider。

2. **灵活的认证方式**: 通过配置多个 Provider，可以支持多种认证方式（用户名密码、LDAP、OAuth 等）。

3. **父级 AuthenticationManager**: 如果所有的 Provider 都无法处理认证请求，会委托给父级 AuthenticationManager，这提供了额外的灵活性。

4. **清除凭据**: 认证成功后，会清除 Authentication 对象中的敏感凭据（如密码），这是出于安全考虑。

5. **事件发布**: 认证成功后，会发布认证成功事件，这使得应用程序可以监听认证事件并进行额外的处理（如日志记录、审计等）。

**3.2.3 AuthenticationProvider 源码分析**

```java
package org.springframework.security.authentication;

/**
 * AuthenticationProvider 接口
 * 
 * 主要职责：
 * 1. 处理特定类型的认证请求
 * 2. 验证用户凭据
 * 3. 返回已认证的 Authentication 对象
 * 
 * 设计模式：
 * - 策略模式：不同的 Provider 实现不同的认证策略
 * 
 * 常见实现类：
 * - DaoAuthenticationProvider: 基于数据库的认证
 * - LdapAuthenticationProvider: 基于 LDAP 的认证
 * - AnonymousAuthenticationProvider: 匿名认证
 * - RememberMeAuthenticationProvider: 记住我认证
 */
public interface AuthenticationProvider {

    /**
     * 认证方法
     * 
     * @param authentication 认证请求
     * @return 已认证的 Authentication 对象，如果返回 null 表示该 Provider 不支持此认证类型
     * @throws AuthenticationException 认证失败
     */
    Authentication authenticate(Authentication authentication) 
            throws AuthenticationException;

    /**
     * 检查是否支持该认证类型
     * 
     * @param authentication 认证类型
     * @return 如果支持返回 true，否则返回 false
     */
    boolean supports(Class<?> authentication);
}
```

**DaoAuthenticationProvider 源码分析**:

```java
package org.springframework.security.authentication.dao;

/**
 * DaoAuthenticationProvider
 * 
 * 基于 DAO 的认证提供者，是用户名密码认证的核心实现
 * 
 * 主要职责：
 * 1. 从 UserDetailsService 加载用户信息
 * 2. 使用 PasswordEncoder 验证密码
 * 3. 创建已认证的 Authentication 对象
 * 
 * 特点：
 * - 支持密码编码
 * - 支持用户状态检查（账户是否过期、锁定等）
 * - 支持隐藏的用户不存在异常（防止用户枚举攻击）
 */
public class DaoAuthenticationProvider extends AbstractUserDetailsAuthenticationProvider {

    /**
     * 密码编码器
     * 
     * 用于验证密码是否正确
     * 默认是 BCryptPasswordEncoder
     */
    private PasswordEncoder passwordEncoder;
    
    /**
     * 用户详情服务
     * 
     * 用于从数据源（如数据库）加载用户信息
     */
    private UserDetailsService userDetailsService;
    
    /**
     * 用户缓存（可选）
     * 
     * 用于缓存用户信息，避免频繁查询数据库
     */
    private UserCache userCache = new NullUserCache();

    /**
     * 验证密码的核心方法
     * 
     * @param userDetails 用户详情
     * @param authentication 认证请求
     * @throws AuthenticationException 验证失败
     */
    @Override
    protected void additionalAuthenticationChecks(UserDetails userDetails, 
            UsernamePasswordAuthenticationToken authentication) 
            throws AuthenticationException {
        
        // 1. 提取用户提交的密码
        Object presentedPassword = authentication.getCredentials();
        
        if (presentedPassword == null) {
            // 2. 密码为空，抛出异常
            throw new BadCredentialsException("Bad credentials");
        }
        
        // 3. 使用密码编码器验证密码
        if (!this.passwordEncoder.matches(presentedPassword.toString(), 
                userDetails.getPassword())) {
            // 4. 密码不匹配，抛出异常
            throw new BadCredentialsException("Bad credentials");
        }
    }

    /**
     * 核心方法：执行认证
     * 
     * @param authentication 认证请求
     * @return 已认证的 Authentication 对象
     * @throws AuthenticationException 认证失败
     */
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        // 1. 确定用户名
        String username = (authentication.getPrincipal() == null) ? "NONE_PROVIDED" 
            : authentication.getName();
        
        // 2. 尝试从缓存加载用户
        UserDetails user = this.userCache.getUserFromCache(username);
        
        if (user == null) {
            // 3. 缓存中没有，从 UserDetailsService 加载
            try {
                user = this.retrieveUser(username, 
                    (UsernamePasswordAuthenticationToken) authentication);
            } 
            catch (UsernameNotFoundException notFound) {
                // 4. 根据配置决定是否隐藏用户不存在异常
                if (this.hideUserNotFoundExceptions) {
                    throw new BadCredentialsException(
                        this.messages.getMessage("AbstractUserDetailsAuthenticationProvider.badCredentials", 
                        "Bad credentials"));
                }
                throw notFound;
            }
            
            // 5. 将用户信息缓存
            this.userCache.putUserInCache(user);
        }
        
        try {
            // 6. 前置检查（账户状态检查）
            this.preAuthenticationChecks.check(user);
            
            // 7. 额外的认证检查（密码验证）
            this.additionalAuthenticationChecks(user, 
                (UsernamePasswordAuthenticationToken) authentication);
        } 
        catch (AuthenticationException exception) {
            // 8. 认证失败，清除缓存
            this.userCache.removeUserFromCache(username);
            throw exception;
        }
        
        // 9. 后置检查（用户详情检查）
        this.postAuthenticationChecks.check(user);
        
        // 10. 创建已认证的 Authentication 对象
        return createSuccessAuthentication(
            authentication, user, user.getAuthorities());
    }

    /**
     * 从 UserDetailsService 加载用户信息
     * 
     * @param username 用户名
     * @param authentication 认证请求
     * @return 用户详情
     * @throws AuthenticationException 认证异常
     */
    @Override
    protected final UserDetails retrieveUser(String username, 
            UsernamePasswordAuthenticationToken authentication) 
            throws AuthenticationException {
        
        try {
            // 委托给 UserDetailsService 加载用户
            UserDetails loadedUser = this.userDetailsService.loadUserByUsername(username);
            
            if (loadedUser == null) {
                throw new InternalAuthenticationServiceException(
                    "UserDetailsService returned null, which is an interface contract violation");
            }
            
            return loadedUser;
        } 
        catch (UsernameNotFoundException ex) {
            // 重新抛出异常
            throw ex;
        } 
        catch (Exception ex) {
            throw new InternalAuthenticationServiceException(ex.getMessage(), ex);
        }
    }
}
```

**源码分析要点**:

1. **密码验证**: 使用 `PasswordEncoder` 来验证密码，而不是直接比较明文密码。

2. **用户缓存**: 使用 `UserCache` 来缓存用户信息，避免频繁查询数据库，提高性能。

3. **用户状态检查**: 在认证之前和之后，都会检查用户的状态（如账户是否过期、是否锁定等）。

4. **隐藏用户不存在异常**: 默认情况下，会隐藏用户不存在异常（统一返回"Bad credentials"），这是为了防止用户枚举攻击。

5. **认证流程**:
   - 从 `UserDetailsService` 加载用户信息
   - 检查用户状态
   - 验证密码
   - 创建已认证的 `Authentication` 对象

#### 3.3 常见问题（FAQ）

**Q1: 如何调试 Spring Security 的过滤器链？**

A: 有多种方法可以调试过滤器链：

**方法 1: 启用 DEBUG 日志**
```yaml
logging:
  level:
    org.springframework.security: DEBUG
```

**方法 2: 使用 Spring Security 的 debug 模式**
```java
@Configuration
@EnableWebSecurity(debug = true)  // 启用调试模式
public class SecurityConfig {
    // ...
}
```

**方法 3: 打印过滤器链**
```java
@Autowired
public void printFilterChain(FilterChainProxy filterChainProxy) {
    List<SecurityFilterChain> filterChains = filterChainProxy.getFilterChains();
    for (SecurityFilterChain chain : filterChains) {
        List<Filter> filters = chain.getFilters();
        System.out.println("Filter Chain:");
        for (Filter filter : filters) {
            System.out.println("  - " + filter.getClass().getName());
        }
    }
}
```

**Q2: 为什么我的自定义过滤器没有被执行？**

A: 常见原因和解决方法：

1. **过滤器顺序问题**: 
   - 确保你的过滤器在正确的位置
   - 使用 `@Order` 注解或在 `HttpSecurity` 中使用 `addFilterBefore()` 或 `addFilterAfter()`

```java
http.addFilterBefore(myCustomFilter, UsernamePasswordAuthenticationFilter.class);
```

2. **URL 匹配问题**:
   - 检查你的过滤器是否只处理特定的 URL
   - 确保你的请求 URL 与过滤器处理的 URL 匹配

3. **过滤器没有注册**:
   - 确保你的过滤器被声明为 Bean 或添加到过滤器链中

```java
@Bean
public FilterRegistrationBean<MyCustomFilter> myCustomFilterRegistration() {
    FilterRegistrationBean<MyCustomFilter> registration = new FilterRegistrationBean<>();
    registration.setFilter(new MyCustomFilter());
    registration.addUrlPatterns("/*");
    return registration;
}
```

**Q3: ExceptionTranslationFilter 是如何工作的？**

A: `ExceptionTranslationFilter` 是 Spring Security 异常处理的核心：

**工作原理**:
1. 捕获过滤器链中抛出的安全异常
2. 根据异常类型进行不同的处理：
   - `AuthenticationException`: 重定向到登录页面
   - `AccessDeniedException`: 返回 403 错误
3. 委托给 `AuthenticationEntryPoint` 和 `AccessDeniedHandler` 进行实际处理

**自定义异常处理**:
```java
http
    .exceptionHandling(exceptions -> exceptions
        .authenticationEntryPoint((request, response, authException) -> {
            // 自定义认证异常处理
            response.sendRedirect("/custom-login?error=" + authException.getMessage());
        })
        .accessDeniedHandler((request, response, accessDeniedException) -> {
            // 自定义访问拒绝处理
            response.sendError(403, "Access Denied: " + accessDeniedException.getMessage());
        })
    );
```

**Q4: 什么是 CSRF？Spring Security 如何防护 CSRF 攻击？**

A: CSRF（Cross-Site Request Forgery，跨站请求伪造）是一种常见的 Web 攻击。

**攻击原理**:
1. 用户登录了银行网站 `bank.com`
2. 用户访问了恶意网站 `evil.com`
3. 恶意网站包含一个链接或表单，向 `bank.com` 发送请求
4. 浏览器自动携带 `bank.com` 的 Cookie，请求被执行

**Spring Security 的防护机制**:
1. 生成 CSRF Token
2. 将 Token 存储在 Session 中
3. 在表单中包含隐藏字段：`<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>`
4. 提交表单时验证 Token

**配置 CSRF**:
```java
http
    .csrf(csrf -> csrf
        .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())  // 使用 Cookie 存储 Token
        .ignoringRequestMatchers("/api/**")  // 某些端点可以忽略 CSRF 验证
    );
```

**在 Thymeleaf 中使用**:
```html
<form th:action="@{/login}" method="post">
    <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}"/>
    <!-- 其他表单字段 -->
    <button type="submit">登录</button>
</form>
```

**Q5: 如何实现方法级别的安全控制？**

A: Spring Security 支持方法级别的安全控制，主要有三种方式：

**方式 1: 使用 @Secured 注解**
```java
@Configuration
@EnableGlobalMethodSecurity(securedEnabled = true)
public class MethodSecurityConfig {
    // ...
}

@Service
public class UserService {
    @Secured("ROLE_ADMIN")
    public void adminMethod() {
        // 只有 ADMIN 角色可以调用
    }
    
    @Secured({"ROLE_ADMIN", "ROLE_USER"})
    public void userOrAdminMethod() {
        // ADMIN 或 USER 角色都可以调用
    }
}
```

**方式 2: 使用 @PreAuthorize 和 @PostAuthorize 注解**
```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig {
    // ...
}

@Service
public class UserService {
    @PreAuthorize("hasRole('ADMIN')")
    public void adminMethod() {
        // 方法执行前检查权限
    }
    
    @PreAuthorize("#userId == authentication.name")
    public void getUser(String userId) {
        // 只有当 userId 等于当前登录用户名时才能调用
    }
    
    @PostAuthorize("returnObject.owner == authentication.name")
    public Document getDocument(String docId) {
        // 方法执行后检查返回值的 owner 属性
        return documentRepository.findById(docId);
    }
}
```

**方式 3: 使用 JSR-250 注解**
```java
@Configuration
@EnableGlobalMethodSecurity(jsr250Enabled = true)
public class MethodSecurityConfig {
    // ...
}

@Service
public class UserService {
    @RolesAllowed("ROLE_ADMIN")
    public void adminMethod() {
        // 只有 ADMIN 角色可以调用
    }
    
    @PermitAll
    public void publicMethod() {
        // 所有人都可以调用
    }
    
    @DenyAll
    public void privateMethod() {
        // 没有人可以调用
    }
}
```

#### 3.4 思考题

1. **过滤器链设计**:
   - 问题：为什么 Spring Security 要使用过滤器链而不是单一的过滤器？
   - 提示：考虑单一职责原则、灵活性、可扩展性

2. **认证提供者选择**:
   - 问题：ProviderManager 如何选择合适的 AuthenticationProvider？
   - 提示：研究 `supports()` 方法的设计

3. **密码存储安全**:
   - 问题：为什么不能直接存储明文密码？
   - 提示：考虑数据库泄露、彩虹表攻击、加盐

4. **异常处理机制**:
   - 问题：ExceptionTranslationFilter 是如何区分认证异常和访问拒绝异常的？
   - 提示：研究异常类型和继承层次

5. **性能优化**:
   - 问题：在高并发场景下，如何优化 Spring Security 的性能？
   - 提示：考虑缓存、异步处理、数据库查询优化

---

### 知识点4: 配置方式详解

#### 4.1 Spring Security 配置方式演进

Spring Security 的配置方式随着版本的演进发生了多次变化：

**4.1.1 XML 配置方式（Spring Security 2.x/3.x）**

```xml
<!-- 早期的 XML 配置方式 -->
<http auto-config="true">
    <intercept-url pattern="/admin/**" access="ROLE_ADMIN" />
    <intercept-url pattern="/**" access="ROLE_USER" />
    <form-login />
    <logout />
</http>

<authentication-manager>
    <authentication-provider>
        <user-service>
            <user name="user" password="password" authorities="ROLE_USER" />
            <user name="admin" password="admin" authorities="ROLE_ADMIN" />
        </user-service>
    </authentication-provider>
</authentication-manager>
```

**优点**:
- 配置集中，易于管理
- 适合大型企业应用

**缺点**:
- 配置冗长，容易出错
- 类型不安全
- IDE 支持较弱

**4.1.2 Java 配置方式（Spring Security 3.2+）**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            .and()
            .formLogin();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth
            .inMemoryAuthentication()
                .withUser("user").password("password").roles("USER")
                .and()
                .withUser("admin").password("admin").roles("ADMIN");
    }
}
```

**优点**:
- 类型安全
- IDE 支持好
- 配置更简洁

**缺点**:
- 继承 `WebSecurityConfigurerAdapter`（现已过时）

**4.1.3 基于 Bean 的配置方式（Spring Security 5.7+，推荐）**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(withDefaults());
        
        return http.build();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
            .username("user")
            .password(passwordEncoder().encode("password"))
            .roles("USER")
            .build();
        
        UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder().encode("admin"))
            .roles("ADMIN")
            .build();
        
        return new InMemoryUserDetailsManager(user, admin);
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**优点**:
- 不继承任何类
- 更灵活，更符合 Spring Boot 的设计理念
- 支持多个 SecurityFilterChain

**4.1.4 配置方式对比**

| 特性 | XML 配置 | Java 配置（旧） | Bean 配置（新） |
|------|---------|----------------|----------------|
| **类型安全** | ❌ | ✅ | ✅ |
| **IDE 支持** | ⚠️ | ✅ | ✅ |
| **灵活性** | ⚠️ | ✅ | ✅✅ |
| **简洁性** | ❌ | ✅ | ✅✅ |
| **推荐程度** | ❌ 已过时 | ⚠️ 不推荐 | ✅ 推荐 |

#### 4.2 核心配置类详解

**4.2.1 @EnableWebSecurity 注解**

```java
package org.springframework.security.config.annotation.web.configuration;

/**
 * @EnableWebSecurity 注解
 * 
 * 主要作用：
 * 1. 导入 WebSecurityConfiguration 配置类
 * 2. 启用 @EnableGlobalAuthentication
 * 3. 启用 WebSecurityCustomizer Bean
 * 4. 添加 SpringSecurityFilterChain（DelegatingFilterProxy）
 * 
 * 源码分析：
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import({ WebSecurityConfiguration.class, 
         SpringWebMvcImportSelector.class, 
         OAuth2ImportSelector.class,
         HttpSecurityConfiguration.class })
@EnableGlobalAuthentication
public @interface EnableWebSecurity {

    /**
     * 是否启用调试模式
     * 
     * 调试模式会输出详细的安全日志，帮助排查问题
     * 
     * 注意：不要在生产环境启用！
     * 
     * @return 是否启用调试模式
     */
    boolean debug() default false;

    /**
     * 是否忽略请求（用于静态资源等）
     * 
     * @deprecated 使用 WebSecurityCustomizer 代替
     * @return 是否忽略请求
     */
    @Deprecated
    boolean ignore() default false;
}
```

**源码分析要点**:

1. **@Import 注解**: 导入了多个配置类，包括 `WebSecurityConfiguration`、`HttpSecurityConfiguration` 等。

2. **@EnableGlobalAuthentication**: 启用了全局认证配置，这是方法级别安全控制的基础。

3. **调试模式**: `debug()` 属性可以启用调试模式，输出详细的安全日志，但不要在生产环境使用。

**4.2.2 SecurityFilterChain Bean**

```java
package org.springframework.security.web;

/**
 * SecurityFilterChain 接口
 * 
 * 主要职责：
 * 1. 定义一个安全过滤器链
 * 2. 决定该链是否处理特定的请求
 * 3. 提供该链包含的所有过滤器
 * 
 * 设计模式：
 * - 责任链模式：按顺序执行多个过滤器
 * - 策略模式：不同的 SecurityFilterChain 可以处理不同的请求
 * 
 * 常见实现类：
 * - DefaultSecurityFilterChain: 默认实现
 */
public interface SecurityFilterChain {

    /**
     * 检查该过滤器链是否匹配当前的请求
     * 
     * @param request HTTP 请求
     * @return 如果匹配返回 true，否则返回 false
     */
    boolean matches(HttpServletRequest request);

    /**
     * 获取该过滤器链包含的所有过滤器
     * 
     * @return 过滤器列表
     */
    List<Filter> getFilters();
}
```

**创建 SecurityFilterChain Bean**:

```java
/**
 * 创建 SecurityFilterChain Bean
 * 
 * 这是推荐的方式，替代继承 WebSecurityConfigurerAdapter
 * 
 * @param http HttpSecurity 配置对象
 * @return SecurityFilterChain 实例
 * @throws Exception 配置异常
 */
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    // 配置 HTTP 安全
    http
        // 1. 授权配置
        .authorizeHttpRequests(authorize -> authorize
            // 静态资源 - 所有人可访问
            .requestMatchers("/css/**", "/js/**", "/images/**").permitAll()
            // 公开页面 - 所有人可访问
            .requestMatchers("/", "/home", "/register").permitAll()
            // 管理员页面 - 需要 ADMIN 角色
            .requestMatchers("/admin/**").hasRole("ADMIN")
            // 用户页面 - 需要 USER 角色
            .requestMatchers("/user/**").hasRole("USER")
            // 其他所有请求 - 需要认证
            .anyRequest().authenticated()
        )
        // 2. 表单登录配置
        .formLogin(form -> form
            .loginPage("/login")  // 自定义登录页面
            .loginProcessingUrl("/login")  // 登录处理 URL
            .defaultSuccessUrl("/", true)  // 登录成功后的默认页面
            .failureUrl("/login?error=true")  // 登录失败 URL
            .permitAll()
        )
        // 3. 登出配置
        .logout(logout -> logout
            .logoutUrl("/logout")  // 登出 URL
            .logoutSuccessUrl("/login?logout=true")  // 登出成功后跳转的页面
            .invalidateHttpSession(true)  // 使会话失效
            .clearAuthentication(true)  // 清除认证信息
            .permitAll()
        )
        // 4. HTTP Basic 认证
        .httpBasic(withDefaults())
        // 5. CSRF 配置
        .csrf(csrf -> csrf
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
            .ignoringRequestMatchers("/api/**")  // 某些端点可以忽略 CSRF
        )
        // 6. 会话管理
        .sessionManagement(session -> session
            .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)  // 按需创建会话
            .maximumSessions(1)  // 最大会话数
            .maxSessionsPreventsLogin(false)  // 不阻止新登录（踢出旧会话）
        )
        // 7. 异常处理
        .exceptionHandling(exceptions -> exceptions
            .authenticationEntryPoint((request, response, authException) -> {
                // 自定义认证异常处理
                response.sendRedirect("/custom-login");
            })
            .accessDeniedHandler((request, response, accessDeniedException) -> {
                // 自定义访问拒绝处理
                response.sendError(403, "Access Denied");
            })
        )
        // 8. 记住我功能
        .rememberMe(rememberMe -> rememberMe
            .key("uniqueAndSecret")  // 密钥
            .tokenValiditySeconds(86400)  // Token 有效期（秒）
            .userDetailsService(userDetailsService)  // 用户详情服务
        )
        // 9. 匿名用户
        .anonymous(anonymous -> anonymous
            .key("anonymousKey")  // 匿名用户的密钥
            .principal("anonymousUser")  // 匿名用户的标识
            .authorities(new SimpleGrantedAuthority("ROLE_ANONYMOUS"))  // 匿名用户的权限
        );

    return http.build();
}
```

**配置说明**:

1. **authorizeHttpRequests()**: 配置请求的授权规则
2. **formLogin()**: 配置表单登录
3. **logout()**: 配置登出
4. **httpBasic()**: 配置 HTTP Basic 认证
5. **csrf()**: 配置 CSRF 防护
6. **sessionManagement()**: 配置会话管理
7. **exceptionHandling()**: 配置异常处理
8. **rememberMe()**: 配置记住我功能
9. **anonymous()**: 配置匿名用户

**4.2.3 UserDetailsService Bean**

```java
/**
 * 创建 UserDetailsService Bean
 * 
 * UserDetailsService 是加载用户信息的核心接口
 * 
 * @param passwordEncoder 密码编码器
 * @return UserDetailsService 实例
 */
@Bean
public UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
    // 创建普通用户
    UserDetails user = User.builder()
        .username("user")
        .password(passwordEncoder.encode("123456"))
        .roles("USER")
        .accountLocked(false)  // 账户是否锁定
        .accountExpired(false)  // 账户是否过期
        .credentialsExpired(false)  // 凭证是否过期
        .disabled(false)  // 账户是否禁用
        .build();

    // 创建管理员用户
    UserDetails admin = User.builder()
        .username("admin")
        .password(passwordEncoder.encode("admin123"))
        .roles("ADMIN", "USER")
        .accountLocked(false)
        .accountExpired(false)
        .credentialsExpired(false)
        .disabled(false)
        .build();

    // 创建内存中的用户详情管理器
    return new InMemoryUserDetailsManager(user, admin);
}
```

**4.2.4 PasswordEncoder Bean**

```java
/**
 * 创建 PasswordEncoder Bean
 * 
 * 密码编码器用于加密和验证密码
 * 
 * Spring Security 提供了多种密码编码器：
 * - BCryptPasswordEncoder: 使用 BCrypt 算法（推荐）
 * - Pbkdf2PasswordEncoder: 使用 PBKDF2 算法
 * - SCryptPasswordEncoder: 使用 SCrypt 算法
 * - Argon2PasswordEncoder: 使用 Argon2 算法
 * - NoOpPasswordEncoder: 不加密（仅用于测试）
 * 
 * @return PasswordEncoder 实例
 */
@Bean
public PasswordEncoder passwordEncoder() {
    // 使用 BCrypt 算法，默认强度为 10
    return new BCryptPasswordEncoder(10);
    
    // 或者使用其他编码器：
    // return new Pbkdf2PasswordEncoder();
    // return new SCryptPasswordEncoder();
    // return new Argon2PasswordEncoder();
    // return NoOpPasswordEncoder.getInstance();  // 不推荐！
}
```

**BCryptPasswordEncoder 源码分析**:

```java
package org.springframework.security.crypto.bcrypt;

/**
 * BCryptPasswordEncoder
 * 
 * BCrypt 是一种自适应的哈希函数，基于 Blowfish 密码算法
 * 
 * 特点：
 * 1. 自动加盐：每次生成的哈希值都不同
 * 2. 自适应：可以调整计算强度（4-31）
 * 3. 安全：被广泛使用和验证
 * 
 * 默认强度：10（推荐范围：10-12）
 * 
 * 强度与时间的关系（近似值）：
 * - 4: 约 0.05 秒
 * - 10: 约 0.6 秒
 * - 12: 约 2.5 秒
 * - 14: 约 10 秒
 */
public class BCryptPasswordEncoder implements PasswordEncoder {

    /**
     * BCrypt 算法的强度（2 的幂次方）
     * 
     * 有效范围：4-31
     * 默认值：10
     */
    private final int strength;
    
    /**
     * BCrypt 实例
     */
    private final BCryptPasswordEncoder.BCryptVersion version;
    
    /**
     * 随机数生成器
     */
    private final SecureRandom random;

    /**
     * 构造函数（使用默认强度 10）
     */
    public BCryptPasswordEncoder() {
        this(-1);
    }

    /**
     * 构造函数（自定义强度）
     * 
     * @param strength BCrypt 强度（4-31）
     */
    public BCryptPasswordEncoder(int strength) {
        this(strength, null);
    }

    /**
     * 构造函数（自定义强度和随机数生成器）
     * 
     * @param strength BCrypt 强度
     * @param random 随机数生成器
     */
    public BCryptPasswordEncoder(int strength, SecureRandom random) {
        this(BCryptPasswordEncoder.BCryptVersion.$2A, strength, random);
    }

    /**
     * 核心方法：对原始密码进行编码
     * 
     * @param rawPassword 原始密码
     * @return 编码后的密码
     */
    @Override
    public String encode(CharSequence rawPassword) {
        if (rawPassword == null) {
            throw new IllegalArgumentException("rawPassword cannot be null");
        }
        
        // 1. 生成随机盐
        // 2. 使用 BCrypt 算法哈希密码
        // 3. 返回格式：$2a$[强度]$[22位盐][31位哈希值]
        String salt = this.version.getSalt();
        return BCrypt.hashpw(rawPassword.toString(), salt);
    }

    /**
     * 核心方法：验证密码
     * 
     * @param rawPassword 原始密码（用户输入的密码）
     * @param encodedPassword 编码后的密码（数据库中存储的密码）
     * @return 如果密码匹配返回 true，否则返回 false
     */
    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        if (rawPassword == null) {
            throw new IllegalArgumentException("rawPassword cannot be null");
        }
        
        if (encodedPassword == null || encodedPassword.length() == 0) {
            logger.warn("Empty encoded password");
            return false;
        }
        
        // 1. 检查编码后的密码格式
        if (!this.version.prefixMatches(encodedPassword)) {
            this.logger.warn("Encoded password does not look like BCrypt");
            return false;
        }
        
        try {
            // 2. 使用 BCrypt 算法验证密码
            //    BCrypt 会从 encodedPassword 中提取盐，然后哈希 rawPassword
            //    最后比较两个哈希值是否相等
            return BCrypt.checkpw(rawPassword.toString(), encodedPassword);
        } 
        catch (Exception ex) {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Failed to verify password", ex);
            }
            return false;
        }
    }

    /**
     * BCrypt 版本枚举
     * 
     * BCrypt 有多个版本：
     * - $2a$: 原始版本
     * - $2b$: 修复了某些字符编码问题
     * - $2y$: 类似于 $2b$
     */
    public enum BCryptVersion {
        $2A("$2a$"),
        $2Y("$2y$"),
        $2B("$2b$");

        private final String version;

        BCryptVersion(String version) {
            this.version = version;
        }

        /**
         * 获取盐
         * 
         * @return 盐
         */
        String getSalt() {
            // 生成 22 位随机盐
            SecureRandom random = new SecureRandom();
            byte[] salt = new byte[16];
            random.nextBytes(salt);
            return BCrypt.gensaltLog2(this.version, 10);
        }

        /**
         * 检查前缀是否匹配
         * 
         * @param encodedPassword 编码后的密码
         * @return 如果前缀匹配返回 true
         */
        boolean prefixMatches(String encodedPassword) {
            return encodedPassword.startsWith(this.version);
        }
    }
}
```

**源码分析要点**:

1. **自动加盐**: 每次哈希密码时，都会生成随机盐，这使得相同的密码会产生不同的哈希值。

2. **自适应强度**: 可以调整计算强度（4-31），强度越高，计算时间越长，但也越安全。

3. **密码验证**: `matches()` 方法会从编码后的密码中提取盐，然后使用相同的盐哈希原始密码，最后比较哈希值是否相等。

4. **格式**: 编码后的密码格式为 `$2a$[强度]$[22位盐][31位哈希值]`，例如：
   ```
   $2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy
   ```

#### 4.3 高级配置

**4.3.1 多个 SecurityFilterChain**

在某些情况下，你可能需要为不同的 URL 路径配置不同的安全规则。

```java
@Configuration
@EnableWebSecurity
public class MultiSecurityChainConfig {
    
    /**
     * 配置 REST API 的安全过滤器链
     * 
     * @param http HttpSecurity 配置对象
     * @return SecurityFilterChain 实例
     * @throws Exception 配置异常
     */
    @Bean
    @Order(1)  // 优先级高（数字越小，优先级越高）
    public SecurityFilterChain apiSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/api/**")  // 只处理 /api/** 开头的请求
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()  // 公开的 API
                .requestMatchers("/api/admin/**").hasRole("ADMIN")  // 管理 API
                .anyRequest().authenticated()  // 其他 API 需要认证
            )
            .httpBasic(withDefaults())  // 使用 HTTP Basic 认证
            .csrf(csrf -> csrf.disable())  // API 通常不需要 CSRF
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)  // 无状态会话
            );

        return http.build();
    }

    /**
     * 配置 Web 应用程序的安全过滤器链
     * 
     * @param http HttpSecurity 配置对象
     * @return SecurityFilterChain 实例
     * @throws Exception 配置异常
     */
    @Bean
    @Order(2)  // 优先级低
    public SecurityFilterChain webSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/home", "/register").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutSuccessUrl("/")
                .permitAll()
            );

        return http.build();
    }
}
```

**4.3.2 自定义过滤器**

你可以创建自定义过滤器并将其添加到过滤器链中。

```java
/**
 * 自定义过滤器示例
 * 
 * 这个过滤器用于记录请求日志
 */
public class RequestLoggingFilter extends OncePerRequestFilter {
    
    private static final Logger logger = LoggerFactory.getLogger(RequestLoggingFilter.class);

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain) 
            throws ServletException, IOException {
        
        // 记录请求信息
        logger.info("Request: {} {}", request.getMethod(), request.getRequestURI());
        
        // 继续执行过滤器链
        filterChain.doFilter(request, response);
        
        // 记录响应状态
        logger.info("Response: {}", response.getStatus());
    }
}
```

**将自定义过滤器添加到过滤器链**:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        // ... 其他配置 ...
        // 添加自定义过滤器
        .addFilterBefore(new RequestLoggingFilter(), 
            UsernamePasswordAuthenticationFilter.class);

    return http.build();
}
```

**添加过滤器的位置**:
- `addFilterBefore(filter, beforeFilter)`: 在指定过滤器之前添加
- `addFilterAfter(filter, afterFilter)`: 在指定过滤器之后添加
- `addFilterAt(filter, atFilter)`: 替换指定过滤器

**4.3.3 自定义 UserDetailsService**

在实际应用中，用户信息通常存储在数据库中，因此需要自定义 `UserDetailsService`。

```java
/**
 * 自定义 UserDetailsService
 * 
 * 从数据库加载用户信息
 */
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;

    /**
     * 根据用户名加载用户信息
     * 
     * @param username 用户名
     * @return 用户详情
     * @throws UsernameNotFoundException 用户不存在
     */
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 1. 从数据库查询用户
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException(
                "User not found: " + username));

        // 2. 将数据库用户转换为 Spring Security 的 UserDetails
        return org.springframework.security.core.userdetails.User.builder()
            .username(user.getUsername())
            .password(user.getPassword())
            .roles(user.getRoles().toArray(new String[0]))
            .accountLocked(!user.isActive())
            .disabled(!user.isEnabled())
            .build();
    }
}
```

**使用自定义 UserDetailsService**:

```java
@Bean
public UserDetailsService userDetailsService(UserRepository userRepository) {
    return username -> {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException(
                "User not found: " + username));

        return org.springframework.security.core.userdetails.User.builder()
            .username(user.getUsername())
            .password(user.getPassword())
            .roles(user.getRoles().toArray(new String[0]))
            .build();
    };
}
```

**4.3.4 自定义 AuthenticationProvider**

如果你需要自定义认证逻辑，可以实现 `AuthenticationProvider` 接口。

```java
/**
 * 自定义认证提供者
 * 
 * 实现自定义的认证逻辑
 */
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {
    
    @Autowired
    private UserDetailsService userDetailsService;
    
    @Autowired
    private PasswordEncoder passwordEncoder;

    /**
     * 执行认证
     * 
     * @param authentication 认证请求
     * @return 已认证的 Authentication 对象
     * @throws AuthenticationException 认证异常
     */
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        // 1. 提取用户名和密码
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();

        // 2. 加载用户信息
        UserDetails userDetails = userDetailsService.loadUserByUsername(username);

        // 3. 验证密码
        if (!passwordEncoder.matches(password, userDetails.getPassword())) {
            throw new BadCredentialsException("Invalid password");
        }

        // 4. 检查账户状态
        if (!userDetails.isEnabled()) {
            throw new DisabledException("Account is disabled");
        }

        if (!userDetails.isAccountNonLocked()) {
            throw new LockedException("Account is locked");
        }

        if (!userDetails.isAccountNonExpired()) {
            throw new AccountExpiredException("Account has expired");
        }

        if (!userDetails.isCredentialsNonExpired()) {
            throw new CredentialsExpiredException("Credentials have expired");
        }

        // 5. 创建已认证的 Authentication 对象
        return new UsernamePasswordAuthenticationToken(
            userDetails,
            password,
            userDetails.getAuthorities()
        );
    }

    /**
     * 检查是否支持该认证类型
     * 
     * @param authentication 认证类型
     * @return 如果支持返回 true
     */
    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }
}
```

#### 4.4 配置最佳实践

**4.4.1 安全配置原则**

1. **最小权限原则**: 只授予用户完成工作所需的最小权限
2. **默认拒绝**: 默认情况下拒绝所有访问，只允许明确授权的访问
3. **纵深防御**: 在多个层面实施安全控制
4. **安全默认值**: 使用安全的默认配置（如 BCrypt 密码编码）

**4.4.2 配置示例**

```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)  // 启用方法级别安全
public class SecurityConfig {
    
    /**
     * 配置安全过滤器链
     */
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            // 1. 授权配置：默认拒绝，明确允许
            .authorizeHttpRequests(auth -> auth
                // 静态资源
                .requestMatchers("/css/**", "/js/**", "/images/**").permitAll()
                // 公开页面
                .requestMatchers("/", "/home", "/register", "/forgot-password").permitAll()
                // 公开 API
                .requestMatchers("/api/public/**").permitAll()
                // 管理功能：需要 ADMIN 角色
                .requestMatchers("/admin/**", "/api/admin/**").hasRole("ADMIN")
                // 用户功能：需要 USER 角色
                .requestMatchers("/user/**", "/api/user/**").hasRole("USER")
                // 其他所有请求：需要认证
                .anyRequest().authenticated()
            )
            // 2. 表单登录
            .formLogin(form -> form
                .loginPage("/login")  // 自定义登录页面
                .loginProcessingUrl("/login")  // 登录处理 URL
                .defaultSuccessUrl("/dashboard", true)  // 登录成功后跳转
                .failureUrl("/login?error=true")  // 登录失败
                .permitAll()
            )
            // 3. 登出
            .logout(logout -> logout
                .logoutUrl("/logout")  // 登出 URL
                .logoutSuccessUrl("/login?logout=true")  // 登出成功后跳转
                .invalidateHttpSession(true)  // 使会话失效
                .clearAuthentication(true)  // 清除认证信息
                .deleteCookies("JSESSIONID")  // 删除 Cookie
                .permitAll()
            )
            // 4. CSRF 防护
            .csrf(csrf -> csrf
                .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
                .ignoringRequestMatchers("/api/**")  // API 可以忽略 CSRF
            )
            // 5. 会话管理
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)  // 按需创建会话
                .maximumSessions(1)  // 最大会话数
                .maxSessionsPreventsLogin(false)  // 不阻止新登录
                .sessionFixation().migrateSession()  // 会话固定攻击防护
            )
            // 6. 异常处理
            .exceptionHandling(exceptions -> exceptions
                .authenticationEntryPoint((request, response, authException) -> {
                    // AJAX 请求返回 401
                    if ("XMLHttpRequest".equals(request.getHeader("X-Requested-With"))) {
                        response.sendError(HttpServletResponse.SC_UNAUTHORIZED, authException.getMessage());
                    } else {
                        // 普通请求重定向到登录页面
                        response.sendRedirect("/login");
                    }
                })
                .accessDeniedHandler((request, response, accessDeniedException) -> {
                    // AJAX 请求返回 403
                    if ("XMLHttpRequest".equals(request.getHeader("X-Requested-With"))) {
                        response.sendError(HttpServletResponse.SC_FORBIDDEN, accessDeniedException.getMessage());
                    } else {
                        // 普通请求返回 403 页面
                        response.sendError(HttpServletResponse.SC_FORBIDDEN);
                    }
                })
            )
            // 7. 记住我功能
            .rememberMe(rememberMe -> rememberMe
                .key("uniqueAndSecret")  // 密钥（必须唯一）
                .tokenValiditySeconds(86400)  // Token 有效期（1 天）
                .rememberMeCookieName("remember-me")  // Cookie 名称
                .rememberMeParameter("remember-me")  // 表单参数名称
                .userDetailsService(userDetailsService)  // 用户详情服务
                .tokenRepository(persistentTokenRepository())  // Token 持久化
            )
            // 8. 匿名用户
            .anonymous(anonymous -> anonymous
                .key("anonymousKey")
                .principal("anonymousUser")
                .authorities(new SimpleGrantedAuthority("ROLE_ANONYMOUS"))
            )
            // 9. 安全头部
            .headers(headers -> headers
                .contentSecurityPolicy(csp -> csp
                    .policyDirectives("default-src 'self'"))
                .frameOptions(frameOptions -> frameOptions
                    .deny())  // 防止点击劫持
                .httpStrictTransportSecurity(hsts -> hsts
                    .includeSubDomains(true)
                    .maxAgeInSeconds(31536000))  // HSTS
                .xssProtection(xss -> xss
                    .headerValue("1; mode=block"))  // XSS 防护
            );

        return http.build();
    }

    /**
     * 配置密码编码器
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12);
    }

    /**
     * 配置用户详情服务
     */
    @Bean
    public UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
        UserDetails user = User.builder()
            .username("user")
            .password(passwordEncoder.encode("123456"))
            .roles("USER")
            .build();

        UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder.encode("admin123"))
            .roles("ADMIN", "USER")
            .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    /**
     * 配置 Remember Me Token 持久化
     */
    @Bean
    public PersistentTokenRepository persistentTokenRepository() {
        JdbcTokenRepositoryImpl tokenRepository = new JdbcTokenRepositoryImpl();
        tokenRepository.setDataSource(dataSource);
        return tokenRepository;
    }

    @Autowired
    private DataSource dataSource;
}
```

#### 4.5 常见问题（FAQ）

**Q1: 如何配置多个登录页面？**

A: 可以通过创建多个 `SecurityFilterChain` 来实现：

```java
@Bean
@Order(1)
public SecurityFilterChain adminSecurityFilterChain(HttpSecurity http) throws Exception {
    http
        .securityMatcher("/admin/**")
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/admin/login").permitAll()
            .requestMatchers("/admin/**").hasRole("ADMIN")
            .anyRequest().authenticated()
        )
        .formLogin(form -> form
            .loginPage("/admin/login")  // 管理员登录页面
            .loginProcessingUrl("/admin/login")
            .defaultSuccessUrl("/admin/dashboard", true)
            .permitAll()
        );

    return http.build();
}

@Bean
@Order(2)
public SecurityFilterChain userSecurityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/login").permitAll()
            .requestMatchers("/user/**").hasRole("USER")
            .anyRequest().authenticated()
        )
        .formLogin(form -> form
            .loginPage("/login")  // 普通用户登录页面
            .loginProcessingUrl("/login")
            .defaultSuccessUrl("/user/dashboard", true)
            .permitAll()
        );

    return http.build();
}
```

**Q2: 如何实现动态权限控制？**

A: 有多种方式实现动态权限控制：

**方法 1: 自定义 AccessDecisionVoter**
```java
@Component
public class CustomAccessDecisionVoter implements AccessDecisionVoter<Object> {
    
    @Autowired
    private PermissionService permissionService;

    @Override
    public int vote(Authentication authentication, Object object, 
            Collection<ConfigAttribute> attributes) {
        
        // 获取当前用户
        String username = authentication.getName();
        
        // 获取请求的 URL
        HttpServletRequest request = ((FilterInvocation) object).getHttpRequest();
        String url = request.getRequestURI();
        
        // 检查用户是否有访问该 URL 的权限
        boolean hasPermission = permissionService.hasPermission(username, url);
        
        return hasPermission ? ACCESS_GRANTED : ACCESS_DENIED;
    }

    @Override
    public boolean supports(ConfigAttribute attribute) {
        return true;
    }

    @Override
    public boolean supports(Class<?> clazz) {
        return true;
    }
}
```

**方法 2: 使用自定义 FilterSecurityInterceptor**
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .anyRequest().access((authentication, request) -> {
                // 动态检查权限
                String username = authentication.get().getName();
                String url = request.getRequestURI();
                boolean hasPermission = permissionService.hasPermission(username, url);
                return new AuthorizationDecision(hasPermission);
            })
        );

    return http.build();
}
```

**方法 3: 使用 @PreAuthorize 注解**
```java
@Service
public class DocumentService {
    
    @PreAuthorize("@permissionService.hasPermission(authentication.name, #documentId)")
    public Document getDocument(String documentId) {
        return documentRepository.findById(documentId);
    }
}
```

**Q3: 如何配置 OAuth 2.0 登录？**

A: Spring Security 提供了对 OAuth 2.0 的完整支持：

**依赖**:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

**配置**:
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: your-client-id
            client-secret: your-client-secret
            scope:
              - profile
              - email
          github:
            client-id: your-client-id
            client-secret: your-client-secret
            scope:
              - read:user
              - user:email
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://www.googleapis.com/oauth2/v2/userinfo
```

**安全配置**:
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .anyRequest().authenticated()
        )
        .oauth2Login(withDefaults());

    return http.build();
}
```

**Q4: 如何实现基于 IP 的访问控制？**

A: 可以通过自定义过滤器或使用 SpEL 表达式：

**方法 1: 自定义过滤器**
```java
public class IPFilter extends OncePerRequestFilter {
    
    private static final Set<String> ALLOWED_IPS = Set.of(
        "127.0.0.1",
        "192.168.1.100"
    );

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain) 
            throws ServletException, IOException {
        
        String clientIP = request.getRemoteAddr();
        
        if (!ALLOWED_IPS.contains(clientIP)) {
            response.sendError(HttpServletResponse.SC_FORBIDDEN, "Access denied");
            return;
        }
        
        filterChain.doFilter(request, response);
    }
}
```

**方法 2: 使用 SpEL**
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/admin/**").access(
                (authentication, request) -> {
                    String ip = request.getRemoteAddr();
                    boolean isAdmin = authentication.get()
                        .getAuthorities().stream()
                        .anyMatch(a -> a.getAuthority().equals("ROLE_ADMIN"));
                    boolean isAllowedIP = Set.of("127.0.0.1", "192.168.1.100").contains(ip);
                    return new AuthorizationDecision(isAdmin && isAllowedIP);
                }
            )
            .anyRequest().authenticated()
        );

    return http.build();
}
```

**Q5: 如何测试 Spring Security 配置？**

A: Spring Security 提供了强大的测试支持：

**依赖**:
```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

**测试示例**:
```java
@SpringBootTest
@AutoConfigureMockMvc
public class SecurityConfigTest {
    
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testAccessHomePageWithoutAuthentication() throws Exception {
        mockMvc.perform(get("/"))
            .andExpect(status().isOk());
    }

    @Test
    public void testAccessAdminPageWithoutAuthentication() throws Exception {
        mockMvc.perform(get("/admin"))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("http://localhost/login"));
    }

    @Test
    @WithMockUser(username = "user", roles = "USER")
    public void testAccessAdminPageWithUserRole() throws Exception {
        mockMvc.perform(get("/admin"))
            .andExpect(status().isForbidden());
    }

    @Test
    @WithMockUser(username = "admin", roles = "ADMIN")
    public void testAccessAdminPageWithAdminRole() throws Exception {
        mockMvc.perform(get("/admin"))
            .andExpect(status().isOk());
    }

    @Test
    public void testLoginWithValidCredentials() throws Exception {
        mockMvc.perform(post("/login")
                .param("username", "user")
                .param("password", "123456")
                .with(csrf()))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("/"));
    }

    @Test
    public void testLoginWithInvalidCredentials() throws Exception {
        mockMvc.perform(post("/login")
                .param("username", "user")
                .param("password", "wrong-password")
                .with(csrf()))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("/login?error=true"));
    }
}
```

#### 4.6 思考题

1. **配置方式选择**:
   - 问题：为什么推荐使用基于 Bean 的配置方式而不是继承 `WebSecurityConfigurerAdapter`？
   - 提示：考虑灵活性、Spring Boot 设计理念、多个 SecurityFilterChain

2. **密码编码器选择**:
   - 问题：如何在不同的密码编码器（BCrypt、PBKDF2、SCrypt、Argon2）之间选择？
   - 提示：考虑安全性、性能、算法特性

3. **多安全链设计**:
   - 问题：在什么情况下应该使用多个 SecurityFilterChain？
   - 提示：考虑不同的安全需求（API vs Web、不同的认证方式）

4. **动态权限管理**:
   - 问题：如何设计一个灵活的动态权限管理系统？
   - 提示：考虑权限的数据结构、缓存、性能优化

5. **测试策略**:
   - 问题：如何全面测试 Spring Security 的配置？
   - 提示：考虑单元测试、集成测试、安全测试

---

## 模块2 - 认证机制深度剖析

### 知识点5: 认证核心原理

#### 5.1 认证概述

认证（Authentication）是验证用户身份的过程，是安全系统的第一道防线。Spring Security 提供了灵活且强大的认证机制。

**5.1.1 认证与授权的区别**

| 特性 | 认证（Authentication） | 授权（Authorization） |
|------|----------------------|---------------------|
| **问题** | 你是谁？ | 你能做什么？ |
| **时机** | 授权之前 | 认证之后 |
| **目的** | 验证用户身份 | 控制用户访问权限 |
| **核心** | 用户名、密码、生物特征等 | 角色、权限等 |
| **失败后果** | 无法访问系统 | 无法访问特定资源 |

**5.1.2 认证方式分类**

1. **基于知识的认证**:
   - 用户名/密码
   - PIN 码
   - 安全问题

2. **基于持有的认证**:
   - 硬件令牌
   - 手机验证码
   - 数字证书

3. **基于生物特征的认证**:
   - 指纹
   - 面部识别
   - 虹膜扫描

4. **多因素认证（MFA）**:
   - 结合多种认证方式
   - 提高安全性

**5.1.3 Spring Security 支持的认证方式**

1. **用户名/密码认证** (最常用)
2. **HTTP Basic 认证**
3. **HTTP Digest 认证**
4. **X.509 证书认证**
5. **LDAP 认证**
6. **Remember Me 认证**
7. **匿名认证**
8. **OAuth 2.0 / OpenID Connect**
9. **JWT (JSON Web Token)**

#### 5.2 认证流程深度分析

**5.2.1 完整认证流程图**

```
用户提交登录表单
       ↓
┌──────────────────────────────────────────────────────┐
│ 1. UsernamePasswordAuthenticationFilter              │
│    - 拦截登录请求（POST /login）                      │
│    - 提取用户名和密码                                │
│    - 创建未认证的 Authentication 对象                │
└──────────────────────────────────────────────────────┘
       ↓
┌──────────────────────────────────────────────────────┐
│ 2. AuthenticationManager                             │
│    - 接收未认证的 Authentication 对象                │
│    - 遍历 AuthenticationProvider 列表                │
└──────────────────────────────────────────────────────┘
       ↓
┌──────────────────────────────────────────────────────┐
│ 3. AuthenticationProvider                           │
│    - 检查是否支持该认证类型                          │
│    - 调用 UserDetailsService 加载用户                │
└──────────────────────────────────────────────────────┘
       ↓
┌──────────────────────────────────────────────────────┐
│ 4. UserDetailsService                               │
│    - 从数据源加载用户信息                            │
│    - 返回 UserDetails 对象                          │
└──────────────────────────────────────────────────────┘
       ↓
┌──────────────────────────────────────────────────────┐
│ 5. AuthenticationProvider                           │
│    - 检查用户状态（锁定、过期等）                    │
│    - 使用 PasswordEncoder 验证密码                  │
└──────────────────────────────────────────────────────┘
       ↓
   密码匹配？
       ├─ 是 → 继续检查用户状态
       │         ├─ 状态正常 → 创建已认证的 Authentication
       │         │             ├─ 设置用户详情
       │         │             ├─ 设置权限
       │         │             └─ 清除密码（安全）
       │         └─ 状态异常 → 抛出异常
       │                        ├─ LockedException
       │                        ├─ DisabledException
       │                        └─ CredentialsExpiredException
       │
       └─ 否 → 抛出 BadCredentialsException
                ↓
          ExceptionTranslationFilter
                ↓
          重定向到登录页面（带错误信息）
```

**5.2.2 源码分析：认证流程关键类**

##### UsernamePasswordAuthenticationFilter 源码详解

```java
package org.springframework.security.web.authentication;

/**
 * UsernamePasswordAuthenticationFilter
 * 
 * 处理基于用户名密码的认证请求
 * 
 * 主要职责：
 * 1. 拦截特定的 URL（默认是 POST /login）
 * 2. 从请求中提取用户名和密码
 * 3. 创建未认证的 Authentication 对象
 * 4. 委托给 AuthenticationManager 进行认证
 * 5. 认证成功后，将 Authentication 对象设置到 SecurityContext
 * 6. 认证失败后，委托给 AuthenticationFailureHandler 处理
 * 
 * 可配置参数：
 * - loginProcessingUrl: 登录处理 URL（默认是 /login）
 * - usernameParameter: 用户名参数名（默认是 username）
 * - passwordParameter: 密码参数名（默认是 password）
 * - postOnly: 是否只支持 POST 方法（默认是 true）
 */
public class UsernamePasswordAuthenticationFilter extends 
        AbstractAuthenticationProcessingFilter {
    
    /**
     * 用户名参数名
     * 
     * 默认值是 "username"
     */
    private String usernameParameter = "username";
    
    /**
     * 密码参数名
     * 
     * 默认值是 "password"
     */
    private String passwordParameter = "password";
    
    /**
     * 是否只支持 POST 方法
     * 
     * 默认值是 true，出于安全考虑
     */
    private boolean postOnly = true;

    /**
     * 构造函数
     * 
     * 创建一个默认的过滤器，只处理 POST /login 请求
     */
    public UsernamePasswordAuthenticationFilter() {
        super(new AntPathRequestMatcher("/login", "POST"));
    }

    /**
     * 核心方法：执行认证
     * 
     * 认证流程：
     * 1. 检查请求方法（如果 postOnly 为 true）
     * 2. 从请求中提取用户名和密码
     * 3. 创建未认证的 UsernamePasswordAuthenticationToken 对象
     * 4. 设置认证详情（如 IP 地址、会话 ID 等）
     * 5. 委托给 AuthenticationManager 进行认证
     * 6. 如果认证成功，返回已认证的 Authentication 对象
     * 7. 如果认证失败，抛出 AuthenticationException
     * 
     * @param request HTTP 请求
     * @return 认证结果（已认证的 Authentication 对象）
     * @throws AuthenticationException 认证失败
     */
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, 
            HttpServletResponse response) throws AuthenticationException {
        
        // 1. 检查请求方法
        if (this.postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                "Authentication method not supported: " + request.getMethod());
        }
        
        // 2. 从请求中提取用户名
        String username = obtainUsername(request);
        
        // 3. 去除用户名前后的空格
        username = (username != null) ? username : "";
        username = username.trim();
        
        // 4. 从请求中提取密码
        String password = obtainPassword(request);
        password = (password != null) ? password : "";
        
        // 5. 创建未认证的 UsernamePasswordAuthenticationToken 对象
        //    构造参数：(principal, credentials, authorities)
        //    - principal: 用户名
        //    - credentials: 密码
        //    - authorities: 权限（未认证时为 null）
        UsernamePasswordAuthenticationToken authRequest = 
            new UsernamePasswordAuthenticationToken(username, password);
        
        // 6. 设置认证详情
        //    AuthenticationDetailsSource 负责构建额外的认证详情
        //    通常包括：
        //    - remoteAddress: 客户端 IP 地址
        //    - sessionId: 会话 ID
        setDetails(request, authRequest);
        
        // 7. 委托给 AuthenticationManager 进行认证
        //    AuthenticationManager 会遍历所有的 AuthenticationProvider，
        //    直到找到能处理该认证请求的 Provider
        //    如果认证成功，返回已认证的 Authentication 对象
        //    如果认证失败，抛出 AuthenticationException
        return this.getAuthenticationManager().authenticate(authRequest);
    }

    /**
     * 从请求中获取用户名
     * 
     * 子类可以重写此方法以支持自定义的用户名提取逻辑
     * 
     * @param request HTTP 请求
     * @return 用户名
     */
    @Nullable
    protected String obtainUsername(HttpServletRequest request) {
        return request.getParameter(this.usernameParameter);
    }

    /**
     * 从请求中获取密码
     * 
     * 子类可以重写此方法以支持自定义的密码提取逻辑
     * 
     * @param request HTTP 请求
     * @return 密码
     */
    @Nullable
    protected String obtainPassword(HttpServletRequest request) {
        return request.getParameter(this.passwordParameter);
    }

    /**
     * 设置认证详情
     * 
     * 认证详情通常包括：
     * - 客户端 IP 地址
     * - 会话 ID
     * - 其他认证相关信息
     * 
     * @param request HTTP 请求
     * @param authRequest Authentication 对象
     */
    protected void setDetails(HttpServletRequest request, 
            UsernamePasswordAuthenticationToken authRequest) {
        // 使用 AuthenticationDetailsSource 构建认证详情
        authRequest.setDetails(this.authenticationDetailsSource.buildDetails(request));
    }

    /**
     * 设置用户名参数名
     * 
     * @param usernameParameter 用户名参数名
     */
    public void setUsernameParameter(String usernameParameter) {
        this.usernameParameter = usernameParameter;
    }

    /**
     * 设置密码参数名
     * 
     * @param passwordParameter 密码参数名
     */
    public void setPasswordParameter(String passwordParameter) {
        this.passwordParameter = passwordParameter;
    }

    /**
     * 设置是否只支持 POST 方法
     * 
     * @param postOnly 是否只支持 POST
     */
    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }
}
```

**源码分析要点**:

1. **认证请求创建**: 创建 `UsernamePasswordAuthenticationToken` 对象时，使用的是构造函数 `new UsernamePasswordAuthenticationToken(username, password)`，这个构造函数创建的是一个**未认证**的 Authentication 对象（`authenticated = false`）。

2. **认证详情**: 使用 `AuthenticationDetailsSource` 来构建额外的认证详情，这对于审计和日志记录非常重要。

3. **委托模式**: 这个过滤器本身不执行认证逻辑，而是委托给 `AuthenticationManager`，这使得认证逻辑可以灵活配置。

4. **只支持 POST**: 默认情况下，登录请求必须是 POST 方法，这是出于安全考虑（防止 CSRF 攻击）。

##### UsernamePasswordAuthenticationToken 源码详解

```java
package org.springframework.security.authentication;

import java.util.Collection;

/**
 * UsernamePasswordAuthenticationToken
 * 
 * 用于基于用户名密码的认证
 * 
 * 这个类有两个生命周期：
 * 1. 认证前：表示认证请求，包含用户名和密码
 * 2. 认证后：表示已认证的主体，包含用户详情和权限
 * 
 * 设计模式：
 * - 值对象模式：不可变对象
 * - 工厂方法模式：提供多个静态工厂方法用于创建不同状态的对象
 */
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {

    /**
     * 序列化版本 ID
     */
    private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;

    /**
     * 主体标识
     * 
     * 认证前：通常是用户名（String）
     * 认证后：通常是 UserDetails 对象
     */
    private final Object principal;

    /**
     * 凭证
     * 
     * 认证前：是密码（String）
     * 认证后：通常被清除（设为 null），出于安全考虑
     */
    private Object credentials;

    /**
     * 构造函数 1：未认证的 Token
     * 
     * 用于创建认证请求对象
     * 
     * @param principal 用户名
     * @param credentials 密码
     */
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
        super(null);
        this.principal = principal;
        this.credentials = credentials;
        setAuthenticated(false);  // 标记为未认证
    }

    /**
     * 构造函数 2：已认证的 Token
     * 
     * 用于创建已认证的主体对象
     * 
     * @param principal 用户详情（UserDetails）
     * @param credentials 凭证（通常为 null）
     * @param authorities 权限列表
     */
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
            Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(true);  // 标记为已认证
    }

    /**
     * 获取凭证
     * 
     * @return 密码
     */
    @Override
    public Object getCredentials() {
        return this.credentials;
    }

    /**
     * 获取主体标识
     * 
     * @return 用户名或 UserDetails
     */
    @Override
    public Object getPrincipal() {
        return this.principal;
    }

    /**
     * 设置认证状态
     * 
     * 只能设置为 false，不能设置为 true
     * 
     * @param authenticated 认证状态
     * @throws IllegalArgumentException 如果尝试设置为 true
     */
    @Override
    public void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException {
        // 只能设置为 false
        // 如果需要设置为 true，应该使用构造函数创建已认证的 Token
        Assert.isTrue(!isAuthenticated,
            "Cannot set this token to trusted - use constructor which takes a GrantedAuthority list instead");
        super.setAuthenticated(false);
    }

    /**
     * 擦除凭证
     * 
     * 出于安全考虑，认证后应该清除敏感信息
     */
    @Override
    public void eraseCredentials() {
        super.eraseCredentials();
        this.credentials = null;
    }
}
```

**源码分析要点**:

1. **两种构造函数**: 提供了两个构造函数，一个用于创建未认证的 Token，一个用于创建已认证的 Token。

2. **不可变性**: 这个类设计为不可变对象，一旦创建就不能修改（除了擦除凭证）。

3. **安全设计**: `setAuthenticated(true)` 只能通过构造函数调用，防止外部代码随意将 Token 设置为已认证状态。

4. **凭证清除**: 认证成功后，应该调用 `eraseCredentials()` 清除密码等敏感信息。

##### AbstractAuthenticationProcessingFilter 源码详解

```java
package org.springframework.security.web.authentication;

/**
 * AbstractAuthenticationProcessingFilter
 * 
 * 处理认证请求的抽象过滤器
 * 
 * 这是所有认证过滤器的基类，提供了通用的认证处理流程
 * 
 * 主要职责：
 * 1. 拦截特定的 URL（登录请求）
 * 2. 调用子类实现的 attemptAuthentication() 方法
 * 3. 处理认证成功和失败的情况
 * 4. 将认证结果保存到 SecurityContext
 * 
 * 模板方法模式：
 * - attemptAuthentication(): 抽象方法，由子类实现具体的认证逻辑
 * - successfulAuthentication(): 认证成功后的处理（可重写）
 * - unsuccessfulAuthentication(): 认证失败后的处理（可重写）
 */
public abstract class AbstractAuthenticationProcessingFilter extends GenericFilterBean {

    /**
     * 认证管理器
     * 
     * 用于执行实际的认证逻辑
     */
    private AuthenticationManager authenticationManager;

    /**
     * 认证成功处理器
     * 
     * 处理认证成功后的逻辑，如重定向、返回 Token 等
     */
    private AuthenticationSuccessHandler successHandler = new SavedRequestAwareAuthenticationSuccessHandler();

    /**
     * 认证失败处理器
     * 
     * 处理认证失败后的逻辑，如重定向到错误页面、返回错误信息等
     */
    private AuthenticationFailureHandler failureHandler = new SimpleUrlAuthenticationFailureHandler();

    /**
     * 请求匹配器
     * 
     * 用于判断当前请求是否是需要处理的认证请求
     */
    private RequestMatcher requiresAuthenticationRequestMatcher;

    /**
     * 是否继续过滤器链（认证失败时）
     * 
     * 默认为 false，即认证失败时不继续执行过滤器链
     */
    private boolean continueChainBeforeSuccessfulAuthentication = false;

    /**
     * 会话认证策略
     * 
     * 控制认证后的会话管理
     */
    private SessionAuthenticationStrategy sessionStrategy = new NullSessionAuthenticationStrategy();

    /**
     * 核心方法：处理过滤请求
     * 
     * @param request 请求
     * @param response 响应
     * @param chain 过滤器链
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;

        // 1. 检查当前请求是否是需要处理的认证请求
        if (!requiresAuthentication(httpRequest, httpResponse)) {
            // 不是认证请求，继续执行过滤器链
            chain.doFilter(request, response);
            return;
        }

        // 2. 如果需要，在认证前继续执行过滤器链
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Request is to process authentication");
        }

        Authentication authResult;
        try {
            // 3. 调用子类实现的认证方法
            //    这是模板方法模式的核心
            authResult = attemptAuthentication(httpRequest, httpResponse);
            
            if (authResult == null) {
                // 认证结果为 null，返回
                return;
            }

            // 4. 认证成功后的会话处理
            this.sessionStrategy.onAuthentication(authResult, httpRequest, httpResponse);

            // 5. 如果需要，在认证成功前继续执行过滤器链
            if (this.continueChainBeforeSuccessfulAuthentication) {
                chain.doFilter(request, response);
            }
        } 
        catch (InternalAuthenticationServiceException failed) {
            // 6. 认证过程中发生内部错误
            this.logger.error("An internal error occurred while trying to authenticate the user.", failed);
            unsuccessfulAuthentication(httpRequest, httpResponse, failed);
            return;
        } 
        catch (AuthenticationException failed) {
            // 7. 认证失败
            unsuccessfulAuthentication(httpRequest, httpResponse, failed);
            return;
        }

        // 8. 认证成功
        //    将认证结果设置到 SecurityContext
        successfulAuthentication(httpRequest, httpResponse, chain, authResult);
    }

    /**
     * 抽象方法：执行认证
     * 
     * 子类必须实现此方法，提供具体的认证逻辑
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @return 认证结果（已认证的 Authentication 对象）
     * @throws AuthenticationException 认证失败
     */
    public abstract Authentication attemptAuthentication(HttpServletRequest request, 
            HttpServletResponse response) throws AuthenticationException;

    /**
     * 认证成功后的处理
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @param chain 过滤器链
     * @param authResult 认证结果
     */
    protected void successfulAuthentication(HttpServletRequest request, 
            HttpServletResponse response, FilterChain chain, Authentication authResult)
            throws IOException, ServletException {
        
        // 1. 将认证结果保存到 SecurityContext
        SecurityContext context = SecurityContextHolder.createEmptyContext();
        context.setAuthentication(authResult);
        SecurityContextHolder.setContext(context);

        // 2. 如果需要，将 SecurityContext 保存到 Session
        if (this.securityContextRepository != null) {
            this.securityContextRepository.saveContext(context, request, response);
        }

        // 3. 调用成功处理器
        //    默认实现是重定向到之前保存的请求或默认成功页面
        this.successHandler.onAuthenticationSuccess(request, response, authResult);
    }

    /**
     * 认证失败后的处理
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @param failed 认证异常
     */
    protected void unsuccessfulAuthentication(HttpServletRequest request, 
            HttpServletResponse response, AuthenticationException failed)
            throws IOException, ServletException {
        
        // 1. 清除 SecurityContext
        SecurityContextHolder.clearContext();

        // 2. 调用失败处理器
        //    默认实现是重定向到登录失败页面
        this.failureHandler.onAuthenticationFailure(request, response, failed);
    }

    /**
     * 检查是否需要处理当前请求
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @return 如果需要处理返回 true
     */
    protected boolean requiresAuthentication(HttpServletRequest request, 
            HttpServletResponse response) {
        return this.requiresAuthenticationRequestMatcher.matches(request);
    }
}
```

**源码分析要点**:

1. **模板方法模式**: `attemptAuthentication()` 是抽象方法，由子类实现具体的认证逻辑。

2. **认证成功处理**: 将认证结果保存到 `SecurityContext`，然后调用 `AuthenticationSuccessHandler`。

3. **认证失败处理**: 清除 `SecurityContext`，然后调用 `AuthenticationFailureHandler`。

4. **会话管理**: 使用 `SessionAuthenticationStrategy` 来管理认证后的会话。

#### 5.3 认证异常体系

Spring Security 定义了丰富的认证异常类，用于表示不同的认证失败情况。

**5.3.1 异常层次结构**

```
AuthenticationException (认证异常基类)
    ├── AccountExpiredException (账户过期)
    ├── AuthenticationCredentialsNotFoundException (找不到认证凭据)
    ├── AuthenticationServiceException (认证服务异常)
    ├── BadCredentialsException (错误的凭据)
    ├── CredentialsExpiredException (凭证过期)
    ├── DisabledException (账户已禁用)
    ├── InsufficientAuthenticationException (认证信息不足)
    │   └── PreAuthenticatedCredentialsNotFoundException (预认证凭据未找到)
    ├── LockedException (账户已锁定)
    ├── OAuth2AuthenticationException (OAuth2 认证异常)
    ├── ProviderNotFoundException (找不到认证提供者)
    ├── RememberMeAuthenticationException (记住我认证异常)
    ├── SessionAuthenticationException (会话认证异常)
    └── UsernameNotFoundException (用户名未找到)
```

**5.3.2 常见异常详解**

**1. BadCredentialsException（错误的凭据）**

```java
package org.springframework.security.authentication;

/**
 * BadCredentialsException
 * 
 * 当用户提供的凭据（用户名/密码）不正确时抛出
 * 
 * 注意：出于安全考虑，默认情况下会隐藏用户不存在异常，
 * 统一抛出 BadCredentialsException，防止用户枚举攻击
 */
public class BadCredentialsException extends AuthenticationException {
    
    public BadCredentialsException(String msg) {
        super(msg);
    }

    public BadCredentialsException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 用户名或密码错误
- 签名验证失败
- Token 验证失败

**2. UsernameNotFoundException（用户名未找到）**

```java
package org.springframework.security.core.userdetails;

/**
 * UsernameNotFoundException
 * 
 * 当找不到指定的用户名时抛出
 * 
 * 注意：这个异常默认会被转换为 BadCredentialsException，
 * 以防止用户枚举攻击
 */
public class UsernameNotFoundException extends AuthenticationException {
    
    public UsernameNotFoundException(String msg) {
        super(msg);
    }

    public UsernameNotFoundException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 用户不存在
- 用户已被删除

**3. DisabledException（账户已禁用）**

```java
package org.springframework.security.authentication;

/**
 * DisabledException
 * 
 * 当用户账户被禁用时抛出
 * 
 * 常见原因：
 * - 管理员禁用了账户
 * - 账户被标记为不活跃
 * - 账户未激活
 */
public class DisabledException extends AuthenticationException {
    
    public DisabledException(String msg) {
        super(msg);
    }

    public DisabledException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 用户账户被管理员禁用
- 邮箱未验证
- 账户未激活

**4. LockedException（账户已锁定）**

```java
package org.springframework.security.authentication;

/**
 * LockedException
 * 
 * 当用户账户被锁定时抛出
 * 
 * 常见原因：
 * - 多次登录失败
 * - 安全策略触发
 * - 管理员手动锁定
 */
public class LockedException extends AuthenticationException {
    
    public LockedException(String msg) {
        super(msg);
    }

    public LockedException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 多次登录失败
- 安全策略触发锁定
- 管理员手动锁定

**5. AccountExpiredException（账户过期）**

```java
package org.springframework.security.authentication;

/**
 * AccountExpiredException
 * 
 * 当用户账户过期时抛出
 * 
 * 常见原因：
 * - 账户有效期已过
 * - 订阅过期
 * - 账户未续期
 */
public class AccountExpiredException extends AuthenticationException {
    
    public AccountExpiredException(String msg) {
        super(msg);
    }

    public AccountExpiredException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 账户有效期已过
- 试用期结束
- 订阅过期

**6. CredentialsExpiredException（凭证过期）**

```java
package org.springframework.security.authentication;

/**
 * CredentialsExpiredException
 * 
 * 当用户凭证（如密码）过期时抛出
 * 
 * 常见原因：
 * - 密码过期
 * - 证书过期
 * - Token 过期
 */
public class CredentialsExpiredException extends AuthenticationException {
    
    public CredentialsExpiredException(String msg) {
        super(msg);
    }

    public CredentialsExpiredException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```

**使用场景**:
- 密码过期需要修改
- 证书过期需要更新
- Token 过期需要刷新

#### 5.4 认证事件监听

Spring Security 提供了认证事件监听机制，允许你在认证成功或失败时执行自定义逻辑。

**5.4.1 认证事件类型**

1. **AuthenticationSuccessEvent**: 认证成功事件
2. **AbstractAuthenticationFailureEvent**: 认证失败事件
   - **BadCredentialsEvent**: 错误的凭据
   - **UsernameNotFoundEvent**: 用户名未找到
   - **AuthenticationFailureDisabledEvent**: 账户已禁用
   - **AuthenticationFailureLockedEvent**: 账户已锁定
   - **AuthenticationFailureExpiredEvent**: 账户或凭证过期

**5.4.2 监听认证事件**

```java
/**
 * 认证事件监听器
 * 
 * 监听认证成功和失败事件，执行自定义逻辑
 */
@Component
@Slf4j
public class AuthenticationEventListener {
    
    /**
     * 监听认证成功事件
     * 
     * @param event 认证成功事件
     */
    @EventListener
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        // 获取认证信息
        Authentication authentication = event.getAuthentication();
        String username = authentication.getName();
        
        // 记录日志
        log.info("User {} authenticated successfully", username);
        
        // 更新最后登录时间
        updateLastLoginTime(username);
        
        // 发送登录成功通知
        sendLoginSuccessNotification(username);
    }

    /**
     * 监听认证失败事件
     * 
     * @param event 认证失败事件
     */
    @EventListener
    public void onAuthenticationFailure(AbstractAuthenticationFailureEvent event) {
        // 获取异常信息
        AuthenticationException exception = event.getException();
        Authentication authentication = event.getAuthentication();
        
        String username = (authentication != null) ? authentication.getName() : "unknown";
        
        // 记录日志
        log.warn("Authentication failed for user {}: {}", username, exception.getMessage());
        
        // 更新登录失败次数
        updateFailedLoginAttempts(username);
        
        // 检查是否需要锁定账户
        checkAndLockAccount(username);
    }

    /**
     * 监听错误凭据事件
     * 
     * @param event 错误凭据事件
     */
    @EventListener
    public void onBadCredentials(BadCredentialsEvent event) {
        Authentication authentication = event.getAuthentication();
        String username = authentication.getName();
        
        log.warn("Bad credentials for user {}", username);
    }

    /**
     * 更新最后登录时间
     * 
     * @param username 用户名
     */
    private void updateLastLoginTime(String username) {
        // 实现更新最后登录时间的逻辑
        // 例如：更新数据库中的 last_login_time 字段
    }

    /**
     * 发送登录成功通知
     * 
     * @param username 用户名
     */
    private void sendLoginSuccessNotification(String username) {
        // 实现发送登录成功通知的逻辑
        // 例如：发送邮件或短信通知
    }

    /**
     * 更新登录失败次数
     * 
     * @param username 用户名
     */
    private void updateFailedLoginAttempts(String username) {
        // 实现更新登录失败次数的逻辑
        // 例如：在数据库中增加 failed_login_attempts 计数器
    }

    /**
     * 检查是否需要锁定账户
     * 
     * @param username 用户名
     */
    private void checkAndLockAccount(String username) {
        // 实现检查和锁定账户的逻辑
        // 例如：如果失败次数超过 5 次，锁定账户
    }
}
```

#### 5.5 常见问题（FAQ）

**Q1: 如何自定义认证失败时的错误消息？**

A: 有多种方式可以自定义错误消息：

**方法 1: 使用 messages.properties 文件**
```properties
# messages.properties
AbstractUserDetailsAuthenticationProvider.badCredentials=用户名或密码错误
AbstractUserDetailsAuthenticationProvider.disabled=账户已禁用
AbstractUserDetailsAuthenticationProvider.locked=账户已锁定
AbstractUserDetailsAuthenticationProvider.expired=账户已过期
AbstractUserDetailsAuthenticationProvider.credentialsExpired=密码已过期
```

**方法 2: 自定义 AuthenticationFailureHandler**
```java
@Component
public class CustomAuthenticationFailureHandler implements AuthenticationFailureHandler {
    
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, 
            HttpServletResponse response, AuthenticationException exception)
            throws IOException, ServletException {
        
        String errorMessage;
        
        if (exception instanceof BadCredentialsException) {
            errorMessage = "用户名或密码错误";
        } else if (exception instanceof DisabledException) {
            errorMessage = "账户已禁用";
        } else if (exception instanceof LockedException) {
            errorMessage = "账户已锁定";
        } else if (exception instanceof AccountExpiredException) {
            errorMessage = "账户已过期";
        } else if (exception instanceof CredentialsExpiredException) {
            errorMessage = "密码已过期";
        } else {
            errorMessage = "认证失败";
        }
        
        // 设置错误消息
        request.getSession().setAttribute("error", errorMessage);
        
        // 重定向到登录页面
        response.sendRedirect("/login?error=true");
    }
}
```

**方法 3: 使用异常处理器**
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .exceptionHandling(exceptions -> exceptions
            .authenticationEntryPoint((request, response, authException) -> {
                String errorMessage = "认证失败";
                
                if (authException instanceof BadCredentialsException) {
                    errorMessage = "用户名或密码错误";
                }
                
                response.sendRedirect("/login?error=" + URLEncoder.encode(errorMessage, "UTF-8"));
            })
        );

    return http.build();
}
```

**Q2: 如何实现登录失败次数限制？**

A: 可以通过自定义 `AuthenticationProvider` 或使用监听器来实现：

**方法 1: 自定义 AuthenticationProvider**
```java
@Component
public class CustomAuthenticationProvider extends DaoAuthenticationProvider {
    
    @Autowired
    private LoginAttemptService loginAttemptService;

    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        String username = authentication.getName();
        
        // 检查账户是否被锁定
        if (loginAttemptService.isBlocked(username)) {
            throw new LockedException("账户已被锁定，请稍后再试");
        }
        
        try {
            // 调用父类的认证方法
            Authentication auth = super.authenticate(authentication);
            
            // 认证成功，清除失败记录
            loginAttemptService.loginSucceeded(username);
            
            return auth;
        } catch (AuthenticationException ex) {
            // 认证失败，增加失败次数
            loginAttemptService.loginFailed(username);
            
            throw ex;
        }
    }
}
```

**方法 2: 使用事件监听器**
```java
@Component
public class LoginAttemptHandler {
    
    private static final int MAX_ATTEMPTS = 5;
    private static final long BLOCK_TIME = 30 * 60 * 1000; // 30 分钟
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    @EventListener
    public void onAuthenticationFailure(AbstractAuthenticationFailureEvent event) {
        String username = event.getAuthentication().getName();
        
        // 增加失败次数
        String key = "login_attempt:" + username;
        Long attempts = redisTemplate.opsForValue().increment(key);
        
        // 设置过期时间
        if (attempts == 1) {
            redisTemplate.expire(key, BLOCK_TIME, TimeUnit.MILLISECONDS);
        }
        
        // 检查是否超过最大尝试次数
        if (attempts >= MAX_ATTEMPTS) {
            // 锁定账户
            String blockKey = "login_blocked:" + username;
            redisTemplate.opsForValue().set(blockKey, "true", BLOCK_TIME, TimeUnit.MILLISECONDS);
        }
    }
    
    @EventListener
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        String username = event.getAuthentication().getName();
        
        // 清除失败记录
        redisTemplate.delete("login_attempt:" + username);
    }
    
    public boolean isBlocked(String username) {
        String blockKey = "login_blocked:" + username;
        return Boolean.TRUE.equals(redisTemplate.hasKey(blockKey));
    }
}
```

**Q3: 如何实现账户锁定和解锁功能？**

A: 可以通过实现 `UserDetails` 接口或使用数据库字段来实现：

**方法 1: 在 UserDetails 中实现**
```java
@Entity
public class User implements UserDetails {
    
    private String username;
    private String password;
    private boolean locked = false;  // 是否锁定
    private boolean enabled = true;  // 是否启用
    private LocalDateTime lockedTime;  // 锁定时间
    private LocalDateTime lockExpirationTime;  // 锁定过期时间
    
    @Override
    public boolean isAccountNonLocked() {
        // 检查是否被锁定
        if (!locked) {
            return true;
        }
        
        // 检查锁定是否已过期
        if (lockExpirationTime != null && LocalDateTime.now().isAfter(lockExpirationTime)) {
            locked = false;
            lockedTime = null;
            lockExpirationTime = null;
            return true;
        }
        
        return false;
    }
    
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }
    
    @Override
    public boolean isEnabled() {
        return enabled;
    }
    
    // 锁定账户
    public void lock(int minutes) {
        this.locked = true;
        this.lockedTime = LocalDateTime.now();
        this.lockExpirationTime = LocalDateTime.now().plusMinutes(minutes);
    }
    
    // 解锁账户
    public void unlock() {
        this.locked = false;
        this.lockedTime = null;
        this.lockExpirationTime = null;
    }
    
    // 其他方法和字段...
}
```

**方法 2: 使用服务层实现**
```java
@Service
public class AccountService {
    
    @Autowired
    private UserRepository userRepository;
    
    /**
     * 锁定账户
     * 
     * @param username 用户名
     * @param minutes 锁定时长（分钟）
     */
    public void lockAccount(String username, int minutes) {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
        
        user.setLocked(true);
        user.setLockedTime(LocalDateTime.now());
        user.setLockExpirationTime(LocalDateTime.now().plusMinutes(minutes));
        
        userRepository.save(user);
    }
    
    /**
     * 解锁账户
     * 
     * @param username 用户名
     */
    public void unlockAccount(String username) {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
        
        user.setLocked(false);
        user.setLockedTime(null);
        user.setLockExpirationTime(null);
        
        userRepository.save(user);
    }
    
    /**
     * 检查账户是否被锁定
     * 
     * @param username 用户名
     * @return 如果锁定返回 true
     */
    public boolean isAccountLocked(String username) {
        return userRepository.findByUsername(username)
            .map(User::isAccountNonLocked)
            .orElse(false);
    }
}
```

**Q4: 如何实现多因素认证（MFA）？**

A: 多因素认证需要结合多种认证方式，以下是实现思路：

**1. 实现两步验证**
```java
@Configuration
@EnableWebSecurity
public class TwoFactorAuthConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/login", "/login/verify").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(new TwoFactorAuthenticationFilter(), 
                UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
}
```

**2. 创建两步验证过滤器**
```java
public class TwoFactorAuthenticationFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain) 
            throws ServletException, IOException {
        
        // 检查是否是两步验证请求
        if ("/login/verify".equals(request.getRequestURI())) {
            // 验证两步验证码
            String code = request.getParameter("code");
            String username = (String) request.getSession().getAttribute("username");
            
            if (isValidTwoFactorCode(username, code)) {
                // 验证成功，设置认证信息
                UserDetails userDetails = userDetailsService.loadUserByUsername(username);
                Authentication authentication = new UsernamePasswordAuthenticationToken(
                    userDetails, null, userDetails.getAuthorities());
                SecurityContextHolder.getContext().setAuthentication(authentication);
                
                response.sendRedirect("/home");
                return;
            } else {
                // 验证失败
                response.sendRedirect("/login?error=invalid_code");
                return;
            }
        }
        
        filterChain.doFilter(request, response);
    }
    
    private boolean isValidTwoFactorCode(String username, String code) {
        // 验证两步验证码的逻辑
        // 例如：验证 Google Authenticator、短信验证码等
        return true;
    }
}
```

**Q5: 如何实现密码过期提醒和强制修改密码？**

A: 可以通过实现 `UserDetails` 接口或使用自定义过滤器来实现：

**1. 在 UserDetails 中实现**
```java
@Entity
public class User implements UserDetails {
    
    private LocalDateTime passwordChangedTime;
    private int passwordValidDays = 90;  // 密码有效期（天）
    
    @Override
    public boolean isCredentialsNonExpired() {
        if (passwordChangedTime == null) {
            return true;
        }
        
        LocalDateTime expirationTime = passwordChangedTime.plusDays(passwordValidDays);
        return LocalDateTime.now().isBefore(expirationTime);
    }
    
    /**
     * 检查密码是否即将过期（例如：7 天内）
     */
    public boolean isPasswordExpiringSoon() {
        if (passwordChangedTime == null) {
            return false;
        }
        
        LocalDateTime warningTime = passwordChangedTime.plusDays(passwordValidDays - 7);
        return LocalDateTime.now().isAfter(warningTime);
    }
    
    /**
     * 获取密码剩余有效天数
     */
    public long getPasswordRemainingDays() {
        if (passwordChangedTime == null) {
            return passwordValidDays;
        }
        
        LocalDateTime expirationTime = passwordChangedTime.plusDays(passwordValidDays);
        return ChronoUnit.DAYS.between(LocalDateTime.now(), expirationTime);
    }
}
```

**2. 实现密码过期检查过滤器**
```java
@Component
public class PasswordExpiryCheckFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain) 
            throws ServletException, IOException {
        
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        
        if (authentication != null && authentication.isAuthenticated()) {
            Object principal = authentication.getPrincipal();
            
            if (principal instanceof User) {
                User user = (User) principal;
                
                // 检查密码是否即将过期
                if (user.isPasswordExpiringSoon() && 
                    !request.getRequestURI().equals("/change-password")) {
                    
                    long remainingDays = user.getPasswordRemainingDays();
                    
                    // 在 Session 中设置警告消息
                    request.getSession().setAttribute("passwordExpiringSoon", true);
                    request.getSession().setAttribute("passwordRemainingDays", remainingDays);
                }
                
                // 检查密码是否已过期
                if (!user.isCredentialsNonExpired() && 
                    !request.getRequestURI().equals("/change-password")) {
                    
                    // 强制跳转到修改密码页面
                    response.sendRedirect("/change-password?expired=true");
                    return;
                }
            }
        }
        
        filterChain.doFilter(request, response);
    }
}
```

#### 5.6 思考题

1. **认证流程优化**:
   - 问题：在高并发场景下，如何优化认证流程的性能？
   - 提示：考虑缓存、异步处理、数据库查询优化

2. **安全性增强**:
   - 问题：如何防止暴力破解攻击？
   - 提示：考虑登录失败次数限制、验证码、IP 黑名单

3. **多设备登录**:
   - 问题：如何实现单用户最大登录数限制？
   - 提示：考虑会话管理、并发控制

4. **审计日志**:
   - 问题：如何记录详细的认证审计日志？
   - 提示：考虑事件监听、日志记录、审计表

5. **自定义认证**:
   - 问题：如何实现基于手机验证码的认证？
   - 提示：考虑自定义 `AuthenticationProvider`、自定义过滤器

---

### 知识点6: 用户详情服务

#### 6.1 UserDetailsService 接口

`UserDetailsService` 是 Spring Security 中加载用户信息的核心接口，它定义了从数据源加载用户信息的标准方式。

**6.1.1 接口定义**

```java
package org.springframework.security.core.userdetails;

/**
 * UserDetailsService 接口
 * 
 * 核心接口，用于从特定数据源加载用户信息
 * 
 * 主要职责：
 * 1. 根据用户名加载用户信息
 * 2. 返回完整的 UserDetails 对象
 * 3. 如果用户不存在，抛出 UsernameNotFoundException
 * 
 * 设计模式：
 * - 策略模式：不同的实现类可以从不同的数据源加载用户
 * - 模板方法模式：子类实现 loadUserByUsername() 方法
 * 
 * 常见实现类：
 * - InMemoryUserDetailsManager: 从内存加载用户
 * - JdbcUserDetailsManager: 从数据库加载用户
 * - LdapUserDetailsManager: 从 LDAP 加载用户
 * 
 * 自定义实现：
 * 通常需要实现此接口以从自定义数据源（如数据库、NoSQL、外部 API）加载用户
 */
public interface UserDetailsService {

    /**
     * 根据用户名加载用户信息
     * 
     * @param username 用户名
     * @return 用户详情（不能为 null）
     * @throws UsernameNotFoundException 如果用户不存在
     */
    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```

**6.1.2 UserDetails 接口**

```java
package org.springframework.security.core.userdetails;

/**
 * UserDetails 接口
 * 
 * 表示核心用户信息
 * 
 * 主要职责：
 * 1. 封装用户的核心信息（用户名、密码、权限）
 * 2. 提供用户状态信息（是否锁定、过期等）
 * 3. 被框架用于认证和授权
 * 
 * 设计模式：
 * - 值对象模式：不可变对象
 * - DTO 模式：数据传输对象
 * 
 * 实现方式：
 * 1. 使用 Spring 提供的 User 类
 * 2. 自定义实现类（通常与实体类集成）
 * 3. 使用 Lombok 简化实现
 */
public interface UserDetails extends Serializable {

    /**
     * 获取用户权限
     * 
     * 注意：不能返回 null
     * 
     * @return 权限集合
     */
    Collection<? extends GrantedAuthority> getAuthorities();

    /**
     * 获取密码
     * 
     * @return 编码后的密码
     */
    String getPassword();

    /**
     * 获取用户名
     * 
     * @return 用户名
     */
    String getUsername();

    /**
     * 账户是否未过期
     * 
     * @return 如果未过期返回 true
     */
    boolean isAccountNonExpired();

    /**
     * 账户是否未锁定
     * 
     * @return 如果未锁定返回 true
     */
    boolean isAccountNonLocked();

    /**
     * 凭证是否未过期
     * 
     * @return 如果未过期返回 true
     */
    boolean isCredentialsNonExpired();

    /**
     * 账户是否启用
     * 
     * @return 如果启用返回 true
     */
    boolean isEnabled();
}
```

#### 6.2 内置实现详解

**6.2.1 InMemoryUserDetailsManager**

```java
package org.springframework.security.provisioning;

/**
 * InMemoryUserDetailsManager
 * 
 * 从内存中管理用户信息
 * 
 * 主要特点：
 * 1. 用户信息存储在内存中
 * 2. 应用重启后数据丢失
 * 3. 适合开发和测试环境
 * 4. 不适合生产环境
 * 
 * 使用场景：
 * - 原型开发
 * - 单元测试
 * - 演示系统
 */
public class InMemoryUserDetailsManager implements UserDetailsManager, UserDetailsService {
    
    /**
     * 用户映射表
     * 
     * Key: 用户名（小写）
     * Value: MutableUserDetails 对象
     */
    private final Map<String, MutableUserDetails> users = new HashMap<>();

    /**
     * 构造函数（无用户）
     */
    public InMemoryUserDetailsManager() {
    }

    /**
     * 构造函数（单个用户）
     * 
     * @param users 用户详情列表
     */
    public InMemoryUserDetailsManager(UserDetails... users) {
        for (UserDetails user : users) {
            createUser(user);
        }
    }

    /**
     * 构造函数（用户列表）
     * 
     * @param users 用户详情集合
     */
    public InMemoryUserDetailsManager(Collection<UserDetails> users) {
        for (UserDetails user : users) {
            createUser(user);
        }
    }

    /**
     * 根据用户名加载用户
     * 
     * @param username 用户名
     * @return 用户详情
     * @throws UsernameNotFoundException 用户不存在
     */
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 用户名转换为小写（不区分大小写）
        String key = username.toLowerCase();
        
        UserDetails user = this.users.get(key);
        
        if (user == null) {
            throw new UsernameNotFoundException(username);
        }
        
        return new User(user.getUsername(), user.getPassword(), user.getAuthorities());
    }

    /**
     * 创建用户
     * 
     * @param user 用户详情
     */
    @Override
    public void createUser(UserDetails user) {
        Assert.notNull(user, "User must not be null");
        
        String key = user.getUsername().toLowerCase();
        this.users.put(key, new MutableUserDetails(user));
    }

    /**
     * 更新用户
     * 
     * @param user 用户详情
     */
    @Override
    public void updateUser(UserDetails user) {
        Assert.notNull(user, "User must not be null");
        
        String key = user.getUsername().toLowerCase();
        this.users.put(key, new MutableUserDetails(user));
    }

    /**
     * 删除用户
     * 
     * @param username 用户名
     */
    @Override
    public void deleteUser(String username) {
        Assert.notNull(username, "Username must not be null");
        
        String key = username.toLowerCase();
        this.users.remove(key);
    }

    /**
     * 修改密码
     * 
     * @param oldPassword 旧密码
     * @param newPassword 新密码
     */
    @Override
    public void changePassword(String oldPassword, String newPassword) {
        // 获取当前用户
        Authentication currentUser = SecurityContextHolder.getContext()
            .getAuthentication();
        
        if (currentUser == null) {
            throw new AuthenticationCredentialsNotFoundException(
                "No authenticated user to change password for");
        }
        
        String username = currentUser.getName();
        
        // 加载用户
        UserDetails user = loadUserByUsername(username);
        
        // 验证旧密码
        if (!user.getPassword().equals(oldPassword)) {
            throw new BadCredentialsException("Old password is incorrect");
        }
        
        // 更新密码
        MutableUserDetails mutableUser = this.users.get(username.toLowerCase());
        mutableUser.setPassword(newPassword);
    }

    /**
     * 检查用户是否存在
     * 
     * @param username 用户名
     * @return 如果存在返回 true
     */
    @Override
    public boolean userExists(String username) {
        Assert.notNull(username, "Username must not be null");
        
        String key = username.toLowerCase();
        return this.users.containsKey(key);
    }
}
```

**使用示例**:

```java
@Bean
public UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
    // 创建用户
    UserDetails user = User.builder()
        .username("user")
        .password(passwordEncoder.encode("123456"))
        .roles("USER")
        .build();

    UserDetails admin = User.builder()
        .username("admin")
        .password(passwordEncoder.encode("admin123"))
        .roles("ADMIN", "USER")
        .build();

    // 创建 InMemoryUserDetailsManager
    return new InMemoryUserDetailsManager(user, admin);
}
```

**6.2.2 JdbcUserDetailsManager**

```java
package org.springframework.security.provisioning;

/**
 * JdbcUserDetailsManager
 * 
 * 从数据库管理用户信息
 * 
 * 主要特点：
 * 1. 使用 JDBC 访问数据库
 * 2. 用户信息持久化存储
 * 3. 支持标准的用户/权限表结构
 * 4. 支持自定义表结构
 * 
 * 使用场景：
 * - 生产环境
 * - 需要持久化存储
 * - 需要动态管理用户
 * 
 * 默认表结构：
 * - users: 用户表（username, password, enabled）
 * - authorities: 权限表（username, authority）
 * - groups: 组表（id, group_name）
 */
public class JdbcUserDetailsManager extends JdbcDaoImpl implements UserDetailsManager, GroupManager {
    
    /**
     * 创建用户的 SQL
     */
    public static final String DEF_CREATE_USER_SQL = 
        "insert into users (username, password, enabled) values (?, ?, ?)";
    
    /**
     * 删除用户的 SQL
     */
    public static final String DEF_DELETE_USER_SQL = 
        "delete from users where username = ?";
    
    /**
     * 更新用户密码的 SQL
     */
    public static final String DEF_UPDATE_USER_SQL = 
        "update users set password = ?, enabled = ? where username = ?";
    
    /**
     * 检查用户是否存在的 SQL
     */
    public static final String DEF_USER_EXISTS_SQL = 
        "select username from users where username = ?";
    
    /**
     * 更改密码的 SQL
     */
    public static final String DEF_CHANGE_PASSWORD_SQL = 
        "update users set password = ? where username = ?";
    
    /**
     * 创建用户
     * 
     * @param user 用户详情
     */
    @Override
    public void createUser(UserDetails user) {
        validateUserDetails(user);
        
        getJdbcTemplate().update(DEF_CREATE_USER_SQL,
            user.getUsername(),
            user.getPassword(),
            user.isEnabled() ? 1 : 0);
        
        if (user.getAuthorities() != null && !user.getAuthorities().isEmpty()) {
            for (GrantedAuthority auth : user.getAuthorities()) {
                insertUserAuthority(user.getUsername(), auth.getAuthority());
            }
        }
    }

    /**
     * 更新用户
     * 
     * @param user 用户详情
     */
    @Override
    public void updateUser(UserDetails user) {
        validateUserDetails(user);
        
        getJdbcTemplate().update(DEF_UPDATE_USER_SQL,
            user.getPassword(),
            user.isEnabled() ? 1 : 0,
            user.getUsername());
        
        // 删除旧的权限
        deleteUserAuthorities(user.getUsername());
        
        // 添加新的权限
        if (user.getAuthorities() != null && !user.getAuthorities().isEmpty()) {
            for (GrantedAuthority auth : user.getAuthorities()) {
                insertUserAuthority(user.getUsername(), auth.getAuthority());
            }
        }
    }

    /**
     * 删除用户
     * 
     * @param username 用户名
     */
    @Override
    public void deleteUser(String username) {
        deleteUserAuthorities(username);
        getJdbcTemplate().update(DEF_DELETE_USER_SQL, username);
    }

    /**
     * 修改密码
     * 
     * @param oldPassword 旧密码
     * @param newPassword 新密码
     */
    @Override
    public void changePassword(String oldPassword, String newPassword) {
        Authentication currentUser = SecurityContextHolder.getContext()
            .getAuthentication();
        
        if (currentUser == null) {
            throw new AuthenticationCredentialsNotFoundException(
                "No authenticated user to change password for");
        }
        
        String username = currentUser.getName();
        
        getJdbcTemplate().update(DEF_CHANGE_PASSWORD_SQL, newPassword, username);
    }

    /**
     * 检查用户是否存在
     * 
     * @param username 用户名
     * @return 如果存在返回 true
     */
    @Override
    public boolean userExists(String username) {
        List<String> users = getJdbcTemplate().queryForList(
            DEF_USER_EXISTS_SQL, new Object[]{username}, String.class);
        
        return !users.isEmpty();
    }
}
```

**使用示例**:

```java
@Bean
public UserDetailsService userDetailsService(DataSource dataSource, PasswordEncoder passwordEncoder) {
    JdbcUserDetailsManager manager = new JdbcUserDetailsManager(dataSource);
    
    // 创建用户
    UserDetails user = User.builder()
        .username("user")
        .password(passwordEncoder.encode("123456"))
        .roles("USER")
        .build();

    UserDetails admin = User.builder()
        .username("admin")
        .password(passwordEncoder.encode("admin123"))
        .roles("ADMIN", "USER")
        .build();

    // 检查用户是否存在，不存在则创建
    if (!manager.userExists(user.getUsername())) {
        manager.createUser(user);
    }
    
    if (!manager.userExists(admin.getUsername())) {
        manager.createUser(admin);
    }
    
    return manager;
}
```

**数据库表结构**:

```sql
-- 用户表
CREATE TABLE users (
    username VARCHAR(50) NOT NULL PRIMARY KEY,
    password VARCHAR(100) NOT NULL,
    enabled BOOLEAN NOT NULL
);

-- 权限表
CREATE TABLE authorities (
    username VARCHAR(50) NOT NULL,
    authority VARCHAR(50) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username),
    UNIQUE (username, authority)
);

-- 组表（可选）
CREATE TABLE groups (
    id BIGINT NOT NULL PRIMARY KEY,
    group_name VARCHAR(50) NOT NULL
);

CREATE TABLE group_authorities (
    group_id BIGINT NOT NULL,
    authority VARCHAR(50) NOT NULL,
    FOREIGN KEY (group_id) REFERENCES groups(id)
);

CREATE TABLE group_members (
    group_id BIGINT NOT NULL,
    username VARCHAR(50) NOT NULL,
    FOREIGN KEY (group_id) REFERENCES groups(id),
    FOREIGN KEY (username) REFERENCES users(username)
);
```

#### 6.3 自定义 UserDetailsService

在实际应用中，通常需要自定义 `UserDetailsService` 以从数据库或其他数据源加载用户信息。

**6.3.1 实现自定义 UserDetailsService**

```java
package com.example.security.service;

import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

/**
 * 自定义 UserDetailsService
 * 
 * 从数据库加载用户信息
 */
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private PasswordEncoder passwordEncoder;

    /**
     * 根据用户名加载用户信息
     * 
     * @param username 用户名
     * @return 用户详情
     * @throws UsernameNotFoundException 用户不存在
     */
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 1. 从数据库查询用户
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException(
                "User not found: " + username));
        
        // 2. 将数据库用户转换为 Spring Security 的 UserDetails
        return org.springframework.security.core.userdetails.User.builder()
            .username(user.getUsername())
            .password(user.getPassword())
            .roles(user.getRoles().toArray(new String[0]))
            .accountLocked(!user.isActive())
            .disabled(!user.isEnabled())
            .accountExpired(user.isAccountExpired())
            .credentialsExpired(user.isCredentialsExpired())
            .build();
    }
}
```

**6.3.2 自定义 UserDetails**

```java
package com.example.security.model;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.Collection;
import java.util.Set;
import java.util.stream.Collectors;

/**
 * 自定义 UserDetails 实现
 * 
 * 同时也是 JPA 实体类
 */
@Entity
@Table(name = "users")
public class User implements UserDetails {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String password;
    
    @Column(nullable = false)
    private boolean enabled = true;
    
    @Column(nullable = false)
    private boolean active = true;
    
    @Column(nullable = false)
    private boolean accountExpired = false;
    
    @Column(nullable = false)
    private boolean credentialsExpired = false;
    
    @Column(nullable = false)
    private boolean locked = false;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column
    private LocalDateTime lastLoginAt;
    
    @Column
    private LocalDateTime passwordChangedAt;
    
    @Column
    private Integer passwordValidDays = 90;
    
    @ElementCollection(fetch = FetchType.EAGER)
    @CollectionTable(name = "user_roles", joinColumns = @JoinColumn(name = "user_id"))
    @Column(name = "role")
    private Set<String> roles;

    /**
     * 获取权限
     * 
     * 将角色转换为权限
     */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return roles.stream()
            .map(role -> new SimpleGrantedAuthority("ROLE_" + role))
            .collect(Collectors.toList());
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
        return !accountExpired;
    }

    @Override
    public boolean isAccountNonLocked() {
        return !locked;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        if (credentialsExpired) {
            return false;
        }
        
        // 检查密码是否过期
        if (passwordChangedAt == null) {
            return true;
        }
        
        LocalDateTime expirationTime = passwordChangedAt.plusDays(passwordValidDays);
        return LocalDateTime.now().isBefore(expirationTime);
    }

    @Override
    public boolean isEnabled() {
        return enabled;
    }
    
    /**
     * 获取密码剩余有效天数
     */
    public long getPasswordRemainingDays() {
        if (passwordChangedAt == null) {
            return passwordValidDays;
        }
        
        LocalDateTime expirationTime = passwordChangedAt.plusDays(passwordValidDays);
        return java.time.temporal.ChronoUnit.DAYS.between(LocalDateTime.now(), expirationTime);
    }
    
    /**
     * 检查密码是否即将过期（7 天内）
     */
    public boolean isPasswordExpiringSoon() {
        return getPasswordRemainingDays() <= 7;
    }
    
    /**
     * 锁定账户
     * 
     * @param minutes 锁定时长（分钟）
     */
    public void lock(int minutes) {
        this.locked = true;
    }
    
    /**
     * 解锁账户
     */
    public void unlock() {
        this.locked = false;
    }

    /**
     * 修改密码
     * 
     * @param newPassword 新密码
     */
    public void changePassword(String newPassword) {
        this.password = newPassword;
        this.passwordChangedAt = LocalDateTime.now();
        this.credentialsExpired = false;
    }

    /**
     * 更新最后登录时间
     */
    public void updateLastLoginTime() {
        this.lastLoginAt = LocalDateTime.now();
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public void setEnabled(boolean enabled) {
        this.enabled = enabled;
    }

    public boolean isActive() {
        return active;
    }

    public void setActive(boolean active) {
        this.active = active;
    }

    public boolean isAccountExpired() {
        return accountExpired;
    }

    public void setAccountExpired(boolean accountExpired) {
        this.accountExpired = accountExpired;
    }

    public boolean isCredentialsExpired() {
        return credentialsExpired;
    }

    public void setCredentialsExpired(boolean credentialsExpired) {
        this.credentialsExpired = credentialsExpired;
    }

    public boolean isLocked() {
        return locked;
    }

    public void setLocked(boolean locked) {
        this.locked = locked;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public void setCreatedAt(LocalDateTime createdAt) {
        this.createdAt = createdAt;
    }

    public LocalDateTime getLastLoginAt() {
        return lastLoginAt;
    }

    public void setLastLoginAt(LocalDateTime lastLoginAt) {
        this.lastLoginAt = lastLoginAt;
    }

    public LocalDateTime getPasswordChangedAt() {
        return passwordChangedAt;
    }

    public void setPasswordChangedAt(LocalDateTime passwordChangedAt) {
        this.passwordChangedAt = passwordChangedAt;
    }

    public Integer getPasswordValidDays() {
        return passwordValidDays;
    }

    public void setPasswordValidDays(Integer passwordValidDays) {
        this.passwordValidDays = passwordValidDays;
    }

    public Set<String> getRoles() {
        return roles;
    }

    public void setRoles(Set<String> roles) {
        this.roles = roles;
    }
}
```

#### 6.4 用户缓存

用户缓存可以显著提高性能，避免每次认证都查询数据库。

**6.4.1 UserCache 接口**

```java
package org.springframework.security.core.userdetails.cache;

/**
 * UserCache 接口
 * 
 * 缓存用户信息
 * 
 * 主要实现：
 * - NullUserCache: 不缓存
 * - EhCacheBasedUserCache: 使用 Ehcache
 * - SpringCacheBasedUserCache: 使用 Spring Cache
 */
public interface UserCache {

    /**
     * 根据用户名从缓存获取用户
     * 
     * @param username 用户名
     * @return 用户详情，如果不存在返回 null
     */
    UserDetails getUserFromCache(String username);

    /**
     * 将用户放入缓存
     * 
     * @param user 用户详情
     */
    void putUserInCache(UserDetails user);

    /**
     * 从缓存移除用户
     * 
     * @param username 用户名
     */
    void removeUserFromCache(String username);
}
```

**6.4.2 配置用户缓存**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    /**
     * 配置用户缓存
     * 
     * @return UserCache 实例
     */
    @Bean
    public UserCache userCache() {
        // 使用 Spring Cache
        Cache cache = cacheManager.getCache("userCache");
        return new SpringCacheBasedUserCache(cache);
    }

    /**
     * 配置缓存管理器
     * 
     * @return CacheManager 实例
     */
    @Bean
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager("userCache");
    }

    /**
     * 配置 AuthenticationProvider
     * 
     * @param userDetailsService 用户详情服务
     * @param passwordEncoder 密码编码器
     * @param userCache 用户缓存
     * @return AuthenticationProvider 实例
     */
    @Bean
    public AuthenticationProvider authenticationProvider(
            UserDetailsService userDetailsService,
            PasswordEncoder passwordEncoder,
            UserCache userCache) {
        
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        provider.setPasswordEncoder(passwordEncoder);
        provider.setUserCache(userCache);
        
        return provider;
    }
}
```

#### 6.5 常见问题（FAQ）

**Q1: 如何在 UserDetailsService 中注入其他 Service？**

A: 可以使用 `@Autowired` 或构造函数注入：

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    private final UserRepository userRepository;
    private final RoleService roleService;
    private final LoginAttemptService loginAttemptService;
    
    @Autowired
    public CustomUserDetailsService(UserRepository userRepository,
                                   RoleService roleService,
                                   LoginAttemptService loginAttemptService) {
        this.userRepository = userRepository;
        this.roleService = roleService;
        this.loginAttemptService = loginAttemptService;
    }
    
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 使用注入的 Service
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));
        
        Set<String> roles = roleService.getUserRoles(user.getId());
        user.setRoles(roles);
        
        return user;
    }
}
```

**Q2: 如何实现用户登录时的额外逻辑（如更新最后登录时间）？**

A: 可以通过事件监听器或自定义 `AuthenticationProvider` 实现：

**方法 1: 使用事件监听器**
```java
@Component
public class LoginListener {
    
    @Autowired
    private UserRepository userRepository;
    
    @EventListener
    public void onAuthenticationSuccess(AuthenticationSuccessEvent event) {
        String username = event.getAuthentication().getName();
        
        User user = userRepository.findByUsername(username).orElse(null);
        if (user != null) {
            user.updateLastLoginTime();
            userRepository.save(user);
        }
    }
}
```

**方法 2: 自定义 AuthenticationProvider**
```java
@Component
public class CustomAuthenticationProvider extends DaoAuthenticationProvider {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        Authentication auth = super.authenticate(authentication);
        
        if (auth != null && auth.isAuthenticated()) {
            String username = auth.getName();
            User user = userRepository.findByUsername(username).orElse(null);
            if (user != null) {
                user.updateLastLoginTime();
                userRepository.save(user);
            }
        }
        
        return auth;
    }
}
```

**Q3: 如何实现用户状态的动态检查（如从数据库读取）？**

A: 在 `UserDetails` 实现中实现动态检查：

```java
@Entity
public class User implements UserDetails {
    
    @Column(nullable = false)
    private boolean enabled = true;
    
    @Column(nullable = false)
    private boolean locked = false;
    
    @Column(nullable = false)
    private boolean accountExpired = false;
    
    @Column(nullable = false)
    private boolean credentialsExpired = false;
    
    // 这些方法会在每次认证时被调用
    @Override
    public boolean isEnabled() {
        // 从数据库读取最新状态
        return this.enabled;
    }
    
    @Override
    public boolean isAccountNonLocked() {
        // 从数据库读取最新状态
        return !this.locked;
    }
    
    @Override
    public boolean isAccountNonExpired() {
        // 从数据库读取最新状态
        return !this.accountExpired;
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        // 从数据库读取最新状态
        return !this.credentialsExpired;
    }
}
```

**Q4: 如何实现多租户系统的用户认证？**

A: 可以通过自定义 `UserDetailsService` 实现：

```java
@Service
public class MultiTenantUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private TenantContextHolder tenantContextHolder;
    
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 获取当前租户 ID
        String tenantId = tenantContextHolder.getTenantId();
        
        // 根据租户 ID 和用户名查询用户
        User user = userRepository.findByTenantIdAndUsername(tenantId, username)
            .orElseThrow(() -> new UsernameNotFoundException(
                "User not found: " + username + " in tenant: " + tenantId));
        
        return user;
    }
}
```

**Q5: 如何实现用户的批量导入？**

A: 可以使用 `JdbcUserDetailsManager` 或自定义实现：

```java
@Service
public class UserImportService {
    
    @Autowired
    private JdbcUserDetailsManager userDetailsManager;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    /**
     * 批量导入用户
     * 
     * @param users 用户列表
     */
    public void importUsers(List<UserImportDto> users) {
        for (UserImportDto dto : users) {
            // 检查用户是否存在
            if (!userDetailsManager.userExists(dto.getUsername())) {
                // 创建用户
                UserDetails user = User.builder()
                    .username(dto.getUsername())
                    .password(passwordEncoder.encode(dto.getPassword()))
                    .roles(dto.getRoles().toArray(new String[0]))
                    .enabled(dto.isEnabled())
                    .build();
                
                userDetailsManager.createUser(user);
            }
        }
    }
    
    /**
     * 从 CSV 文件导入用户
     * 
     * @param file CSV 文件
     */
    public void importUsersFromCsv(MultipartFile file) throws IOException {
        List<UserImportDto> users = new ArrayList<>();
        
        BufferedReader reader = new BufferedReader(new InputStreamReader(file.getInputStream()));
        String line;
        
        while ((line = reader.readLine()) != null) {
            String[] parts = line.split(",");
            
            UserImportDto dto = new UserImportDto();
            dto.setUsername(parts[0]);
            dto.setPassword(parts[1]);
            dto.setRoles(Arrays.asList(parts[2].split(";")));
            dto.setEnabled(Boolean.parseBoolean(parts[3]));
            
            users.add(dto);
        }
        
        importUsers(users);
    }
}
```

#### 6.6 思考题

1. **性能优化**:
   - 问题：在用户数量很大的情况下，如何优化 `UserDetailsService` 的性能？
   - 提示：考虑缓存、数据库索引、分页加载

2. **数据源多样性**:
   - 问题：如何实现从多个数据源加载用户信息（如数据库 + LDAP）？
   - 提示：考虑组合模式、代理模式

3. **安全性**:
   - 问题：如何防止用户枚举攻击（通过登录响应判断用户是否存在）？
   - 提示：考虑统一错误消息、延迟响应

4. **扩展性**:
   - 问题：如何设计一个灵活的权限模型（支持 RBAC、ABAC 等）？
   - 提示：考虑权限的粒度、继承关系、动态权限

5. **监控和审计**:
   - 问题：如何监控 `UserDetailsService` 的性能和使用情况？
   - 提示：考虑指标收集、日志记录、性能分析

---

### 知识点7: 密码编码机制

#### 7.1 密码编码概述

密码编码是安全系统中的关键环节，用于保护用户密码的安全存储。

**7.1.1 为什么不能明文存储密码？**

1. **数据库泄露风险**: 如果数据库被攻击，明文密码会直接暴露
2. **内部人员威胁**: 管理员可能访问到密码
3. **密码重用**: 用户经常在多个网站使用相同密码
4. **合规要求**: 许多法律法规（如 GDPR、PCI DSS）要求加密存储密码

**7.1.2 密码哈希 vs 加密**

| 特性 | 哈希（Hashing） | 加密（Encryption） |
|------|----------------|-------------------|
| **可逆性** | 不可逆（单向） | 可逆（需要密钥） |
| **用途** | 密码存储 | 数据传输 |
| **密钥** | 不需要密钥 | 需要密钥 |
| **算法** | MD5, SHA-256, BCrypt | AES, RSA |
| **安全性** | 高（如果使用正确的算法） | 取决于密钥管理 |

**结论**: 密码存储应该使用**哈希**，而不是加密。

**7.1.3 密码哈希的最佳实践**

1. **使用自适应哈希函数**: 如 BCrypt、SCrypt、Argon2
2. **加盐**: 每个密码使用唯一的随机盐
3. **慢哈希**: 哈希函数应该足够慢，以抵御暴力破解
4. **避免过时算法**: 不使用 MD5、SHA-1 等

#### 7.2 PasswordEncoder 接口

```java
package org.springframework.security.crypto.password;

/**
 * PasswordEncoder 接口
 * 
 * 密码编码服务的接口
 * 
 * 主要职责：
 * 1. 对原始密码进行编码
 * 2. 验证原始密码与编码后的密码是否匹配
 * 
 * 设计模式：
 * - 策略模式：不同的实现类使用不同的编码算法
 * 
 * 常见实现类：
 * - BCryptPasswordEncoder: BCrypt 算法（推荐）
 * - Pbkdf2PasswordEncoder: PBKDF2 算法
 * - SCryptPasswordEncoder: SCrypt 算法
 * - Argon2PasswordEncoder: Argon2 算法
 * - NoOpPasswordEncoder: 不编码（仅用于测试）
 * - StandardPasswordEncoder: SHA-256 算法
 */
public interface PasswordEncoder {

    /**
     * 对原始密码进行编码
     * 
     * @param rawPassword 原始密码
     * @return 编码后的密码
     */
    String encode(CharSequence rawPassword);

    /**
     * 验证原始密码与编码后的密码是否匹配
     * 
     * @param rawPassword 原始密码（用户输入的密码）
     * @param encodedPassword 编码后的密码（数据库中存储的密码）
     * @return 如果匹配返回 true
     */
    boolean matches(CharSequence rawPassword, String encodedPassword);
}
```

#### 7.3 BCryptPasswordEncoder 详解

**7.3.1 BCrypt 算法介绍**

BCrypt 是一种基于 Blowfish 密码算法的自适应哈希函数，专门用于密码存储。

**特点**:
1. **自适应**: 可以调整计算强度（4-31）
2. **自动加盐**: 每次哈希都会生成随机盐
3. **抗 GPU/ASIC**: 算法设计使得 GPU/ASIC 加速效果有限

**7.3.2 BCryptPasswordEncoder 源码分析**

```java
package org.springframework.security.crypto.bcrypt;

/**
 * BCryptPasswordEncoder
 * 
 * BCrypt 密码编码器
 * 
 * 主要特点：
 * 1. 使用 BCrypt 算法
 * 2. 自动加盐
 * 3. 可调整计算强度（4-31）
 * 4. 每次生成的哈希值都不同
 * 
 * 格式：$2a$[强度]$[22位盐][31位哈希值]
 * 
 * 示例：$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy
 * 
 * 解析：
 * - $2a$: BCrypt 版本
 * - 10: 计算强度（2^10 次迭代）
 * - N9qo8uLOickgx2ZMRZoMye: 22 位盐
 * - IjZAgcfl7p92ldGxad68LJZdL17lhWy: 31 位哈希值
 */
public class BCryptPasswordEncoder implements PasswordEncoder {
    
    /**
     * BCrypt 版本
     */
    private final BCryptPasswordEncoder.BCryptVersion version;
    
    /**
     * 计算强度（2 的幂次方）
     * 
     * 有效范围：4-31
     * 推荐值：10-12
     */
    private final int strength;
    
    /**
     * 随机数生成器
     */
    private final SecureRandom random;

    /**
     * 构造函数（默认强度 10）
     */
    public BCryptPasswordEncoder() {
        this(-1);
    }

    /**
     * 构造函数（自定义强度）
     * 
     * @param strength 计算强度（4-31）
     */
    public BCryptPasswordEncoder(int strength) {
        this(BCryptPasswordEncoder.BCryptVersion.$2A, strength, null);
    }

    /**
     * 构造函数（自定义强度和随机数生成器）
     * 
     * @param strength 计算强度
     * @param random 随机数生成器
     */
    public BCryptPasswordEncoder(int strength, SecureRandom random) {
        this(BCryptPasswordEncoder.BCryptVersion.$2A, strength, random);
    }

    /**
     * 核心方法：对原始密码进行编码
     * 
     * @param rawPassword 原始密码
     * @return 编码后的密码
     */
    @Override
    public String encode(CharSequence rawPassword) {
        if (rawPassword == null) {
            throw new IllegalArgumentException("rawPassword cannot be null");
        }
        
        // 1. 生成盐
        String salt;
        if (this.random != null) {
            salt = BCrypt.gensalt(this.version.getVersion2a(), this.strength, this.random);
        } 
        else {
            salt = BCrypt.gensalt(this.version.getVersion2a(), this.strength);
        }
        
        // 2. 哈希密码
        return BCrypt.hashpw(rawPassword.toString(), salt);
    }

    /**
     * 核心方法：验证密码
     * 
     * @param rawPassword 原始密码（用户输入）
     * @param encodedPassword 编码后的密码（数据库）
     * @return 如果匹配返回 true
     */
    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        if (rawPassword == null) {
            throw new IllegalArgumentException("rawPassword cannot be null");
        }
        
        if (encodedPassword == null || encodedPassword.length() == 0) {
            logger.warn("Empty encoded password");
            return false;
        }
        
        // 1. 检查编码后的密码格式
        if (!this.version.prefixMatches(encodedPassword)) {
            this.logger.warn("Encoded password does not look like BCrypt");
            return false;
        }
        
        try {
            // 2. 验证密码
            //    BCrypt.checkpw() 会从 encodedPassword 中提取盐，
            //    然后使用相同的盐哈希 rawPassword，
            //    最后比较哈希值是否相等
            return BCrypt.checkpw(rawPassword.toString(), encodedPassword);
        } 
        catch (Exception ex) {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Failed to verify password", ex);
            }
            return false;
        }
    }
}
```

**7.3.3 BCrypt 强度与性能**

| 强度 | 迭代次数 | 哈希时间（约） | 安全性 |
|------|---------|---------------|--------|
| 4 | 2^4 = 16 | 0.05 秒 | 低 |
| 8 | 2^8 = 256 | 0.1 秒 | 中 |
| 10 | 2^10 = 1024 | 0.6 秒 | 高 |
| 12 | 2^12 = 4096 | 2.5 秒 | 很高 |
| 14 | 2^14 = 16384 | 10 秒 | 极高 |

**推荐**:
- 普通应用：强度 10
- 高安全需求：强度 12
- 低性能设备：强度 8

#### 7.4 其他密码编码器

**7.4.1 Pbkdf2PasswordEncoder**

```java
package org.springframework.security.crypto.password;

/**
 * Pbkdf2PasswordEncoder
 * 
 * PBKDF2（Password-Based Key Derivation Function 2）密码编码器
 * 
 * 特点：
 * 1. 基于 HMAC 的迭代哈希
 * 2. 可以调整迭代次数和盐长度
 * 3. 广泛支持（RFC 2898）
 * 
 * 适用场景：
 * - 需要 FIPS 合规
 * - 需要标准化算法
 */
public class Pbkdf2PasswordEncoder implements PasswordEncoder {
    
    /**
     * 构造函数
     * 
     * @param secret 密钥（可选）
     * @param iterations 迭代次数（默认 185000）
     * @param hashWidth 哈希宽度（默认 256）
     * @param secretSize 密钥大小
     */
    public Pbkdf2PasswordEncoder(CharSequence secret, int iterations, 
            int hashWidth, int secretSize) {
        // ...
    }

    @Override
    public String encode(CharSequence rawPassword) {
        // ...
    }

    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        // ...
    }
}
```

**使用示例**:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    // 使用 PBKDF2 算法
    return new Pbkdf2PasswordEncoder("", 185000, 256);
}
```

**7.4.2 SCryptPasswordEncoder**

```java
package org.springframework.security.crypto.scrypt;

/**
 * SCryptPasswordEncoder
 * 
 * SCrypt 密码编码器
 * 
 * 特点：
 * 1. 内存密集型算法
 * 2. 抗 GPU/ASIC 攻击
 * 3. 可调整 CPU/内存成本
 * 
 * 适用场景：
 * - 需要抗 GPU 攻击
 * - 高安全需求
 */
public class SCryptPasswordEncoder implements PasswordEncoder {
    
    /**
     * 构造函数
     * 
     * @param cpuCost CPU 成本（默认 16384）
     * @param memoryCost 内存成本（默认 8）
     * @param parallelization 并行度（默认 1）
     * @param keyLength 密钥长度（默认 32）
     * @param saltLength 盐长度（默认 64）
     */
    public SCryptPasswordEncoder(int cpuCost, int memoryCost, 
            int parallelization, int keyLength, int saltLength) {
        // ...
    }

    @Override
    public String encode(CharSequence rawPassword) {
        // ...
    }

    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        // ...
    }
}
```

**使用示例**:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    // 使用 SCrypt 算法
    return new SCryptPasswordEncoder(16384, 8, 1, 32, 64);
}
```

**7.4.3 Argon2PasswordEncoder**

```java
package org.springframework.security.crypto.argon2;

/**
 * Argon2PasswordEncoder
 * 
 * Argon2 密码编码器（2015 年密码哈希竞赛获胜者）
 * 
 * 特点：
 * 1. 最新的密码哈希算法
 * 2. 抗 GPU/ASIC 攻击
 * 3. 三种模式：Argon2d、Argon2i、Argon2id
 * 
 * 适用场景：
 * - 最高安全需求
 * - 现代系统
 */
public class Argon2PasswordEncoder implements PasswordEncoder {
    
    /**
     * 构造函数
     * 
     * @param iterations 迭代次数
     * @param memory 内存成本（KB）
     * @param parallelism 并行度
     * @param type Argon2 类型
     * @param saltLength 盐长度
     * @param hashLength 哈希长度
     */
    public Argon2PasswordEncoder(int iterations, int memory, int parallelism, 
            Argon2PasswordEncoder.Argon2Type type, int saltLength, int hashLength) {
        // ...
    }

    @Override
    public String encode(CharSequence rawPassword) {
        // ...
    }

    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        // ...
    }
}
```

**使用示例**:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    // 使用 Argon2id 算法
    return new Argon2PasswordEncoder(10, 65536, 1, 
        Argon2PasswordEncoder.Argon2Types.ARGON2id, 16, 32);
}
```

#### 7.5 密码编码器迁移

在现有系统中，可能需要从旧的密码编码器迁移到新的编码器。

**7.5.1 使用 DelegatingPasswordEncoder**

`DelegatingPasswordEncoder` 支持多种密码编码器并存，便于迁移。

```java
@Configuration
public class PasswordEncoderConfig {
    
    /**
     * 创建 DelegatingPasswordEncoder
     * 
     * 支持多种密码编码格式
     * 
     * 格式：{encoderId}encodedPassword
     * 
     * 示例：
     * - {bcrypt}$2a$10$...
     * - {pbkdf2}...
     * - {sha256}...
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        Map<String, PasswordEncoder> encoders = new HashMap<>();
        
        // 注册各种编码器
        encoders.put("bcrypt", new BCryptPasswordEncoder(10));
        encoders.put("pbkdf2", new Pbkdf2PasswordEncoder("", 185000, 256));
        encoders.put("scrypt", new SCryptPasswordEncoder(16384, 8, 1, 32, 64));
        encoders.put("argon2", new Argon2PasswordEncoder(10, 65536, 1, 
            Argon2PasswordEncoder.Argon2Types.ARGON2id, 16, 32));
        encoders.put("sha256", new StandardPasswordEncoder());
        
        // 使用 bcrypt 作为默认编码器
        return new DelegatingPasswordEncoder("bcrypt", encoders);
    }
}
```

**7.5.2 密码迁移策略**

```java
@Service
public class PasswordMigrationService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    /**
     * 迁移用户密码
     * 
     * 检测密码格式，如果是旧格式则重新编码
     * 
     * @param user 用户
     * @param rawPassword 原始密码
     */
    public void migratePasswordIfNeeded(User user, String rawPassword) {
        String encodedPassword = user.getPassword();
        
        // 检查密码格式
        if (!encodedPassword.startsWith("{bcrypt}")) {
            // 旧格式，使用新编码器重新编码
            String newEncodedPassword = passwordEncoder.encode(rawPassword);
            
            // 更新数据库
            user.setPassword(newEncodedPassword);
            userRepository.save(user);
        }
    }
    
    /**
     * 批量迁移密码
     * 
     * 注意：需要用户登录后才能迁移
     */
    @Scheduled(fixedRate = 3600000)  // 每小时执行一次
    public void migratePasswordsBatch() {
        // 找到使用旧格式的用户
        List<User> users = userRepository.findByPasswordNotStartingWith("{bcrypt}");
        
        for (User user : users) {
            // 发送密码重置邮件
            sendPasswordResetEmail(user);
        }
    }
}
```

#### 7.6 常见问题（FAQ）

**Q1: 如何选择合适的密码编码器？**

A: 选择密码编码器时需要考虑以下因素：

| 算法 | 优点 | 缺点 | 适用场景 |
|------|------|------|----------|
| **BCrypt** | 广泛支持、成熟、配置简单 | GPU 加速效果有限 | 大多数应用（推荐） |
| **PBKDF2** | 标准化、FIPS 合规 | CPU 密集 | 需要合规的企业应用 |
| **SCrypt** | 内存密集、抗 GPU | 复杂、需要调优 | 高安全需求 |
| **Argon2** | 最新、最安全 | 支持有限 | 最高安全需求 |

**推荐**:
- 普通应用：BCrypt（强度 10-12）
- 企业应用：PBKDF2（合规需求）
- 高安全应用：SCrypt 或 Argon2

**Q2: BCrypt 的强度应该设置多少？**

A: 强度设置需要平衡安全性和性能：

**推荐值**:
- **强度 10**: 约 0.6 秒，适合大多数应用
- **强度 12**: 约 2.5 秒，适合高安全需求
- **强度 8**: 约 0.1 秒，适合低性能设备

**选择建议**:
1. 测试不同强度在目标硬件上的性能
2. 选择可接受的延迟时间
3. 定期评估（随着硬件性能提升，可能需要增加强度）

**Q3: 如何实现密码过期策略？**

A: 有多种方式实现密码过期：

**方法 1: 在 UserDetails 中实现**
```java
@Entity
public class User implements UserDetails {
    
    @Column
    private LocalDateTime passwordChangedAt;
    
    @Column
    private Integer passwordValidDays = 90;
    
    @Override
    public boolean isCredentialsNonExpired() {
        if (passwordChangedAt == null) {
            return true;
        }
        
        LocalDateTime expirationTime = passwordChangedAt.plusDays(passwordValidDays);
        return LocalDateTime.now().isBefore(expirationTime);
    }
    
    /**
     * 检查密码是否即将过期（7 天内）
     */
    public boolean isPasswordExpiringSoon() {
        if (passwordChangedAt == null) {
            return false;
        }
        
        LocalDateTime warningTime = passwordChangedAt.plusDays(passwordValidDays - 7);
        return LocalDateTime.now().isAfter(warningTime);
    }
}
```

**方法 2: 使用拦截器提醒**
```java
@Component
public class PasswordExpiryInterceptor extends HandlerInterceptorAdapter {
    
    @Override
    public boolean preHandle(HttpServletRequest request, 
            HttpServletResponse response, Object handler) throws Exception {
        
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        
        if (authentication != null && authentication.isAuthenticated()) {
            Object principal = authentication.getPrincipal();
            
            if (principal instanceof User) {
                User user = (User) principal;
                
                if (user.isPasswordExpiringSoon()) {
                    long remainingDays = user.getPasswordRemainingDays();
                    
                    // 设置警告消息
                    request.setAttribute("passwordExpiringSoon", true);
                    request.setAttribute("passwordRemainingDays", remainingDays);
                }
            }
        }
        
        return true;
    }
}
```

**Q4: 如何实现密码强度验证？**

A: 可以通过自定义验证器实现：

```java
@Component
public class PasswordStrengthValidator {
    
    /**
     * 验证密码强度
     * 
     * @param password 密码
     * @return 验证结果
     */
    public ValidationResult validatePassword(String password) {
        List<String> errors = new ArrayList<>();
        
        // 检查长度
        if (password == null || password.length() < 8) {
            errors.add("密码长度至少为 8 位");
        }
        
        // 检查是否包含小写字母
        if (!password.matches(".*[a-z].*")) {
            errors.add("密码必须包含至少一个小写字母");
        }
        
        // 检查是否包含大写字母
        if (!password.matches(".*[A-Z].*")) {
            errors.add("密码必须包含至少一个大写字母");
        }
        
        // 检查是否包含数字
        if (!password.matches(".*\\d.*")) {
            errors.add("密码必须包含至少一个数字");
        }
        
        // 检查是否包含特殊字符
        if (!password.matches(".*[!@#$%^&*()_+\\-=\\[\\]{};':\"\\\\|,.<>/?].*")) {
            errors.add("密码必须包含至少一个特殊字符");
        }
        
        // 检查常见弱密码
        if (isCommonPassword(password)) {
            errors.add("密码过于简单，请使用更复杂的密码");
        }
        
        if (errors.isEmpty()) {
            return ValidationResult.success();
        } else {
            return ValidationResult.failure(errors);
        }
    }
    
    /**
     * 检查是否是常见弱密码
     */
    private boolean isCommonPassword(String password) {
        List<String> commonPasswords = Arrays.asList(
            "password", "12345678", "qwerty", "abc123", "monkey"
        );
        
        return commonPasswords.contains(password.toLowerCase());
    }
}
```

**在 Controller 中使用**:

```java
@RestController
@RequestMapping("/api")
public class UserController {
    
    @Autowired
    private PasswordStrengthValidator passwordValidator;
    
    @PostMapping("/register")
    public ResponseEntity<?> register(@RequestBody RegisterDto dto) {
        // 验证密码强度
        ValidationResult result = passwordValidator.validatePassword(dto.getPassword());
        
        if (!result.isValid()) {
            return ResponseEntity.badRequest().body(result.getErrors());
        }
        
        // 创建用户
        // ...
        
        return ResponseEntity.ok().build();
    }
}
```

**Q5: 如何实现密码历史记录（防止重复使用旧密码）？**

A: 可以通过数据库表或缓存实现：

```java
@Entity
@Table(name = "password_history")
public class PasswordHistory {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "user_id", nullable = false)
    private Long userId;
    
    @Column(nullable = false)
    private String password;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    // Getters and Setters
}
```

```java
@Service
public class PasswordService {
    
    @Autowired
    private PasswordHistoryRepository passwordHistoryRepository;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    /**
     * 检查密码是否在历史记录中
     * 
     * @param userId 用户 ID
     * @param newPassword 新密码
     * @param historySize 历史记录大小（默认 5）
     * @return 如果密码在历史记录中返回 true
     */
    public boolean isPasswordInHistory(Long userId, String newPassword, int historySize) {
        // 获取最近的 N 个密码
        List<PasswordHistory> history = passwordHistoryRepository
            .findTopNByUserIdOrderByCreatedAtDesc(userId, historySize);
        
        // 检查新密码是否与历史密码匹配
        for (PasswordHistory record : history) {
            if (passwordEncoder.matches(newPassword, record.getPassword())) {
                return true;
            }
        }
        
        return false;
    }
    
    /**
     * 保存密码到历史记录
     * 
     * @param userId 用户 ID
     * @param encodedPassword 编码后的密码
     */
    public void savePasswordToHistory(Long userId, String encodedPassword) {
        PasswordHistory history = new PasswordHistory();
        history.setUserId(userId);
        history.setPassword(encodedPassword);
        history.setCreatedAt(LocalDateTime.now());
        
        passwordHistoryRepository.save(history);
        
        // 清理旧的历史记录（保留最近 N 个）
        List<PasswordHistory> allHistory = passwordHistoryRepository
            .findByUserIdOrderByCreatedAtDesc(userId);
        
        if (allHistory.size() > 5) {
            List<PasswordHistory> toDelete = allHistory.subList(5, allHistory.size());
            passwordHistoryRepository.deleteAll(toDelete);
        }
    }
}
```

#### 7.7 思考题

1. **密码编码器选择**:
   - 问题：在一个需要 FIPS 140-2 合规的金融应用中，应该选择哪种密码编码器？
   - 提示：考虑合规性、标准化、性能

2. **性能优化**:
   - 问题：在高并发场景下，如何优化密码验证的性能？
   - 提示：考虑缓存、异步处理、硬件加速

3. **密码重置**:
   - 问题：如何设计一个安全的密码重置流程？
   - 提示：考虑 Token 生成、过期时间、一次性使用

4. **密码策略**:
   - 问题：如何实现动态的密码策略（如管理员与普通用户不同）？
   - 提示：考虑策略模式、配置化、用户角色

5. **合规性**:
   - 问题：如何满足 GDPR 对密码存储的要求？
   - 提示：考虑加密、匿名化、数据最小化

---

### 知识点8: 常用认证方式

#### 8.1 HTTP Basic 认证

HTTP Basic 认证是最简单的认证方式，内置在 HTTP 协议中。

**8.1.1 工作原理**

```
1. 客户端请求受保护资源
2. 服务器返回 401 Unauthorized
   响应头：WWW-Authenticate: Basic realm="Realm"
3. 客户端弹出对话框，输入用户名和密码
4. 客户端重新请求，携带 Authorization 头
   请求头：Authorization: Basic base64(username:password)
5. 服务器验证，成功返回资源
```

**8.1.2 配置示例**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .anyRequest().authenticated()
        )
        .httpBasic(withDefaults());  // 启用 HTTP Basic 认证

    return http.build();
}
```

**8.1.3 优缺点**

| 优点 | 缺点 |
|------|------|
| 简单易实现 | 不安全（Base64 可逆） |
| 所有浏览器支持 | 无法主动登出 |
| 适合 API 认证 | 每次请求都发送凭据 |
| 无状态 | 容易受到中间人攻击 |

**适用场景**:
- 内网应用
- API 认证（配合 HTTPS）
- 简单的认证需求

#### 8.2 HTTP Digest 认证

HTTP Digest 认证是对 Basic 认证的改进，使用 MD5 哈希避免明文传输密码。

**8.1.1 工作原理**

```
1. 客户端请求受保护资源
2. 服务器返回 401 Unauthorized
   响应头：WWW-Authenticate: Digest realm="Realm", nonce="..."
3. 客户端计算哈希值
   response = MD5(MD5(username:realm:password):nonce:MD5(method:uri))
4. 客户端重新请求，携带 Authorization 头
   请求头：Authorization: Digest username="...", response="..."
5. 服务器验证哈希值
```

**8.1.2 配置示例**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .anyRequest().authenticated()
        )
        .httpBasic(httpBasic -> httpBasic
            .authenticationEntryPoint(new DigestAuthenticationEntryPoint())
        );

    return http.build();
}
```

**8.1.3 优缺点**

| 优点 | 缺点 |
|------|------|
| 比 Basic 安全 | 仍然不安全（MD5 已被破解） |
| 密码不明文传输 | 实现复杂 |
| 防止窃听 | 需要服务器存储明文密码 |
| 防止重放攻击（通过 nonce） | 兼容性不如 Basic |

**适用场景**:
- 需要 Basic 认证但稍微提高安全性的场景
- 不推荐用于新应用

#### 8.3 表单登录

表单登录是 Web 应用最常用的认证方式。

**8.3.1 工作原理**

```
1. 用户访问受保护资源
2. Spring Security 重定向到登录页面
3. 用户填写用户名和密码
4. 提交表单到 /login（POST）
5. UsernamePasswordAuthenticationFilter 处理登录请求
6. 认证成功后：
   - 创建 Session
   - 设置 JSESSIONID Cookie
   - 重定向到原始请求页面或默认页面
7. 后续请求携带 JSESSIONID Cookie
8. SessionManagementFilter 从 Session 加载用户信息
```

**8.3.2 配置示例**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/login", "/register").permitAll()
            .anyRequest().authenticated()
        )
        .formLogin(form -> form
            .loginPage("/login")  // 自定义登录页面
            .loginProcessingUrl("/login")  // 登录处理 URL
            .defaultSuccessUrl("/home", true)  // 登录成功后跳转
            .failureUrl("/login?error=true")  // 登录失败后跳转
            .permitAll()
        )
        .logout(logout -> logout
            .logoutUrl("/logout")  // 登出 URL
            .logoutSuccessUrl("/login?logout=true")  // 登出成功后跳转
            .invalidateHttpSession(true)  // 使 Session 失效
            .clearAuthentication(true)  // 清除认证信息
            .permitAll()
        );

    return http.build();
}
```

**8.3.3 自定义登录页面**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <h1>用户登录</h1>
    
    <div th:if="${param.error}">
        <p style="color: red;">用户名或密码错误</p>
    </div>
    
    <div th:if="${param.logout}">
        <p style="color: green;">您已成功登出</p>
    </div>
    
    <form th:action="@{/login}" method="post">
        <div>
            <label>用户名:</label>
            <input type="text" name="username" required autofocus>
        </div>
        
        <div>
            <label>密码:</label>
            <input type="password" name="password" required>
        </div>
        
        <div>
            <input type="checkbox" name="remember-me"> 记住我
        </div>
        
        <button type="submit">登录</button>
    </form>
</body>
</html>
```

#### 8.4 Remember Me 认证

Remember Me 功能允许用户关闭浏览器后仍然保持登录状态。

**8.4.1 工作原理**

**方式 1: 基于哈希的 Token**
```
1. 用户登录时勾选"记住我"
2. 服务器生成 Token
   token = base64(username + ":" + expiryTime + ":" + MD5(username + ":" + expiryTime + ":" + password + ":" + key))
3. 将 Token 保存到 Cookie
   Cookie: remember-me=token; Max-Age=...
4. 下次访问时，从 Cookie 读取 Token
5. 服务器验证 Token
6. 如果验证成功，自动登录
```

**方式 2: 持久化 Token**
```
1. 用户登录时勾选"记住我"
2. 服务器生成随机 Token
   series: 随机字符串
   token: 随机字符串
3. 将 Token 保存到数据库
   persistent_logins 表
4. 将 Token 保存到 Cookie
   Cookie: remember-me=base64(series + ":" + token)
5. 下次访问时，从 Cookie 读取 Token
6. 服务器从数据库查询 Token
7. 如果匹配，自动登录
8. 每次使用后更新 token（防止 Cookie 被窃取）
```

**8.4.2 配置示例**

**基于哈希的 Token（简单但不安全）**:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .rememberMe(rememberMe -> rememberMe
            .key("uniqueAndSecret")  // 密钥（必须唯一）
            .tokenValiditySeconds(86400)  // Token 有效期（1 天）
        );

    return http.build();
}
```

**持久化 Token（推荐）**:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .rememberMe(rememberMe -> rememberMe
            .key("uniqueAndSecret")  // 密钥
            .tokenValiditySeconds(86400)  // Token 有效期
            .rememberMeCookieName("remember-me")  // Cookie 名称
            .rememberMeParameter("remember-me")  // 表单参数名称
            .tokenRepository(persistentTokenRepository())  // Token 持久化
            .userDetailsService(userDetailsService)  // 用户详情服务
        );

    return http.build();
}

@Bean
public PersistentTokenRepository persistentTokenRepository() {
    JdbcTokenRepositoryImpl tokenRepository = new JdbcTokenRepositoryImpl();
    tokenRepository.setDataSource(dataSource);
    return tokenRepository;
}
```

**数据库表结构**:

```sql
CREATE TABLE persistent_logins (
    username VARCHAR(64) NOT NULL,
    series VARCHAR(64) NOT NULL PRIMARY KEY,
    token VARCHAR(64) NOT NULL,
    last_used TIMESTAMP NOT NULL
);
```

#### 8.5 匿名认证

匿名认证允许未登录用户访问受保护资源。

**8.5.1 工作原理**

```
1. 未登录用户访问受保护资源
2. AnonymousAuthenticationFilter 创建匿名认证对象
   principal: "anonymousUser"
   authorities: ROLE_ANONYMOUS
3. 如果资源允许匿名访问，则允许访问
4. 如果资源需要认证，则重定向到登录页面
```

**8.5.2 配置示例**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/public/**").permitAll()  // 允许所有人访问
            .requestMatchers("/anonymous/**").hasRole("ANONYMOUS")  // 只允许匿名用户
            .anyRequest().authenticated()  // 其他请求需要认证
        )
        .anonymous(anonymous -> anonymous
            .key("anonymousKey")  // 密钥
            .principal("anonymousUser")  // 主体标识
            .authorities(new SimpleGrantedAuthority("ROLE_ANONYMOUS"))  // 权限
        );

    return http.build();
}
```

**8.5.3 使用场景**

- 允许匿名用户浏览部分内容
- 区分匿名用户和已登录用户
- 实现"游客"模式

#### 8.6 认证方式对比

| 认证方式 | 优点 | 缺点 | 适用场景 |
|---------|------|------|----------|
| **HTTP Basic** | 简单、标准、无状态 | 不安全、无法主动登出 | 内网 API、简单应用 |
| **HTTP Digest** | 比 Basic 安全 | 仍然不安全、复杂 | 很少使用 |
| **表单登录** | 友好、安全、灵活 | 需要 Session、有状态 | Web 应用（推荐） |
| **Remember Me** | 方便、提高用户体验 | 安全风险、Token 泄露 | "记住我"功能 |
| **匿名认证** | 允许未登录访问 | 功能有限 | 公开内容 |

#### 8.7 常见问题（FAQ）

**Q1: 如何实现同时支持多种认证方式？**

A: Spring Security 默认支持多种认证方式：

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/api/**").hasRole("API_USER")
            .anyRequest().authenticated()
        )
        // 表单登录（用于 Web 应用）
        .formLogin(withDefaults())
        // HTTP Basic（用于 API）
        .httpBasic(withDefaults())
        // Remember Me
        .rememberMe(withDefaults());

    return http.build();
}
```

**工作原理**:
- 对于浏览器请求，使用表单登录
- 对于 API 请求，使用 HTTP Basic
- 如果用户勾选"记住我"，使用 Remember Me

**Q2: 如何实现 AJAX 登录？**

A: 可以通过自定义 `AuthenticationSuccessHandler` 和 `AuthenticationFailureHandler` 实现：

```java
@Component
public class AjaxAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
    
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, 
            HttpServletResponse response, Authentication authentication)
            throws IOException, ServletException {
        
        // 检查是否是 AJAX 请求
        if ("XMLHttpRequest".equals(request.getHeader("X-Requested-With"))) {
            // 返回 JSON
            response.setContentType("application/json;charset=UTF-8");
            response.setStatus(HttpServletResponse.SC_OK);
            
            Map<String, Object> data = new HashMap<>();
            data.put("success", true);
            data.put("username", authentication.getName());
            data.put("roles", authentication.getAuthorities()
                .stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.toList()));
            
            new ObjectMapper().writeValue(response.getWriter(), data);
        } else {
            // 普通 AJAX 请求，重定向
            response.sendRedirect("/home");
        }
    }
}
```

```java
@Component
public class AjaxAuthenticationFailureHandler implements AuthenticationFailureHandler {
    
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, 
            HttpServletResponse response, AuthenticationException exception)
            throws IOException, ServletException {
        
        // 检查是否是 AJAX 请求
        if ("XMLHttpRequest".equals(request.getHeader("X-Requested-With"))) {
            // 返回 JSON
            response.setContentType("application/json;charset=UTF-8");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            
            Map<String, Object> data = new HashMap<>();
            data.put("success", false);
            data.put("message", exception.getMessage());
            
            new ObjectMapper().writeValue(response.getWriter(), data);
        } else {
            // 普通 AJAX 请求，重定向
            response.sendRedirect("/login?error=true");
        }
    }
}
```

**配置**:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .formLogin(form -> form
            .loginPage("/login")
            .successHandler(ajaxAuthenticationSuccessHandler)
            .failureHandler(ajaxAuthenticationFailureHandler)
        );

    return http.build();
}
```

**Q3: 如何实现登录验证码？**

A: 可以通过自定义过滤器实现：

```java
public class CaptchaAuthenticationFilter extends UsernamePasswordAuthenticationFilter {
    
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, 
            HttpServletResponse response) throws AuthenticationException {
        
        // 验证验证码
        String captchaCode = request.getParameter("captcha");
        String sessionCaptcha = (String) request.getSession().getAttribute("captcha");
        
        if (!StringUtils.equalsIgnoreCase(captchaCode, sessionCaptcha)) {
            throw new AuthenticationServiceException("验证码错误");
        }
        
        // 清除验证码
        request.getSession().removeAttribute("captcha");
        
        // 调用父类的认证方法
        return super.attemptAuthentication(request, response);
    }
}
```

**配置**:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .addFilterBefore(new CaptchaAuthenticationFilter(), 
            UsernamePasswordAuthenticationFilter.class);

    return http.build();
}
```

**Q4: 如何实现手机验证码登录？**

A: 可以通过自定义 `AuthenticationProvider` 实现：

```java
@Component
public class SmsAuthenticationProvider implements AuthenticationProvider {
    
    @Autowired
    private SmsCodeService smsCodeService;
    
    @Autowired
    private UserService userService;

    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        SmsAuthenticationToken token = (SmsAuthenticationToken) authentication;
        
        String phoneNumber = token.getPrincipal().toString();
        String code = token.getCredentials().toString();
        
        // 验证验证码
        if (!smsCodeService.verifyCode(phoneNumber, code)) {
            throw new BadCredentialsException("验证码错误或已过期");
        }
        
        // 加载用户信息
        User user = userService.findByPhoneNumber(phoneNumber);
        if (user == null) {
            throw new UsernameNotFoundException("用户不存在");
        }
        
        // 创建已认证的 Token
        return new SmsAuthenticationToken(user, code, user.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(SmsAuthenticationToken.class);
    }
}
```

**Q5: 如何实现单点登录（SSO）？**

A: 可以使用以下方案：

**方案 1: 基于 Session 的 SSO（使用 Spring Session）**

```xml
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```

```java
@EnableRedisHttpSession
@Configuration
public class SessionConfig {
    // 配置 Redis Session
}
```

**方案 2: 基于 Token 的 SSO（使用 JWT）**

```java
@Component
public class JwtTokenProvider {
    
    /**
     * 生成 JWT Token
     * 
     * @param authentication 认证信息
     * @return JWT Token
     */
    public String generateToken(Authentication authentication) {
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        
        Date expiryDate = new Date(System.currentTimeMillis() + expirationTime);
        
        return Jwts.builder()
            .setSubject(Long.toString(userPrincipal.getId()))
            .setIssuedAt(new Date())
            .setExpiration(expiryDate)
            .signWith(SignatureAlgorithm.HS512, secretKey)
            .compact();
    }
    
    /**
     * 验证 JWT Token
     * 
     * @param token JWT Token
     * @return 如果有效返回 true
     */
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token);
            return true;
        } catch (SignatureException ex) {
            logger.error("Invalid JWT signature");
        } catch (MalformedJwtException ex) {
            logger.error("Invalid JWT token");
        } catch (ExpiredJwtException ex) {
            logger.error("Expired JWT token");
        } catch (UnsupportedJwtException ex) {
            logger.error("Unsupported JWT token");
        } catch (IllegalArgumentException ex) {
            logger.error("JWT claims string is empty");
        }
        return false;
    }
}
```

**方案 3: 使用 OAuth 2.0 / OpenID Connect**

参见知识点4.3.3的 OAuth 2.0 配置示例。

#### 8.8 思考题

1. **认证方式选择**:
   - 问题：在一个同时有 Web 应用和移动 API 的系统中，应该选择哪种认证方式？
   - 提示：考虑用户体验、安全性、无状态

2. **会话管理**:
   - 问题：如何防止会话固定攻击？
   - 提示：研究 `sessionFixation()` 配置和会话迁移策略

3. **Token 安全**:
   - 问题：如何存储 Remember Me Token 更安全？
   - 提示：考虑 HttpOnly、Secure、SameSite Cookie 属性

4. **多设备登录**:
   - 问题：如何实现单用户最大登录数限制？
   - 提示：考虑并发控制、会话管理

5. **API 认证**:
   - 问题：如何设计一个安全的 REST API 认证机制？
   - 提示：考虑 JWT、OAuth 2.0、API Key

---

## 结语

本课程讲义（第一部分）详细介绍了 Spring Security 5.3.9.RELEASE 的核心概念和实战应用，涵盖了快速入门和认证机制两大模块。通过本课程，你将掌握：

1. **Spring Security 的核心架构**: 过滤器链、认证流程、授权机制
2. **认证机制**: 用户详情服务、密码编码、多种认证方式
3. **实战经验**: 配置技巧、最佳实践、常见问题解决

在下一部分中，我们将深入探讨：
- 模块3: 授权机制深度剖析
- 模块4: 高级主题

继续学习，你将成为 Spring Security 专家！

---

## 附录

### A. 参考资料

1. **官方文档**
   - Spring Security Reference: https://docs.spring.io/spring-security/reference/
   - Spring Security API 文档: https://docs.spring.io/spring-security/site/docs/current/api/

2. **推荐书籍**
   - "Spring Security in Action"
   - "Spring Security 3.1"
   - "Pro Spring Security"

3. **在线资源**
   - Spring Security GitHub: https://github.com/spring-projects/spring-security
   - Spring Security Samples: https://github.com/spring-projects/spring-security-samples

### B. 代码示例

本课程讲义中的所有代码示例都可以在以下仓库找到：
- GitHub: https://github.com/your-repo/spring-security-course
- 分支: `part1`

### C. 练习题

为了巩固所学知识，建议完成以下练习：

1. **基础练习**
   - 创建一个简单的 Spring Security 应用
   - 实现自定义登录页面
   - 配置多个用户和角色

2. **进阶练习**
   - 实现 Remember Me 功能
   - 自定义 UserDetailsService
   - 实现登录验证码

3. **高级练习**
   - 实现多因素认证
   - 实现 API 认证（JWT）
   - 实现单点登录（SSO）

### D. 常见错误及解决方案

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `AccessDeniedException` | 权限不足 | 检查用户角色和权限配置 |
| `BadCredentialsException` | 用户名或密码错误 | 检查用户凭据和密码编码 |
| `DisabledException` | 账户已禁用 | 启用用户账户 |
| `LockedException` | 账户已锁定 | 解锁用户账户 |
| `AccountExpiredException` | 账户已过期 | 更新账户有效期 |
| `CredentialsExpiredException` | 密码已过期 | 修改密码 |

---

**文档信息**:
- **版本**: 1.0
- **最后更新**: 2024-03-14
- **作者**: Spring Security 课程团队
- **许可**: CC BY-NC-SA 4.0
