# Spring Security 课程讲义

**课程名称**: Spring Security
**教材版本**: 5.3.9.RELEASE
**课程层次**: 本科高级
**学分**: 3 学分
**总学时**: 48 学时（理论 36 + 实验 12）
**授课对象**: 计算机科学与技术专业 高年级本科生
**授课教师**: 汤
**课程学期**: 2026年春季学期

---

## 课程简介

本课程深入讲解 Spring Security 5.3.9.RELEASE 的核心原理和实战应用，通过理论学习和实验操作，使学生掌握 Spring Security 的认证、授权、安全防护等核心技术和最佳实践。课程重点结合理论与实践，帮助学生构建企业级安全应用。

---

## 课程大纲

### 模块1️⃣: Spring Security 快速入门
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点1: Spring Security 是什么？
  - 核心定义与历史
  - 为什么需要 Spring Security
  - 核心功能概览
  - 与其他安全框架对比
  
- 知识点2: 第一个 Spring Security 应用
  - 环境准备与依赖配置
  - 创建项目
  - 默认安全配置
  - 自定义用户配置
  - 实验1：搭建第一个安全应用
  
- 知识点3: 核心架构概念
  - 认证 vs 授权
  - 过滤器链架构
  - SecurityContext 机制
  - DelegatingFilterProxy 原理
  
- 知识点4: 配置方式详解
  - XML 配置方式
  - Java 配置方式
  - 配置继承体系
  - 常用配置方法

### 模块2️⃣: 认证机制深度剖析
**理论学时**: 8学时 | **实验学时**: 2学时

- 知识点5: 认证核心原理
  - Authentication 对象
  - AuthenticationManager 体系
  - ProviderManager 工作机制
  - AuthenticationProvider 接口
  - 认证流程源码分析
  
- 知识点6: 用户详情服务
  - UserDetailsService 接口
  - UserDetails 对象
  - UserDetailsService 实现策略
  - 内存用户配置
  - 数据库用户配置
  - LDAP 集成
  
- 知识点7: 密码编码机制
  - 密码安全原理
  - PasswordEncoder 接口
  - BCryptPasswordEncoder 详解
  - 其他编码器对比
  - 密码加盐与哈希
  - 实验2：实现自定义密码编码
  
- 知识点8: 常用认证方式
  - 表单登录认证
  - HTTP Basic 认证
  - Digest 认证
  - X.509 证书认证
  - Remember-Me 功能

### 模块3️⃣: 授权机制详解
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点9: 授权核心概念
  - 授权 vs 认证
  - AccessDecisionManager
  - AccessDecisionVoter
  - 授权决策流程
  
- 知识点10: 过滤器链详解
  - FilterChainProxy 原理
  - SecurityFilterChain 体系
  - 核心过滤器列表
  - 过滤器执行顺序
  - 自定义过滤器
  
- 知识点11: 表达式式访问控制
  - hasRole() 和 hasAuthority()
  - hasIpAddress() 等安全表达式
  - @PreAuthorize 注解
  - @PostAuthorize 注解
  - 自定义安全表达式
  - 实验3：实现细粒度权限控制
  
- 知识点12: 基于角色的访问控制
  - 角色层次结构
  - 角色继承
  - 动态权限配置
  - 自定义权限决策

### 模块4️⃣: 会话管理与 CSRF 防护
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点13: 会话管理
  - 会话创建策略
  - 会话固定攻击防护
  - 并发会话控制
  - 会话超时处理
  - SessionRegistry 原理
  
- 知识点14: CSRF 防护
  - CSRF 攻击原理
  - CSRF Token 机制
  - CsrfTokenRepository
  - CSRF 配置与禁用
  - 前后端分离场景处理
  - 实验4：配置 CSRF 防护
  
- 知识点15: 安全头配置
  - X-Frame-Options
  - X-XSS-Protection
  - Content-Security-Policy
  - Strict-Transport-Security
  - 自定义安全头

### 模块5️⃣: JWT 令牌认证
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点16: JWT 基础
  - JWT 是什么
  - JWT vs Session
  - JWT 结构解析
  - JWT 优缺点分析
  
- 知识点17: JWT 集成 Spring Security
  - JWT 认证流程
  - JWT 过滤器实现
  - JWT 工具类开发
  - Token 生成与验证
  - Token 刷新机制
  - 实验5：实现 JWT 认证系统
  
- 知识点18: JWT 安全最佳实践
  - Token 签名算法
  - Token 有效期设置
  - Token 刷新策略
  - Token 存储方案
  - 敏感信息处理

### 模块6️⃣: OAuth2 与 SSO
**理论学时**: 4学时 | **实验学时**: 1学时

- 知识点19: OAuth2 基础
  - OAuth2 核心概念
  - 授权码模式
  - 密码模式
  - 客户端模式
  - 简化模式
  
- 知识点20: Spring Security OAuth2
  - 授权服务器配置
  - 资源服务器配置
  - 客户端配置
  - Token 存储
  - JWT 集成

### 模块7️⃣: 安全测试与监控
**理论学时**: 4学时 | **实验学时**: 1学时

- 知识点21: 安全测试
  - @WithMockUser
  - @WithUserDetails
  - SecurityMockMvcConfigurers
  - 测试认证流程
  - 测试授权流程
  
- 知识点22: 安全审计与监控
  - 事件发布机制
  - AuditListener 配置
  - 访问日志记录
  - 异常检测
  - 实验6：实现安全审计系统

### 模块8️⃣: 综合实战项目
**理论学时**: 6学时 | **实验学时**: 0学时

- 知识点23: 企业级安全系统实战
  - 用户管理模块
  - 权限管理模块
  - JWT 认证模块
  - OAuth2 第三方登录
  - 安全审计模块
  - 前后端分离集成
  - 完整项目实战

---

## 教学方法

- **理论授课**（60%）：深入讲解核心原理、源码分析、架构设计
- **实验操作**（25%）：动手实践、代码实现、案例分析
- **项目实战**（15%）：企业级项目开发、最佳实践

---

## 考核方式

- **平时成绩**（30%）：实验作业、课堂参与
- **期中考试**（30%）：理论知识测试
- **期末项目**（40%）：综合项目实战

---

## 参考资料

- 官方文档：https://docs.spring.io/spring-security/site/docs/5.3.9.RELEASE/reference/html5/
- 源码仓库：https://github.com/spring-projects/spring-security
- 推荐书籍：《Spring Security 实战》

---

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

---

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

---

# Spring Security 5.3.9.RELEASE 课程讲义（第三部分）

## 模块5 - JWT 令牌认证

---

## 知识点16: JWT 基础

### 16.1 JWT 概述

#### 16.1.1 什么是 JWT

JSON Web Token (JWT) 是一种开放标准 (RFC 7519)，用于在各方之间以 JSON 对象安全地传输信息。JWT 可以使用 HMAC 算法或使用 RSA 或 ECDSA 的公钥/私钥对进行签名。

JWT 的设计目标是在紧凑和自包含的格式下安全地传输信息。由于 JWT 是自包含的，它减少了对数据库的查询，特别适合分布式系统和微服务架构。

#### 16.1.2 JWT 的历史和发展

JWT 的发展历程反映了Web应用架构的演变：

1. **早期阶段**: 基于Session的认证
   - 服务器端存储会话状态
   - 需要Session复制或粘性会话
   - 横向扩展困难

2. **Token化时代**: JWT的诞生
   - 2010年: JWT规范初稿
   - 2015年: RFC 7519正式发布
   - 特点: 无状态、可扩展、跨域

3. **现代发展**: JWT生态
   - 与OAuth2.0深度集成
   - OpenID Connect的核心组件
   - 微服务架构的标准认证方案

### 16.2 JWT 的结构

JWT 由三部分组成，用点 (.) 分隔：

```
Header.Payload.Signature
```

#### 16.2.1 Header（头部）

Header 通常由两部分组成：令牌的类型（即 JWT）和所使用的签名算法。

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**常用算法**:
- HS256: HMAC SHA256 (对称加密)
- HS384: HMAC SHA384
- HS512: HMAC SHA512
- RS256: RSASSA-PKCS1-v1_5 SHA256 (非对称加密)
- RS384: RSASSA-PKCS1-v1_5 SHA384
- RS512: RSASSA-PKCS1-v1_5 SHA512
- ES256: ECDSA using P-256 and SHA256
- ES384: ECDSA using P-384 and SHA384
- ES512: ECDSA using P-521 and SHA512
- PS256: RSASSA-PSS SHA256
- PS384: RSASSA-PSS SHA384
- PS512: RSASSA-PSS SHA512

**Header Base64 编码示例**:
```java
String header = "{\"alg\":\"HS256\",\"typ\":\"JWT\"}";
String encodedHeader = Base64.getUrlEncoder()
    .withoutPadding()
    .encodeToString(header.getBytes(StandardCharsets.UTF_8));
// 结果: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### 16.2.2 Payload（载荷）

Payload 包含声明（Claims）。声明是关于实体（通常是用户）和其他数据的陈述。

**声明类型**:

1. **注册声明 (Registered Claims)**: 预定义的声明
   - iss (issuer): 签发者
   - sub (subject): 主题
   - aud (audience): 接收方
   - exp (expiration time): 过期时间
   - nbf (not before): 生效时间
   - iat (issued at): 签发时间
   - jti (JWT ID): 唯一标识

2. **公共声明 (Public Claims)**: 自定义声明，可自由定义
   - 建议使用 URI 或 IANA JSON Web Token Registry 避免冲突

3. **私有声明 (Private Claims)**: 双方共享的声明
   - 仅在使用方之间有意义

**Payload 示例**:
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "exp": 1516242622,
  "roles": ["USER", "ADMIN"],
  "customData": {
    "department": "IT",
    "level": 5
  }
}
```

**Payload Base64 编码示例**:
```java
String payload = "{\"sub\":\"1234567890\",\"name\":\"John Doe\",\"iat\":1516239022}";
String encodedPayload = Base64.getUrlEncoder()
    .withoutPadding()
    .encodeToString(payload.getBytes(StandardCharsets.UTF_8));
```

#### 16.2.3 Signature（签名）

Signature 用于验证消息在传输过程中未被篡改，并且对于使用私钥签名的令牌，还可以验证发送者的身份。

**签名生成过程**:

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

**签名示例代码**:
```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class JWTSignatureGenerator {
    
    /**
     * 生成 HMAC SHA256 签名
     * 
     * @param data 待签名的数据 (header.payload)
     * @param secret 密钥
     * @return Base64URL 编码的签名
     * @throws Exception 签名过程中的异常
     */
    public static String generateSignature(String data, String secret) throws Exception {
        // 1. 初始化 HMAC SHA256 算法
        Mac mac = Mac.getInstance("HmacSHA256");
        SecretKeySpec secretKeySpec = new SecretKeySpec(
            secret.getBytes(StandardCharsets.UTF_8), 
            "HmacSHA256"
        );
        mac.init(secretKeySpec);
        
        // 2. 计算签名
        byte[] signature = mac.doFinal(data.getBytes(StandardCharsets.UTF_8));
        
        // 3. Base64URL 编码（不带填充）
        return Base64.getUrlEncoder()
            .withoutPadding()
            .encodeToString(signature);
    }
    
    public static void main(String[] args) throws Exception {
        String header = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9";
        String payload = "eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ";
        String secret = "your-256-bit-secret";
        
        String data = header + "." + payload;
        String signature = generateSignature(data, secret);
        
        System.out.println("完整的 JWT:");
        System.out.println(data + "." + signature);
        // 输出: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    }
}
```

**完整的 JWT 结构示例**:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### 16.3 JWT 的工作原理

#### 16.3.1 认证流程

```
用户登录
    ↓
服务器验证凭据
    ↓
服务器生成 JWT
    ↓
返回 JWT 给客户端
    ↓
客户端存储 JWT (localStorage/cookie)
    ↓
后续请求携带 JWT
    ↓
服务器验证 JWT
    ↓
验证通过，返回资源
```

#### 16.3.2 JWT 生成过程（服务器端）

```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import java.security.Key;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

public class JWTGenerator {
    
    // 密钥应该从配置中读取，不应硬编码
    private static final Key SECRET_KEY = Keys.secretKeyFor(SignatureAlgorithm.HS256);
    private static final long EXPIRATION_TIME = 86400000; // 24小时

    /**
     * 生成 JWT Token
     * 
     * @param subject 主题（通常是用户ID或用户名）
     * @param claims 自定义声明
     * @return JWT 字符串
     */
    public static String generateToken(String subject, Map<String, Object> claims) {
        // 获取当前时间
        Date now = new Date();
        // 计算过期时间
        Date expiryDate = new Date(now.getTime() + EXPIRATION_TIME);

        // 构建JWT
        JwtBuilder builder = Jwts.builder()
            .setSubject(subject)                    // 设置主题
            .setIssuedAt(now)                       // 设置签发时间
            .setExpiration(expiryDate)              // 设置过期时间
            .signWith(SECRET_KEY, SignatureAlgorithm.HS256); // 设置签名算法和密钥

        // 添加自定义声明
        if (claims != null && !claims.isEmpty()) {
            builder.addClaims(claims);
        }

        return builder.compact();
    }

    /**
     * 生成包含用户信息的 Token
     * 
     * @param username 用户名
     * @param userId 用户ID
     * @param roles 用户角色列表
     * @return JWT 字符串
     */
    public static String generateUserToken(String username, String userId, java.util.List<String> roles) {
        Map<String, Object> claims = new HashMap<>();
        claims.put("userId", userId);
        claims.put("roles", roles);
        claims.put("username", username);

        return generateToken(userId, claims);
    }

    public static void main(String[] args) {
        // 生成示例 Token
        String token = generateUserToken(
            "john.doe",
            "12345",
            java.util.Arrays.asList("USER", "ADMIN")
        );

        System.out.println("生成的 JWT:");
        System.out.println(token);
        System.out.println("\nToken 长度: " + token.length());
    }
}
```

#### 16.3.3 JWT 验证过程（服务器端）

```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import io.jsonwebtoken.security.SignatureException;
import java.security.Key;
import java.util.Date;

public class JWTValidator {
    
    private static final Key SECRET_KEY = Keys.secretKeyFor(SignatureAlgorithm.HS256);

    /**
     * 验证并解析 JWT Token
     * 
     * @param token JWT 字符串
     * @return 解析后的 Claims 对象
     * @throws ExpiredJwtException Token 过期
     * @throws UnsupportedJwtException 不支持的 Token
     * @throws MalformedJwtException 格式错误的 Token
     * @throws SignatureException 签名验证失败
     * @throws IllegalArgumentException 参数为空
     */
    public static Claims validateToken(String token) 
            throws ExpiredJwtException, 
                   UnsupportedJwtException, 
                   MalformedJwtException, 
                   SignatureException, 
                   IllegalArgumentException {
        
        // 解析并验证 Token
        Jws<Claims> jws = Jwts.parserBuilder()
            .setSigningKey(SECRET_KEY)
            .build()
            .parseClaimsJws(token);

        return jws.getBody();
    }

    /**
     * 检查 Token 是否过期（不抛出异常）
     * 
     * @param token JWT 字符串
     * @return true 如果 Token 有效，false 如果 Token 无效或过期
     */
    public static boolean isTokenValid(String token) {
        try {
            Claims claims = validateToken(token);
            
            // 检查过期时间
            Date expiration = claims.getExpiration();
            return expiration.after(new Date());
            
        } catch (ExpiredJwtException e) {
            System.out.println("Token 已过期: " + e.getMessage());
            return false;
        } catch (Exception e) {
            System.out.println("Token 无效: " + e.getMessage());
            return false;
        }
    }

    /**
     * 从 Token 中获取用户ID
     * 
     * @param token JWT 字符串
     * @return 用户ID
     */
    public static String getUserIdFromToken(String token) {
        Claims claims = validateToken(token);
        return claims.getSubject();
    }

    /**
     * 从 Token 中获取自定义声明
     * 
     * @param token JWT 字符串
     * @param key 声明的键
     * @return 声明的值
     */
    @SuppressWarnings("unchecked")
    public static <T> T getClaimFromToken(String token, String key, Class<T> type) {
        Claims claims = validateToken(token);
        Object value = claims.get(key);
        
        if (value != null && type.isInstance(value)) {
            return (T) value;
        }
        
        return null;
    }

    public static void main(String[] args) {
        String token = "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NSIsInJvbGVzIjpbIlVTRVIiLCJBRE1JTiJdLCJ1c2VybmFtZSI6ImpvaG4uZG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyNDI2MjJ9.xyz";
        
        // 验证 Token
        try {
            Claims claims = validateToken(token);
            System.out.println("Token 验证成功!");
            System.out.println("Subject: " + claims.getSubject());
            System.out.println("Username: " + claims.get("username"));
            System.out.println("Roles: " + claims.get("roles"));
            System.out.println("Issued At: " + claims.getIssuedAt());
            System.out.println("Expiration: " + claims.getExpiration());
            
        } catch (ExpiredJwtException e) {
            System.err.println("Token 已过期!");
        } catch (Exception e) {
            System.err.println("Token 验证失败: " + e.getMessage());
        }
    }
}
```

### 16.4 JWT 的优势和劣势

#### 16.4.1 优势

**1. 无状态性**
- 服务器不需要存储会话信息
- 易于水平扩展
- 降低服务器内存压力

**2. 跨域支持**
- 天然支持 CORS
- 适合分布式系统
- 移动端友好

**3. 自包含**
- 包含所有必要的用户信息
- 减少数据库查询
- 提高响应速度

**4. 可扩展性**
- 可以在 Token 中存储自定义信息
- 支持多种声明类型
- 灵活的数据结构

**5. 标准化**
- 基于 RFC 7519 标准
- 多语言支持
- 生态系统成熟

#### 16.4.2 劣势

**1. Token 大小**
- 比 Session ID 大
- 每次请求都要携带
- 增加带宽消耗

**2. 无法撤销**
- 一旦签发，在过期前始终有效
- 需要额外的黑名单机制
- 安全事件响应困难

**3. 信息泄露风险**
- Payload 仅编码，未加密
- 敏感信息可能泄露
- 需要谨慎处理数据

**4. Token 刷新复杂**
- 需要实现刷新机制
- 处理过期和刷新逻辑
- 用户体验挑战

**5. 密钥管理**
- 密钥泄露后果严重
- 需要定期轮换
- 密钥分发复杂

### 16.5 JWT 安全最佳实践

#### 16.5.1 密钥管理

```java
import javax.crypto.KeyGenerator;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.util.Base64;

/**
 * JWT 密钥管理工具类
 */
public class JWTKeyManager {
    
    /**
     * 生成安全的随机密钥
     * 
     * @param keySize 密钥大小（位）
     * @return Base64 编码的密钥
     */
    public static String generateSecureKey(int keySize) {
        try {
            // 使用 KeyGenerator 生成密钥
            KeyGenerator keyGenerator = KeyGenerator.getInstance("HmacSHA256");
            keyGenerator.init(keySize, new SecureRandom());
            
            byte[] key = keyGenerator.generateKey().getEncoded();
            return Base64.getEncoder().encodeToString(key);
            
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("生成密钥失败", e);
        }
    }
    
    /**
     * 验证密钥强度
     * 
     * @param key 密钥字符串
     * @return true 如果密钥足够强
     */
    public static boolean isKeyStrongEnough(String key) {
        // 密钥至少 256 位（32字节）
        return key != null && key.length() >= 32;
    }
    
    public static void main(String[] args) {
        // 生成 256 位密钥
        String key = generateSecureKey(256);
        System.out.println("生成的密钥: " + key);
        System.out.println("密钥长度: " + key.length());
        System.out.println("密钥强度: " + (isKeyStrongEnough(key) ? "足够" : "不足"));
    }
}
```

#### 16.5.2 过期时间设置

```java
import java.util.Date;

/**
 * JWT 过期时间配置
 */
public class JWTExpirationConfig {
    
    // 不同场景的过期时间配置
    public static final long ACCESS_TOKEN_EXPIRATION = 15 * 60 * 1000;      // 15分钟
    public static final long REFRESH_TOKEN_EXPIRATION = 7 * 24 * 60 * 60 * 1000L; // 7天
    public static final long REMEMBER_ME_EXPIRATION = 30 * 24 * 60 * 60 * 1000L;  // 30天
    
    /**
     * 根据场景计算过期时间
     * 
     * @param currentTime 当前时间
     * @param rememberMe 是否记住我
     * @return 过期时间
     */
    public static Date calculateExpiration(Date currentTime, boolean rememberMe) {
        long expiration = rememberMe ? REMEMBER_ME_EXPIRATION : ACCESS_TOKEN_EXPIRATION;
        return new Date(currentTime.getTime() + expiration);
    }
    
    /**
     * 计算刷新 Token 的过期时间
     * 
     * @param currentTime 当前时间
     * @return 过期时间
     */
    public static Date calculateRefreshExpiration(Date currentTime) {
        return new Date(currentTime.getTime() + REFRESH_TOKEN_EXPIRATION);
    }
}
```

#### 16.5.3 Token 刷新机制

```java
import io.jsonwebtoken.*;
import java.security.Key;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

/**
 * JWT Token 刷新管理器
 */
public class JWTRefreshManager {
    
    private static final Key SECRET_KEY = io.jsonwebtoken.security.Keys.secretKeyFor(SignatureAlgorithm.HS256);
    
    /**
     * 刷新 Access Token
     * 
     * @param refreshToken 刷新 Token
     * @return 新的 Access Token
     * @throws Exception 刷新失败
     */
    public static String refreshAccessToken(String refreshToken) throws Exception {
        // 验证刷新 Token
        Claims refreshClaims = validateRefreshToken(refreshToken);
        
        // 检查是否在刷新窗口期内（例如：过期前1天内可以刷新）
        Date expiration = refreshClaims.getExpiration();
        Date now = new Date();
        long timeUntilExpiration = expiration.getTime() - now.getTime();
        long refreshWindow = 24 * 60 * 60 * 1000L; // 1天
        
        if (timeUntilExpiration > refreshWindow) {
            throw new IllegalStateException("刷新令牌仍在有效期内，无需刷新");
        }
        
        // 生成新的 Access Token
        String userId = refreshClaims.getSubject();
        @SuppressWarnings("unchecked")
        java.util.List<String> roles = (java.util.List<String>) refreshClaims.get("roles");
        
        return generateAccessToken(userId, roles);
    }
    
    /**
     * 生成 Access Token
     */
    private static String generateAccessToken(String userId, java.util.List<String> roles) {
        Date now = new Date();
        Date expiration = new Date(now.getTime() + JWTExpirationConfig.ACCESS_TOKEN_EXPIRATION);
        
        Map<String, Object> claims = new HashMap<>();
        claims.put("roles", roles);
        claims.put("type", "access");
        
        return Jwts.builder()
            .setSubject(userId)
            .setIssuedAt(now)
            .setExpiration(expiration)
            .addClaims(claims)
            .signWith(SECRET_KEY, SignatureAlgorithm.HS256)
            .compact();
    }
    
    /**
     * 验证刷新 Token
     */
    private static Claims validateRefreshToken(String refreshToken) {
        Claims claims = Jwts.parserBuilder()
            .setSigningKey(SECRET_KEY)
            .build()
            .parseClaimsJws(refreshToken)
            .getBody();
        
        // 检查是否为刷新 Token
        String type = claims.get("type", String.class);
        if (!"refresh".equals(type)) {
            throw new IllegalArgumentException("无效的刷新令牌类型");
        }
        
        return claims;
    }
    
    /**
     * 生成刷新 Token
     */
    public static String generateRefreshToken(String userId, java.util.List<String> roles) {
        Date now = new Date();
        Date expiration = new Date(now.getTime() + JWTExpirationConfig.REFRESH_TOKEN_EXPIRATION);
        
        Map<String, Object> claims = new HashMap<>();
        claims.put("roles", roles);
        claims.put("type", "refresh");
        
        return Jwts.builder()
            .setSubject(userId)
            .setIssuedAt(now)
            .setExpiration(expiration)
            .addClaims(claims)
            .signWith(SECRET_KEY, SignatureAlgorithm.HS256)
            .compact();
    }
}
```

### 16.6 常见问题解答 (FAQ)

**Q1: JWT 和 Session 有什么区别？什么时候该用哪个？**

A: **区别对比**:

| 特性 | JWT | Session |
|------|-----|---------|
| 存储位置 | 客户端 | 服务器 |
| 状态性 | 无状态 | 有状态 |
| 扩展性 | 易于扩展 | 需要Session复制 |
| 大小 | 较大（~1KB） | 较小（~32字节） |
| 撤销 | 困难 | 容易 |
| 跨域 | 容易 | 需要配置 |

**选择建议**:
- **使用 JWT 的场景**:
  - 微服务架构
  - 移动应用
  - 跨域单点登录
  - 无状态API设计
  
- **使用 Session 的场景**:
  - 传统单体应用
  - 需要即时撤销权限
  - 敏感操作（支付、转账）
  - Cookie 大小受限

**Q2: JWT 是否安全？会被攻击吗？**

A: JWT 本身是安全的，但实现不当会导致安全漏洞：

**常见攻击方式**:
1. **签名伪造攻击**: 使用弱密钥或算法混淆
2. **重放攻击**: Token被截获后重复使用
3. **时间攻击**: 通过响应时间推断签名
4. **密钥泄露攻击**: 密钥管理不当

**防护措施**:
- 使用强密钥（至少256位）
- 启用HTTPS传输
- 实现Token黑名单
- 添加JTI（JWT ID）用于追踪
- 设置合理的过期时间
- 使用防重放机制

**Q3: JWT 的大小会影响性能吗？**

A: 是的，JWT 的大小确实会影响性能：

**影响分析**:
- **网络传输**: 每个请求携带 Token（约1-2KB）
- **带宽消耗**: 比 Session ID 大30-50倍
- **解析开销**: 需要Base64解码和签名验证

**优化建议**:
```java
// 精简 Claims，只存储必要信息
Map<String, Object> claims = new HashMap<>();
claims.put("sub", userId);          // 必需
claims.put("roles", roles);         // 必需
claims.put("iat", now);             // 必需
claims.put("exp", expiration);      // 必需
// 避免存储：用户详细信息、大量数据
```

- 压缩 Claims
- 使用Short-lived Token + Refresh Token
- 考虑使用 Session + JWT 混合模式

**Q4: 如何处理 JWT 的过期和刷新？**

A: 推荐使用 **双Token机制**:

```
登录成功
    ↓
生成 Access Token (15分钟) + Refresh Token (7天)
    ↓
客户端存储两个 Token
    ↓
Access Token 过期
    ↓
使用 Refresh Token 换取新的 Access Token
    ↓
Refresh Token 也过期
    ↓
重新登录
```

**实现代码**:
```java
// 登录时
String accessToken = jwtProvider.generateAccessToken(user);
String refreshToken = jwtProvider.generateRefreshToken(user);
return new LoginResponse(accessToken, refreshToken);

// Access Token 过期时
try {
    // 尝试使用 Access Token
    authenticate(accessToken);
} catch (ExpiredJwtException e) {
    // 使用 Refresh Token 获取新的 Access Token
    String newAccessToken = jwtProvider.refreshAccessToken(refreshToken);
    return newAccessToken;
}
```

**Q5: JWT 可以存储敏感信息吗？**

A: **不建议！** JWT 的 Payload 只是 Base64 编码，不是加密：

```java
// ❌ 错误：不要在 JWT 中存储敏感信息
Map<String, Object> claims = new HashMap<>();
claims.put("password", "secret123");      // 危险！
claims.put("creditCard", "4532...");      // 危险！
claims.put("ssn", "123-45-6789");         // 危险！

// ✅ 正确：只存储非敏感的标识和权限
Map<String, Object> claims = new HashMap<>();
claims.put("sub", userId);                // 用户ID
claims.put("roles", roles);               // 角色
claims.put("emailVerified", true);        // 布尔值
```

**原因**:
- Base64 可被任何人解码
- Token 可能被记录在日志中
- URL 中可能泄露

**建议**:
- 只存储用户ID和权限
- 敏感信息从数据库查询
- 使用 JWE (JSON Web Encryption) 如果需要加密

### 16.7 思考题

1. **设计题**: 设计一个基于JWT的单点登录系统，要求支持多个子系统的统一认证，并考虑Token刷新机制。

2. **安全题**: 分析以下场景的安全风险：用户登录后获得JWT，然后不小心将Token发送到了错误的地方（如错误的服务器URL），攻击者可能如何利用这个Token？如何防御？

3. **性能题**: 一个高流量API（QPS 10000+）使用JWT认证，每次请求都需要验证JWT签名，这成为性能瓶颈。如何优化？

4. **架构题**: 在微服务架构中，多个服务都需要验证JWT。每个服务都独立验证还是通过专门的认证服务验证？各有何优劣？

5. **实现题**: 实现一个JWT黑名单机制，用于强制撤销特定的Token（如用户修改密码后，之前的Token应该失效）。

---

## 知识点17: JWT 集成 Spring Security

### 17.1 Spring Security JWT 架构

#### 17.1.1 认证流程图

```
┌─────────┐                  ┌──────────────┐                  ┌──────────┐
│ 客户端   │                  │  Spring      │                  │  数据库   │
│ (前端)   │                  │  Security    │                  │          │
└────┬────┘                  └──────┬───────┘                  └────┬─────┘
     │                              │                               │
     │ 1. POST /login (用户名/密码) │                               │
     ├─────────────────────────────>│                               │
     │                              │ 2. 查询用户信息                │
     │                              ├──────────────────────────────>│
     │                              │ 3. 返回用户详情                │
     │                              │<──────────────────────────────┤
     │                              │ 4. 验证凭据                    │
     │                              │ 5. 生成 JWT                    │
     │ 6. 返回 JWT                  │                               │
     │<─────────────────────────────┤                               │
     │                              │                               │
     │ 7. 存储到 localStorage       │                               │
     │                              │                               │
     │                              │                               │
     │ 8. GET /api/resource (Header: Authorization: Bearer <JWT>) │
     ├─────────────────────────────>│                               │
     │                              │ 9. JWT 验证过滤器              │
     │                              │ 10. 解析 JWT                  │
     │                              │ 11. 验证签名                  │
     │                              │ 12. 提取权限                  │
     │                              │ 13. 设置 SecurityContext      │
     │                              │ 14. 执行业务逻辑              │
     │ 15. 返回资源数据              │                               │
     │<─────────────────────────────┤                               │
```

#### 17.1.2 核心组件

```java
/**
 * Spring Security JWT 核心组件架构
 * 
 * 1. JWT 工具类 (JwtUtil)
 *    - 生成 Token
 *    - 验证 Token
 *    - 解析 Token
 * 
 * 2. JWT 认证过滤器 (JwtAuthenticationFilter)
 *    - 从请求中提取 Token
 *    - 验证 Token 有效性
 *    - 设置认证信息到 SecurityContext
 * 
 * 3. JWT 认证入口点 (JwtAuthenticationEntryPoint)
 *    - 处理认证失败的情况
 *    - 返回 401 未授权
 * 
 * 4. JWT 访问拒绝处理器 (JwtAccessDeniedHandler)
 *    - 处理权限不足的情况
 *    - 返回 403 禁止访问
 * 
 * 5. UserDetailsService 实现
 *    - 从数据库加载用户信息
 *    - 实现 Spring Security 的 UserDetailsService
 * 
 * 6. SecurityConfig 配置类
 *    - 配置安全规则
 *    - 注册过滤器
 *    - 配置 CORS
 *    - 配置异常处理
 */
```

### 17.2 完整实现代码

#### 17.2.1 项目依赖 (pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>spring-security-jwt</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <name>Spring Security JWT Integration</name>
    <description>Spring Security 5.3.9.RELEASE with JWT</description>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
        <relativePath/>
    </parent>
    
    <properties>
        <java.version>11</java.version>
        <spring-security.version>5.3.9.RELEASE</spring-security.version>
        <jjwt.version>0.9.1</jjwt.version>
    </properties>
    
    <dependencies>
        <!-- Spring Boot Web Starter -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- Spring Security -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        
        <!-- Spring Data JPA -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        
        <!-- JWT (JJWT) -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>${jjwt.version}</version>
        </dependency>
        
        <!-- Validation -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        
        <!-- Lombok (可选) -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        
        <!-- H2 Database (开发环境) -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        
        <!-- MySQL Driver (生产环境) -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        
        <!-- Test -->
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
            </plugin>
        </plugins>
    </build>
</project>
```

#### 17.2.2 JWT 工具类

```java
package com.example.security.jwt;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import io.jsonwebtoken.security.SignatureException;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.stereotype.Component;

import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.util.Base64;
import java.util.Date;
import java.util.stream.Collectors;

/**
 * JWT Token 工具类
 * 
 * 职责：
 * 1. 生成 JWT Token
 * 2. 验证 JWT Token
 * 3. 从 Token 中提取信息
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@Component
public class JwtTokenProvider {
    
    /**
     * JWT 密钥
     * 从配置文件中读取，至少 256 位（32字节）
     */
    @Value("${app.jwt.secret}")
    private String jwtSecret;
    
    /**
     * Access Token 过期时间（毫秒）
     * 默认：15分钟
     */
    @Value("${app.jwt.expiration-in-ms:900000}")
    private long jwtExpirationInMs;
    
    /**
     * Refresh Token 过期时间（毫秒）
     * 默认：7天
     */
    @Value("${app.jwt.refresh-expiration-in-ms:604800000}")
    private long refreshExpirationInMs;
    
    /**
     * 获取签名密钥
     * 
     * @return SecretKey 对象
     */
    private SecretKey getSigningKey() {
        // 将字符串密钥转换为 SecretKey 对象
        byte[] keyBytes = jwtSecret.getBytes(StandardCharsets.UTF_8);
        return Keys.hmacShaKeyFor(keyBytes);
    }
    
    /**
     * 生成 Access Token
     * 
     * @param authentication Spring Security 认证对象
     * @return JWT Token 字符串
     */
    public String generateToken(Authentication authentication) {
        // 获取用户主体
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        
        // 获取当前时间
        Date now = new Date();
        
        // 计算过期时间
        Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);
        
        // 获取用户权限
        String authorities = authentication.getAuthorities().stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.joining(","));
        
        // 构建 JWT
        return Jwts.builder()
                .setSubject(Long.toString(userPrincipal.getId()))           // 设置主题：用户ID
                .setIssuedAt(now)                                           // 设置签发时间
                .setExpiration(expiryDate)                                  // 设置过期时间
                .claim("username", userPrincipal.getUsername())             // 添加自定义声明：用户名
                .claim("email", userPrincipal.getEmail())                   // 添加自定义声明：邮箱
                .claim("authorities", authorities)                          // 添加自定义声明：权限
                .signWith(getSigningKey(), SignatureAlgorithm.HS512)       // 签名
                .compact();                                                 // 生成 Token
    }
    
    /**
     * 生成 Refresh Token
     * 
     * @param userId 用户ID
     * @return Refresh Token 字符串
     */
    public String generateRefreshToken(Long userId) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + refreshExpirationInMs);
        
        return Jwts.builder()
                .setSubject(Long.toString(userId))
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .claim("type", "refresh")  // 标记为刷新令牌
                .signWith(getSigningKey(), SignatureAlgorithm.HS512)
                .compact();
    }
    
    /**
     * 从 Token 中获取用户ID
     * 
     * @param token JWT Token
     * @return 用户ID
     */
    public Long getUserIdFromToken(String token) {
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        
        return Long.parseLong(claims.getSubject());
    }
    
    /**
     * 从 Token 中获取用户名
     * 
     * @param token JWT Token
     * @return 用户名
     */
    public String getUsernameFromToken(String token) {
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        
        return claims.get("username", String.class);
    }
    
    /**
     * 验证 Token 是否有效
     * 
     * @param token JWT Token
     * @return true 如果 Token 有效，false 否则
     */
    public boolean validateToken(String token) {
        try {
            // 解析并验证 Token
            Jwts.parserBuilder()
                    .setSigningKey(getSigningKey())
                    .build()
                    .parseClaimsJws(token);
            
            log.debug("JWT Token 验证成功");
            return true;
            
        } catch (SignatureException ex) {
            // 签名无效
            log.error("Invalid JWT signature");
        } catch (MalformedJwtException ex) {
            // Token 格式错误
            log.error("Invalid JWT token");
        } catch (ExpiredJwtException ex) {
            // Token 已过期
            log.error("Expired JWT token");
        } catch (UnsupportedJwtException ex) {
            // 不支持的 Token
            log.error("Unsupported JWT token");
        } catch (IllegalArgumentException ex) {
            // Token 参数为空
            log.error("JWT claims string is empty");
        }
        
        return false;
    }
    
    /**
     * 获取 Token 过期时间
     * 
     * @param token JWT Token
     * @return 过期时间
     */
    public Date getExpirationDateFromToken(String token) {
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        
        return claims.getExpiration();
    }
    
    /**
     * 检查 Token 是否即将过期（在指定时间内）
     * 
     * @param token JWT Token
     * @param thresholdInMinutes 阈值（分钟）
     * @return true 如果即将过期
     */
    public boolean isTokenExpiringSoon(String token, int thresholdInMinutes) {
        Date expiration = getExpirationDateFromToken(token);
        long timeUntilExpiration = expiration.getTime() - System.currentTimeMillis();
        long thresholdInMillis = thresholdInMinutes * 60 * 1000L;
        
        return timeUntilExpiration < thresholdInMillis;
    }
}
```

#### 17.2.3 JWT 认证过滤器

```java
package com.example.security.jwt;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * JWT 认证过滤器
 * 
 * 职责：
 * 1. 从请求中提取 JWT Token
 * 2. 验证 Token 有效性
 * 3. 加载用户详情
 * 4. 设置认证信息到 SecurityContext
 * 
 * 继承 OncePerRequestFilter 确保每个请求只执行一次
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    private final JwtTokenProvider tokenProvider;
    private final CustomUserDetailsService customUserDetailsService;
    
    /**
     * 从请求头中提取 JWT Token
     * 
     * Token 格式：Bearer <token>
     * 
     * @param request HTTP 请求
     * @return JWT Token 字符串，如果不存在返回 null
     */
    private String extractJwtFromRequest(HttpServletRequest request) {
        // 获取 Authorization 头
        String bearerToken = request.getHeader("Authorization");
        
        // 检查是否以 "Bearer " 开头
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            // 去掉 "Bearer " 前缀，返回纯 Token
            return bearerToken.substring(7);
        }
        
        return null;
    }
    
    /**
     * 过滤器的核心方法
     * 每个请求都会经过这个方法
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @param filterChain 过滤器链
     * @throws ServletException Servlet 异常
     * @throws IOException IO 异常
     */
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) 
            throws ServletException, IOException {
        
        try {
            // 1. 从请求中提取 JWT Token
            String jwt = extractJwtFromRequest(request);
            
            // 2. 如果 Token 存在且有效
            if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
                // 3. 从 Token 中获取用户ID
                Long userId = tokenProvider.getUserIdFromToken(jwt);
                
                // 4. 加载用户详情（从数据库或缓存）
                UserDetails userDetails = customUserDetailsService.loadUserById(userId);
                
                // 5. 创建认证对象
                UsernamePasswordAuthenticationToken authentication = 
                        new UsernamePasswordAuthenticationToken(
                                userDetails,              // 主体
                                null,                     // 凭据（Token已验证，不需要）
                                userDetails.getAuthorities() // 权限
                        );
                
                // 6. 设置认证详情（包含IP地址等信息）
                authentication.setDetails(
                        new WebAuthenticationDetailsSource().buildDetails(request)
                );
                
                // 7. 将认证信息设置到 SecurityContext
                // 这样后续的过滤器就能获取到认证信息了
                SecurityContextHolder.getContext().setAuthentication(authentication);
                
                log.debug("设置用户认证成功: {}", userDetails.getUsername());
            }
        } catch (Exception ex) {
            // 认证失败，记录错误日志
            log.error("无法设置用户认证", ex);
        }
        
        // 8. 继续过滤器链
        filterChain.doFilter(request, response);
    }
}
```

#### 17.2.4 自定义 UserDetailsService

```java
package com.example.security.jwt;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * 自定义 UserDetailsService 实现
 * 
 * 职责：
 * 1. 从数据库加载用户信息
 * 2. 构建 UserDetails 对象
 * 3. 处理用户不存在的异常
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {
    
    private final UserRepository userRepository;
    
    /**
     * 根据用户名加载用户详情
     * 
     * @param username 用户名或邮箱
     * @return UserDetails 对象
     * @throws UsernameNotFoundException 用户不存在
     */
    @Override
    @Transactional(readOnly = true)
    public UserDetails loadUserByUsername(String usernameOrEmail) 
            throws UsernameNotFoundException {
        
        // 从数据库查询用户
        User user = userRepository.findByUsernameOrEmail(usernameOrEmail, usernameOrEmail)
                .orElseThrow(() -> 
                        new UsernameNotFoundException(
                                "User not found with username or email: " + usernameOrEmail
                        )
                );
        
        // 转换为 UserPrincipal 对象
        return UserPrincipal.create(user);
    }
    
    /**
     * 根据 User ID 加载用户详情
     * 
     * @param userId 用户ID
     * @return UserDetails 对象
     * @throws UsernameNotFoundException 用户不存在
     */
    @Transactional(readOnly = true)
    public UserDetails loadUserById(Long userId) {
        User user = userRepository.findById(userId)
                .orElseThrow(() -> 
                        new ResourceNotFoundException("User", "id", userId)
                );
        
        return UserPrincipal.create(user);
    }
}

/**
 * 自定义异常：资源未找到
 */
class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String resourceName, String fieldName, Object fieldValue) {
        super(String.format("%s not found with %s: '%s'", 
                resourceName, fieldName, fieldValue));
    }
}
```

#### 17.2.5 UserPrincipal 类

```java
package com.example.security.jwt;

import com.example.model.User;
import com.fasterxml.jackson.annotation.JsonIgnore;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.List;
import java.util.stream.Collectors;

/**
 * 自定义 UserDetails 实现
 * 
 * 这个类代表 Spring Security 中的用户主体
 * 它包装了实际的 User 实体，并提供 UserDetails 接口所需的方法
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class UserPrincipal implements UserDetails {
    
    private Long id;
    private String username;
    
    @JsonIgnore  // 序列化时忽略密码
    private String email;
    
    @JsonIgnore  // 序列化时忽略密码
    private String password;
    
    private Collection<? extends GrantedAuthority> authorities;
    
    /**
     * 从 User 实体创建 UserPrincipal
     * 
     * @param user 用户实体
     * @return UserPrincipal 对象
     */
    public static UserPrincipal create(User user) {
        // 将用户的角色转换为 GrantedAuthority
        List<GrantedAuthority> authorities = user.getRoles().stream()
                .map(role -> new SimpleGrantedAuthority(role.getName().name()))
                .collect(Collectors.toList());
        
        return UserPrincipal.builder()
                .id(user.getId())
                .username(user.getUsername())
                .email(user.getEmail())
                .password(user.getPassword())
                .authorities(authorities)
                .build();
    }
    
    /**
     * 返回用户的权限
     * 用于方法级安全注解（@PreAuthorize, @Secured等）
     */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }
    
    /**
     * 返回用户的密码
     * 用于密码验证
     */
    @Override
    public String getPassword() {
        return password;
    }
    
    /**
     * 返回用户名
     */
    @Override
    public String getUsername() {
        return username;
    }
    
    /**
     * 账户是否未过期
     */
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
    
    /**
     * 账户是否未锁定
     */
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }
    
    /**
     * 凭据是否未过期
     */
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }
    
    /**
     * 账户是否启用
     */
    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

#### 17.2.6 JWT 认证入口点

```java
package com.example.security.jwt;

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
 * 
 * 职责：
 * 1. 处理认证失败的情况
 * 2. 返回统一的 401 响应
 * 3. 提供详细的错误信息
 * 
 * 当用户尝试访问受保护的资源而未提供有效的 Token 时，
 * 这个类的 commence 方法会被调用
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    
    /**
     * 处理认证异常
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @param authException 认证异常
     * @throws IOException IO 异常
     */
    @Override
    public void commence(HttpServletRequest request,
                        HttpServletResponse response,
                        AuthenticationException authException) 
            throws IOException {
        
        log.error("Responding with unauthorized error. Message - {}", authException.getMessage());
        
        // 设置响应状态码为 401 Unauthorized
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        // 设置响应内容类型为 JSON
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        // 设置字符编码
        response.setCharacterEncoding("UTF-8");
        
        // 构建错误响应体
        Map<String, Object> body = new HashMap<>();
        body.put("status", HttpServletResponse.SC_UNAUTHORIZED);
        body.put("error", "Unauthorized");
        body.put("message", "您需要登录才能访问此资源");
        body.put("path", request.getServletPath());
        
        // 将错误信息写入响应
        final ObjectMapper mapper = new ObjectMapper();
        mapper.writeValue(response.getOutputStream(), body);
    }
}
```

#### 17.2.7 JWT 访问拒绝处理器

```java
package com.example.security.jwt;

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
 * 
 * 职责：
 * 1. 处理权限不足的情况
 * 2. 返回统一的 403 响应
 * 3. 提供详细的错误信息
 * 
 * 当用户已认证但权限不足时，
 * 这个类的 handle 方法会被调用
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@Component
public class JwtAccessDeniedHandler implements AccessDeniedHandler {
    
    /**
     * 处理访问拒绝异常
     * 
     * @param request HTTP 请求
     * @param response HTTP 响应
     * @param accessDeniedException 访问拒绝异常
     * @throws IOException IO 异常
     */
    @Override
    public void handle(HttpServletRequest request,
                      HttpServletResponse response,
                      AccessDeniedException accessDeniedException) 
            throws IOException {
        
        log.error("Responding with forbidden error. Message - {}", accessDeniedException.getMessage());
        
        // 设置响应状态码为 403 Forbidden
        response.setStatus(HttpServletResponse.SC_FORBIDDEN);
        // 设置响应内容类型为 JSON
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        // 设置字符编码
        response.setCharacterEncoding("UTF-8");
        
        // 构建错误响应体
        Map<String, Object> body = new HashMap<>();
        body.put("status", HttpServletResponse.SC_FORBIDDEN);
        body.put("error", "Forbidden");
        body.put("message", "您没有权限访问此资源");
        body.put("path", request.getServletPath());
        
        // 将错误信息写入响应
        final ObjectMapper mapper = new ObjectMapper();
        mapper.writeValue(response.getOutputStream(), body);
    }
}
```

#### 17.2.8 Spring Security 配置类

```java
package com.example.security;

import com.example.security.jwt.JwtAuthenticationEntryPoint;
import com.example.security.jwt.JwtAuthenticationFilter;
import com.example.security.jwt.JwtAccessDeniedHandler;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.BeanIds;
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
 * Spring Security 主配置类
 * 
 * 职责：
 * 1. 配置安全规则（哪些URL需要认证）
 * 2. 配置 CORS
 * 3. 配置 Session 管理（无状态）
 * 4. 注册 JWT 过滤器
 * 5. 配置异常处理
 * 6. 启用方法级安全
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(
        securedEnabled = true,      // 启用 @Secured 注解
        jsr250Enabled = true,       // 启用 @RolesAllowed 注解
        prePostEnabled = true       // 启用 @PreAuthorize 和 @PostAuthorize 注解
)
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    private final CustomUserDetailsService customUserDetailsService;
    private final JwtAuthenticationEntryPoint unauthorizedHandler;
    private final JwtAccessDeniedHandler accessDeniedHandler;
    
    /**
     * JWT 认证过滤器 Bean
     */
    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter() {
        return new JwtAuthenticationFilter(tokenProvider, customUserDetailsService);
    }
    
    /**
     * 密码编码器 Bean
     * 使用 BCrypt 算法加密密码
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    /**
     * 认证管理器 Bean
     * 用于登录认证
     */
    @Bean(BeanIds.AUTHENTICATION_MANAGER)
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
    
    /**
     * 配置认证管理器
     * 设置 UserDetailsService 和 PasswordEncoder
     */
    @Override
    public void configure(AuthenticationManagerBuilder authenticationManagerBuilder) 
            throws Exception {
        authenticationManagerBuilder
                .userDetailsService(customUserDetailsService)
                .passwordEncoder(passwordEncoder());
    }
    
    /**
     * 配置 HTTP 安全规则
     * 
     * 这是 Spring Security 配置的核心方法
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                // 1. 配置 CORS
                .cors()
                    .and()
                
                // 2. 配置 CSRF（禁用，因为使用 JWT）
                .csrf()
                    .disable()
                
                // 3. 配置异常处理
                .exceptionHandling()
                    .authenticationEntryPoint(unauthorizedHandler)  // 401 处理
                    .accessDeniedHandler(accessDeniedHandler)       // 403 处理
                    .and()
                
                // 4. 配置 Session 管理（无状态）
                .sessionManagement()
                    .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                    .and()
                
                // 5. 配置授权规则
                .authorizeRequests()
                    // 公开访问的端点（不需要认证）
                    .antMatchers("/api/auth/**")
                        .permitAll()
                    .antMatchers("/api/public/**")
                        .permitAll()
                    // 管理员专用端点
                    .antMatchers("/api/admin/**")
                        .hasRole("ADMIN")
                    // 其他所有请求都需要认证
                    .anyRequest()
                        .authenticated();
        
        // 6. 添加 JWT 认证过滤器
        // 这个过滤器会在 UsernamePasswordAuthenticationFilter 之前执行
        http.addFilterBefore(jwtAuthenticationFilter(), 
                           UsernamePasswordAuthenticationFilter.class);
    }
}
```

### 17.3 源码深度分析

#### 17.3.1 OncePerRequestFilter 源码分析

```java
/**
 * OncePerRequestFilter 源码分析
 * 
 * 位置：org.springframework.web.filter.OncePerRequestFilter
 * 
 * 核心思想：确保过滤器在每个请求中只执行一次
 * 
 * 为什么需要这个类？
 * - 在某些情况下（如请求转发），同一个请求可能经过同一个过滤器多次
 * - JWT 验证可能涉及数据库查询，重复执行会导致性能问题
 * 
 * 实现原理：
 * - 使用 request attribute 标记是否已执行
 * - 每次执行前检查 attribute，如果存在则跳过
 */
package org.springframework.web.filter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public abstract class OncePerRequestFilter implements Filter {
    
    /**
     * Filter 已经执行的标记名称
     * 存储在 request attribute 中
     */
    public static final String ALREADY_FILTERED_SUFFIX = ".FILTERED";
    
    /**
     * 核心 doFilter 方法
     * 
     * @param request 请求
     * @param response 响应
     * @param filterChain 过滤器链
     */
    @Override
    public final void doFilter(ServletRequest request, ServletResponse response, 
                               FilterChain filterChain)
            throws ServletException, IOException {
        
        // 类型转换
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        
        // 获取已执行过滤器的标记名称
        String alreadyFilteredAttributeName = getAlreadyFilteredAttributeName();
        
        // 检查是否已经执行过
        if (request.getAttribute(alreadyFilteredAttributeName) != null) {
            // 已执行过，跳过
            if (request.getAttribute(alreadyFilteredAttributeName) != this) {
                // 不是同一个过滤器实例，继续执行
                doFilter(httpRequest, httpResponse, filterChain);
            } else {
                // 是同一个过滤器，跳过执行
                filterChain.doFilter(request, response);
            }
        } else {
            // 首次执行
            // 设置标记
            request.setAttribute(alreadyFilteredAttributeName, this);
            
            try {
                // 调用抽象方法，由子类实现
                doFilterInternal(httpRequest, httpResponse, filterChain);
            } finally {
                // 移除标记
                request.removeAttribute(alreadyFilteredAttributeName);
            }
        }
    }
    
    /**
     * 获取已执行过滤器的标记名称
     * 格式：类名 + .FILTERED
     */
    protected String getAlreadyFilteredAttributeName() {
        return getClass().getName() + ALREADY_FILTERED_SUFFIX;
    }
    
    /**
     * 子类需要实现的方法
     * 实际的过滤逻辑在这里
     */
    protected abstract void doFilterInternal(
            HttpServletRequest request, 
            HttpServletResponse response, 
            FilterChain filterChain) 
            throws ServletException, IOException;
}
```

#### 17.3.2 SecurityContext 源码分析

```java
/**
 * SecurityContext 源码分析
 * 
 * 位置：org.springframework.security.core.context.SecurityContext
 * 
 * 核心概念：SecurityContext 是 Spring Security 存储认证信息的中心
 * 
 * 主要组件：
 * 1. SecurityContext - 接口
 * 2. SecurityContextHolder - 工具类
 * 3. Authentication - 认证对象
 * 
 * 存储策略（MODE）：
 * - MODE_THREADLOCAL: 线程本地存储（默认）
 * - MODE_INHERITABLETHREADLOCAL: 可继承的线程本地
 * - MODE_GLOBAL: 全局存储
 */

// ============ SecurityContext 接口 ============
package org.springframework.security.core.context;

import org.springframework.security.core.Authentication;

/**
 * SecurityContext 接口
 * 
 * 职责：存储当前用户的认证信息
 */
public interface SecurityContext extends java.io.Serializable {
    
    /**
     * 获取认证对象
     * 
     * @return Authentication 对象，如果未认证返回 null
     */
    Authentication getAuthentication();
    
    /**
     * 设置认证对象
     * 
     * @param authentication 认证对象
     */
    void setAuthentication(Authentication authentication);
}

// ============ SecurityContextHolder 工具类 ============
package org.springframework.security.core.context;

/**
 * SecurityContextHolder 工具类
 * 
 * 职责：管理 SecurityContext 的存储和访问
 * 
 * 使用 ThreadLocal 存储，确保每个线程有独立的 SecurityContext
 */
public class SecurityContextHolder {
    
    /**
     * 系统属性名，用于指定存储策略
     */
    public static final String MODE_THREADLOCAL = "MODE_THREADLOCAL";
    public static final String MODE_INHERITABLETHREADLOCAL = "MODE_INHERITABLETHREADLOCAL";
    public static final String MODE_GLOBAL = "MODE_GLOBAL";
    
    /**
     * 存储策略名称
     */
    private static String strategyName = System.getProperty(
            "spring.security.strategy", 
            MODE_THREADLOCAL
    );
    
    /**
     * SecurityContext 存储策略
     */
    private static SecurityContextHolderStrategy strategy;
    
    static {
        initialize();
    }
    
    /**
     * 初始化存储策略
     */
    private static void initialize() {
        if (MODE_THREADLOCAL.equals(strategyName)) {
            strategy = new ThreadLocalSecurityContextHolderStrategy();
        } else if (MODE_INHERITABLETHREADLOCAL.equals(strategyName)) {
            strategy = new InheritableThreadLocalSecurityContextHolderStrategy();
        } else if (MODE_GLOBAL.equals(strategyName)) {
            strategy = new GlobalSecurityContextHolderStrategy();
        } else {
            try {
                // 自定义策略
                Class<?> clazz = Class.forName(strategyName);
                strategy = (SecurityContextHolderStrategy) clazz.newInstance();
            } catch (Exception e) {
                throw new IllegalStateException(e);
            }
        }
    }
    
    /**
     * 获取当前 SecurityContext
     * 
     * @return SecurityContext 对象
     */
    public static SecurityContext getContext() {
        return strategy.getContext();
    }
    
    /**
     * 清除当前 SecurityContext
     * 通常在请求结束时调用
     */
    public static void clearContext() {
        strategy.clearContext();
    }
    
    /**
     * 设置当前 SecurityContext
     * 
     * @param context SecurityContext 对象
     */
    public static void setContext(SecurityContext context) {
        strategy.setContext(context);
    }
    
    /**
     * 创建空的 SecurityContext
     * 
     * @return 空 SecurityContext
     */
    public static SecurityContext createEmptyContext() {
        return strategy.createEmptyContext();
    }
}

// ============ ThreadLocalSecurityContextHolderStrategy ============
package org.springframework.security.core.context;

/**
 * 基于 ThreadLocal 的存储策略实现
 * 
 * 这是默认的策略
 * 每个线程都有自己独立的 SecurityContext
 */
class ThreadLocalSecurityContextHolderStrategy 
        implements SecurityContextHolderStrategy {
    
    /**
     * 线程本地存储
     */
    private static final ThreadLocal<SecurityContext> contextHolder = 
            new ThreadLocal<>();
    
    /**
     * 清除当前线程的 SecurityContext
     */
    @Override
    public void clearContext() {
        contextHolder.remove();
    }
    
    /**
     * 获取当前线程的 SecurityContext
     */
    @Override
    public SecurityContext getContext() {
        SecurityContext ctx = contextHolder.get();
        
        // 如果不存在，创建新的
        if (ctx == null) {
            ctx = createEmptyContext();
            contextHolder.set(ctx);
        }
        
        return ctx;
    }
    
    /**
     * 设置当前线程的 SecurityContext
     */
    @Override
    public void setContext(SecurityContext context) {
        contextHolder.set(context);
    }
    
    /**
     * 创建空的 SecurityContext
     */
    @Override
    public SecurityContext createEmptyContext() {
        return new SecurityContextImpl();
    }
}
```

### 17.4 完整的登录控制器

```java
package com.example.controller;

import com.example.payload.ApiResponse;
import com.example.payload.JwtAuthenticationResponse;
import com.example.payload.LoginRequest;
import com.example.payload.SignUpRequest;
import com.example.security.jwt.JwtTokenProvider;
import com.example.service.AuthService;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

/**
 * 认证控制器
 * 
 * 职责：
 * 1. 处理用户登录
 * 2. 处理用户注册
 * 3. 刷新 Token
 * 
 * @author Spring Security Team
 * @version 1.0.0
 */
@Slf4j
@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {
    
    private final AuthenticationManager authenticationManager;
    private final JwtTokenProvider tokenProvider;
    private final AuthService authService;
    
    /**
     * 用户登录接口
     * 
     * POST /api/auth/login
     * 
     * @param loginRequest 登录请求（用户名、密码）
     * @return JWT Token
     */
    @PostMapping("/login")
    public ResponseEntity<?> authenticateUser(@Valid @RequestBody LoginRequest loginRequest) {
        
        log.info("用户登录尝试: {}", loginRequest.getUsernameOrEmail());
        
        // 1. 使用 AuthenticationManager 验证用户凭据
        // 这会调用 UserDetailsService 加载用户信息
        // 然后使用 PasswordEncoder 验证密码
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        loginRequest.getUsernameOrEmail(),
                        loginRequest.getPassword()
                )
        );
        
        // 2. 将认证信息设置到 SecurityContext
        SecurityContextHolder.getContext().setAuthentication(authentication);
        
        // 3. 生成 JWT Token
        String jwt = tokenProvider.generateToken(authentication);
        
        // 4. 生成 Refresh Token
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        String refreshToken = tokenProvider.generateRefreshToken(userPrincipal.getId());
        
        log.info("用户登录成功: {}", userPrincipal.getUsername());
        
        // 5. 返回 Token
        return ResponseEntity.ok(
                new JwtAuthenticationResponse(jwt, refreshToken)
        );
    }
    
    /**
     * 用户注册接口
     * 
     * POST /api/auth/register
     * 
     * @param signUpRequest 注册请求
     * @return API 响应
     */
    @PostMapping("/register")
    public ResponseEntity<?> registerUser(@Valid @RequestBody SignUpRequest signUpRequest) {
        
        log.info("用户注册尝试: {}", signUpRequest.getUsername());
        
        // 1. 检查用户名是否已存在
        if (authService.existsByUsername(signUpRequest.getUsername())) {
            return ResponseEntity.badRequest()
                    .body(new ApiResponse(false, "用户名已被使用"));
        }
        
        // 2. 检查邮箱是否已存在
        if (authService.existsByEmail(signUpRequest.getEmail())) {
            return ResponseEntity.badRequest()
                    .body(new ApiResponse(false, "邮箱已被注册"));
        }
        
        // 3. 创建用户账户
        User user = authService.registerUser(signUpRequest);
        
        log.info("用户注册成功: {}", user.getUsername());
        
        // 4. 返回成功响应
        return ResponseEntity.ok()
                .body(new ApiResponse(true, "用户注册成功"));
    }
    
    /**
     * 刷新 Access Token
     * 
     * POST /api/auth/refresh
     * 
     * @param refreshTokenRequest 刷新 Token 请求
     * @return 新的 Access Token
     */
    @PostMapping("/refresh")
    public ResponseEntity<?> refreshAccessToken(
            @RequestBody RefreshTokenRequest refreshTokenRequest) {
        
        String refreshToken = refreshTokenRequest.getRefreshToken();
        
        // 验证 Refresh Token
        if (!tokenProvider.validateToken(refreshToken)) {
            return ResponseEntity.badRequest()
                    .body(new ApiResponse(false, "无效的刷新令牌"));
        }
        
        // 获取用户ID
        Long userId = tokenProvider.getUserIdFromToken(refreshToken);
        
        // 生成新的 Access Token
        String newAccessToken = tokenProvider.generateAccessTokenFromUserId(userId);
        
        return ResponseEntity.ok(
                new JwtAuthenticationResponse(newAccessToken, refreshToken)
        );
    }
}

/**
 * 登录请求 DTO
 */
@Data
class LoginRequest {
    @NotBlank
    private String usernameOrEmail;
    
    @NotBlank
    @Size(min = 6, max = 40)
    private String password;
}

/**
 * 注册请求 DTO
 */
@Data
class SignUpRequest {
    @NotBlank
    @Size(min = 3, max = 15)
    private String username;
    
    @NotBlank
    @Size(max = 40)
    @Email
    private String email;
    
    @NotBlank
    @Size(min = 6, max = 20)
    private String password;
}

/**
 * JWT 认证响应 DTO
 */
@Data
@AllArgsConstructor
class JwtAuthenticationResponse {
    private String accessToken;
    private String refreshToken;
    private String tokenType = "Bearer";
    private Long expiresIn;  // 过期时间（秒）
}

/**
 * 刷新 Token 请求 DTO
 */
@Data
class RefreshTokenRequest {
    @NotBlank
    private String refreshToken;
}
```

### 17.5 常见问题解答 (FAQ)

**Q1: JWT 过滤器的执行顺序是什么？为什么在 UsernamePasswordAuthenticationFilter 之前？**

A: **过滤器链执行顺序**：

```
请求 → 
  ChannelProcessingFilter →
  SecurityContextPersistenceFilter →
  ConcurrentSessionFilter →
  LogoutFilter →
  UsernamePasswordAuthenticationFilter ← JWT 过滤器在这里
  BasicAuthenticationFilter →
  RememberMeAuthenticationFilter →
  AnonymousAuthenticationFilter →
  ExceptionTranslationFilter →
  FilterSecurityInterceptor →
  Controller
```

**为什么 JWT 过滤器要在 UsernamePasswordAuthenticationFilter 之前**：

```java
http.addFilterBefore(
    jwtAuthenticationFilter(),  // 我们的 JWT 过滤器
    UsernamePasswordAuthenticationFilter.class  // 在这个过滤器之前
);
```

**原因**：
1. **登录请求**：`POST /api/login` 不带 Token，应该被 UsernamePasswordAuthenticationFilter 处理
2. **API 请求**：`GET /api/users` 带 Token，应该被 JWT 过滤器处理
3. **在之前执行**：JWT 过滤器能先检查请求，如果有有效 Token 就设置认证，后续过滤器就无需处理

**工作流程**：
```java
// JWT 过滤器执行
if (有有效Token) {
    设置 SecurityContext 认证
    // 后续 UsernamePasswordAuthenticationFilter 会发现已有认证，跳过
}
// 没有 Token，继续执行过滤器链
// UsernamePasswordAuthenticationFilter 检查是否为登录请求
```

**Q2: 如何实现 JWT Token 的动态刷新？**

A: **动态刷新机制设计**：

```java
/**
 * 动态 Token 刷新方案
 */
public class DynamicTokenRefreshService {
    
    @Autowired
    private JwtTokenProvider tokenProvider;
    
    /**
     * 方案1: 客户端主动刷新
     * 
     * 流程：
     * 1. 客户端检测 Access Token 即将过期（如还有5分钟）
     * 2. 调用 /api/auth/refresh 接口
     * 3. 使用 Refresh Token 换取新的 Access Token
     * 4. 更新本地存储
     */
    public String refreshToken(String refreshToken) {
        // 验证 Refresh Token
        if (!tokenProvider.validateToken(refreshToken)) {
            throw new InvalidTokenException("刷新令牌无效");
        }
        
        // 获取用户ID
        Long userId = tokenProvider.getUserIdFromToken(refreshToken);
        
        // 生成新的 Access Token
        return tokenProvider.generateAccessTokenFromUserId(userId);
    }
    
    /**
     * 方案2: 服务端自动刷新（滑动窗口）
     * 
     * 流程：
     * 1. 每次请求验证 Token 时
     * 2. 如果 Token 即将过期（如还有10%时间）
     * 3. 自动生成新 Token 并返回
     * 4. 客户端从响应头获取新 Token
     */
    public String checkAndRefreshToken(String oldToken) {
        // 检查是否即将过期
        if (tokenProvider.isTokenExpiringSoon(oldToken, 5)) {
            // 获取用户ID
            Long userId = tokenProvider.getUserIdFromToken(oldToken);
            
            // 生成新 Token
            String newToken = tokenProvider.generateAccessTokenFromUserId(userId);
            
            // 返回新 Token
            return newToken;
        }
        
        return null;
    }
}

/**
 * 在 JWT 过滤器中实现自动刷新
 */
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) 
            throws ServletException, IOException {
        
        try {
            String jwt = extractJwtFromRequest(request);
            
            if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
                // ... 正常认证逻辑 ...
                
                // 检查是否需要刷新
                String newToken = dynamicTokenRefreshService.checkAndRefreshToken(jwt);
                
                if (newToken != null) {
                    // 将新 Token 添加到响应头
                    response.setHeader("X-New-Token", newToken);
                }
            }
        } catch (Exception ex) {
            logger.error("Could not set user authentication", ex);
        }
        
        filterChain.doFilter(request, response);
    }
}
```

**Q3: 如何处理 JWT 的注销（Logout）？**

A: **JWT 注销的挑战和方案**：

```java
/**
 * JWT 注销方案
 * 
 * 问题：JWT 是无状态的，一旦签发就无法撤销
 * 
 * 解决方案：
 */

// ============ 方案1: Token 黑名单（Redis） ============
@Service
public class TokenBlacklistService {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    private static final String BLACKLIST_KEY_PREFIX = "jwt:blacklist:";
    private static final long BLACKLIST_TTL = 7 * 24 * 60 * 60; // 7天
    
    /**
     * 将 Token 加入黑名单
     * 
     * @param token JWT Token
     * @param expiration Token 原始过期时间
     */
    public void addToBlacklist(String token, Date expiration) {
        long ttl = expiration.getTime() - System.currentTimeMillis();
        
        if (ttl > 0) {
            String key = BLACKLIST_KEY_PREFIX + token;
            redisTemplate.opsForValue().set(key, "blacklisted", ttl, TimeUnit.MILLISECONDS);
        }
    }
    
    /**
     * 检查 Token 是否在黑名单中
     * 
     * @param token JWT Token
     * @return true 如果已黑名单
     */
    public boolean isBlacklisted(String token) {
        String key = BLACKLIST_KEY_PREFIX + token;
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }
}

/**
 * 在 JWT 过滤器中检查黑名单
 */
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Autowired
    private TokenBlacklistService blacklistService;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) 
            throws ServletException, IOException {
        
        try {
            String jwt = extractJwtFromRequest(request);
            
            if (StringUtils.hasText(jwt)) {
                // 检查黑名单
                if (blacklistService.isBlacklisted(jwt)) {
                    throw new JwtException("Token 已被注销");
                }
                
                // 继续正常验证...
            }
        } catch (Exception ex) {
            // 处理异常
        }
        
        filterChain.doFilter(request, response);
    }
}

/**
 * 注销控制器
 */
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    
    @Autowired
    private TokenBlacklistService blacklistService;
    
    /**
     * 注销接口
     */
    @PostMapping("/logout")
    public ResponseEntity<?> logoutUser(HttpServletRequest request) {
        // 1. 从请求中获取 Token
        String token = extractJwtFromRequest(request);
        
        if (token != null) {
            // 2. 获取 Token 过期时间
            Date expiration = tokenProvider.getExpirationDateFromToken(token);
            
            // 3. 加入黑名单
            blacklistService.addToBlacklist(token, expiration);
        }
        
        // 4. 清除 SecurityContext
        SecurityContextHolder.clearContext();
        
        return ResponseEntity.ok(new ApiResponse(true, "注销成功"));
    }
}

// ============ 方案2: 短期 Token + 版本号 ============
/**
 * User 实体中增加 tokenVersion 字段
 * 每次注销时增加版本号
 * Token 中存储版本号，验证时比对
 */
@Entity
public class User {
    // ... 其他字段 ...
    
    @Column(name = "token_version")
    private Long tokenVersion = 0L;
    
    // 注销时
    public void incrementTokenVersion() {
        this.tokenVersion++;
    }
}

/**
 * JWT 生成时包含版本号
 */
public String generateToken(User user) {
    Map<String, Object> claims = new HashMap<>();
    claims.put("tokenVersion", user.getTokenVersion());
    
    return Jwts.builder()
        .setSubject(user.getId().toString())
        .addClaims(claims)
        .signWith(key)
        .compact();
}

/**
 * 验证时检查版本号
 */
public boolean validateToken(String token, User user) {
    Long tokenVersion = getTokenVersionFromToken(token);
    
    if (!tokenVersion.equals(user.getTokenVersion())) {
        return false; // 版本不匹配，Token 已失效
    }
    
    return true;
}
```

**Q4: Spring Security JWT 的性能如何优化？**

A: **性能优化策略**：

```java
/**
 * JWT 性能优化方案
 */

// ============ 优化1: 缓存用户详情 ============
@Service
@CacheConfig(cacheNames = "users")
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    /**
     * 缓存用户详情
     * 
     * 使用 Spring Cache + Redis
     */
    @Override
    @Cacheable(key = "#usernameOrEmail")
    @Transactional(readOnly = true)
    public UserDetails loadUserByUsername(String usernameOrEmail) {
        User user = userRepository.findByUsernameOrEmail(usernameOrEmail, usernameOrEmail)
                .orElseThrow(() -> new UsernameNotFoundException("User not found"));
        
        return UserPrincipal.create(user);
    }
    
    /**
     * 用户更新时清除缓存
     */
    @CacheEvict(key = "#user.username")
    public void updateUser(User user) {
        userRepository.save(user);
    }
}

// ============ 优化2: 使用 Caffeine 本地缓存 ============
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        CaffeineCacheManager cacheManager = new CaffeineCacheManager("users");
        cacheManager.setCaffeine(Caffeine.newBuilder()
                .maximumSize(1000)                    // 最大缓存数
                .expireAfterWrite(10, TimeUnit.MINUTES) // 写入后10分钟过期
                .recordStats());                       // 记录统计信息
        return cacheManager;
    }
}

// ============ 优化3: 精简 JWT Claims ============
public class JwtTokenProvider {
    
    /**
     * 精简版 Token：只包含必要信息
     * 
     * 减小 Token 大小：
     * - 不存储用户详细信息（从缓存加载）
     * - 不存储角色列表（使用角色ID）
     * - 使用短的 key 名称
     */
    public String generateToken(Authentication authentication) {
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);
        
        return Jwts.builder()
                .setSubject(userPrincipal.getId().toString())  // 必需
                .setIssuedAt(now)                              // 必需
                .setExpiration(expiryDate)                     // 必需
                .claim("v", userPrincipal.getTokenVersion())   // tokenVersion → v
                // 其他信息从缓存加载
                .signWith(getSigningKey())
                .compact();
    }
}

// ============ 优化4: 使用更快的安全算法 ============
/**
 * 算法性能对比（验证速度）：
 * - HS256 (HMAC-SHA256): 基准
 * - HS512 (HMAC-SHA512): 稍慢
 * - RS256 (RSA-SHA256): 慢 100-1000 倍
 * 
 * 推荐：使用 HS256 或 HS512
 */
@Configuration
public class JwtConfig {
    
    @Bean
    public JwtTokenProvider jwtTokenProvider() {
        // 使用 HS256 而不是 RS256
        // HMAC 验证速度比 RSA 快得多
        return new JwtTokenProvider(SignatureAlgorithm.HS256);
    }
}

// ============ 优化5: 并行验证 ============
/**
 * 对于高并发场景，可以使用并行验证
 */
@Component
public class ParallelJwtValidator {
    
    private final ExecutorService executor = Executors.newFixedThreadPool(10);
    
    /**
     * 并行验证多个 Token
     * 
     * @param tokens Token 列表
     * @return 验证结果
     */
    public Map<String, Boolean> validateTokensParallel(List<String> tokens) {
        return tokens.parallelStream()
                .collect(Collectors.toMap(
                        token -> token,
                        tokenProvider::validateToken
                ));
    }
}
```

**Q5: 如何实现多租户 JWT 认证？**

A: **多租户 JWT 方案**：

```java
/**
 * 多租户 JWT 认证架构
 * 
 * 租户识别方式：
 * 1. 子域名：tenant1.example.com
 * 2. 路径前缀：/api/tenant1/...
 * 3. Header：X-Tenant-ID
 */

// ============ 方案1: 租户 ID 放入 JWT ============
/**
 * JWT Payload 包含租户信息
 */
{
  "sub": "user123",
  "tenantId": "tenant1",  // 租户ID
  "roles": ["USER"],
  "iat": 1234567890,
  "exp": 1234567890
}

/**
 * JWT 生成时包含租户ID
 */
@Service
public class MultiTenantJwtProvider {
    
    public String generateToken(User user, String tenantId) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);
        
        return Jwts.builder()
                .setSubject(user.getId().toString())
                .claim("tenantId", tenantId)  // 租户ID
                .claim("roles", user.getRoles())
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .signWith(getSigningKeyForTenant(tenantId))  // 每个租户独立密钥
                .compact();
    }
    
    /**
     * 每个租户使用独立的签名密钥
     * 增强安全性：一个租户密钥泄露不影响其他租户
     */
    private Key getSigningKeyForTenant(String tenantId) {
        String key = tenantSecretKeys.get(tenantId);
        return Keys.hmacShaKeyFor(key.getBytes(StandardCharsets.UTF_8));
    }
}

// ============ 方案2: 租户拦截器 ============
/**
 * 在 JWT 验证之前提取租户ID
 */
@Component
public class TenantInterceptor implements HandlerInterceptor {
    
    public static final String TENANT_ID_HEADER = "X-Tenant-ID";
    
    @Override
    public boolean preHandle(HttpServletRequest request,
                            HttpServletResponse response,
                            Object handler) {
        
        // 从 Header 中提取租户ID
        String tenantId = request.getHeader(TENANT_ID_HEADER);
        
        if (tenantId != null) {
            // 存储到请求属性中
            request.setAttribute("tenantId", tenantId);
            
            // 存储到 ThreadLocal（供后续使用）
            TenantContext.setTenantId(tenantId);
        }
        
        return true;
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request,
                               HttpServletResponse response,
                               Object handler,
                               Exception ex) {
        // 清除 ThreadLocal
        TenantContext.clear();
    }
}

/**
 * 租户上下文（ThreadLocal）
 */
public class TenantContext {
    
    private static final ThreadLocal<String> TENANT_ID = new ThreadLocal<>();
    
    public static void setTenantId(String tenantId) {
        TENANT_ID.set(tenantId);
    }
    
    public static String getTenantId() {
        return TENANT_ID.get();
    }
    
    public static void clear() {
        TENANT_ID.remove();
    }
}

// ============ 方案3: 多租户用户服务 ============
@Service
public class MultiTenantUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        
        // 获取当前租户ID
        String tenantId = TenantContext.getTenantId();
        
        // 查询租户下的用户
        User user = userRepository.findByUsernameAndTenantId(username, tenantId)
                .orElseThrow(() -> new UsernameNotFoundException(
                        "User not found in tenant: " + tenantId
                ));
        
        return UserPrincipal.create(user);
    }
}

// ============ 方案4: 租户隔离的数据源 ============
/**
 * 每个租户使用独立的数据源或Schema
 */
@Configuration
public class MultiTenantDataSourceConfig {
    
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource defaultDataSource() {
        return DataSourceBuilder.create().build();
    }
    
    /**
     * 租户路由数据源
     */
    @Bean
    public DataSource routingDataSource() {
        Map<Object, Object> targetDataSources = new HashMap<>();
        
        // 每个租户的数据源
        targetDataSources.put("tenant1", dataSourceTenant1());
        targetDataSources.put("tenant2", dataSourceTenant2());
        // ...
        
        RoutingDataSource routingDataSource = new RoutingDataSource();
        routingDataSource.setTargetDataSources(targetDataSources);
        routingDataSource.setDefaultTargetDataSource(defaultDataSource());
        
        return routingDataSource;
    }
    
    /**
     * 根据租户ID选择数据源
     */
    public static class RoutingDataSource extends AbstractRoutingDataSource {
        @Override
        protected Object determineCurrentLookupKey() {
            return TenantContext.getTenantId();
        }
    }
}
```

### 17.6 思考题

1. **设计题**: 设计一个支持多设备登录的 JWT 系统，要求：
   - 用户可以在多个设备同时登录
   - 可以查看所有已登录设备
   - 可以远程注销指定设备
   - 设备信息包含：设备类型、IP地址、最后活跃时间

2. **安全题**: 分析以下场景：攻击者通过 XSS 攻击窃取了用户的 JWT Token。如何防御？除了基本的 XSS 防护，还有哪些 JWT 特定的安全措施？

3. **性能题**: 在微服务架构中，每个服务都需要验证 JWT。如何避免每个服务都查询数据库？设计一个高性能的认证验证方案。

4. **架构题**: 对比 Session 和 JWT 的优劣。在什么场景下应该使用 Session？什么场景下应该使用 JWT？有没有混合方案？

5. **实现题**: 实现 JWT Token 的自动续期机制。当用户持续活跃时，Token 自动延长；当用户长时间不活跃时，Token 自动过期。

---

## 知识点18: JWT 安全最佳实践

### 18.1 JWT 安全威胁分析

#### 18.1.1 常见攻击向量

```java
/**
 * JWT 安全威胁分类
 * 
 * 1. 签名相关攻击
 *    - 空算法攻击 (None Algorithm)
 *    - 算法混淆攻击 (Algorithm Confusion)
 *    - 密钥猜测攻击
 *    - 签名伪造攻击
 * 
 * 2. Token 处理攻击
 *    - Token 注入攻击
 *    - 重放攻击
 *    - 时间攻击
 *    - Token 泄露
 * 
 * 3. 配置不当
 *    - 弱密钥
 *    - 过期时间过长
 *    - 敏感信息泄露
 *    - 缺少加密
 * 
 * 4. 传输安全
 *    - 中间人攻击 (MITM)
 *    - Token 通过 URL 传输
 *    - Cookie 配置不当
 */
```

#### 18.1.2 空算法攻击详解

```java
/**
 * 空算法攻击示例
 * 
 * 攻击原理：
 * 攻击者将 JWT 的 header.alg 设置为 "none"，
 * 然后伪造签名部分，期望服务器跳过签名验证
 * 
 * 攻击步骤：
 * 1. 原始 JWT: header.payload.signature
 * 2. 修改 header: {"alg":"none"}
 * 3. 伪造签名: ""（空字符串）
 * 4. 拼接: eyJhbGciOiJub25lInQ=.payload.
 * 
 * 防御措施：
 * - 明确指定允许的算法
 * - 拒绝 "none" 算法
 * - 使用白名单验证算法
 */
package com.example.security.jwt.attack;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.SignatureException;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.security.Key;
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

/**
 * 安全的 JWT 验证器 - 防御空算法攻击
 */
@Slf4j
@Component
public class SecureJwtValidator {
    
    @Value("${app.jwt.secret}")
    private String jwtSecret;
    
    /**
     * 允许的算法白名单
     * 只允许使用这些算法
     */
    private static final Set<SignatureAlgorithm> ALLOWED_ALGORITHMS = new HashSet<>(
            Arrays.asList(
                    SignatureAlgorithm.HS256,
                    SignatureAlgorithm.HS384,
                    SignatureAlgorithm.HS512
            )
    );
    
    /**
     * 安全的 Token 验证方法
     * 
     * @param token JWT Token
     * @return Claims 对象
     * @throws JwtException 验证失败
     */
    public Claims validateToken(String token) throws JwtException {
        try {
            // 解析时不带签名验证，先获取 header
            Jwt<Header, Claims> untrusted = Jwts.parserBuilder()
                    .build()
                    .parseClaimsJwt(token.substring(0, token.lastIndexOf('.') + 1));
            
            // 检查算法
            String alg = untrusted.getHeader().getAlgorithm();
            
            // ✅ 防御1: 拒绝 "none" 算法
            if ("none".equalsIgnoreCase(alg)) {
                log.error("JWT 空算法攻击检测: alg=none");
                throw new SecurityException("None algorithm not allowed");
            }
            
            // ✅ 防御2: 验证算法在白名单中
            SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.forName(alg);
            if (!ALLOWED_ALGORITHMS.contains(signatureAlgorithm)) {
                log.error("JWT 算法不在白名单中: {}", alg);
                throw new SecurityException("Algorithm not allowed: " + alg);
            }
            
            // ✅ 防御3: 使用固定密钥验证（不依赖 header 中的 alg）
            Key key = Keys.hmacShaKeyFor(jwtSecret.getBytes(StandardCharsets.UTF_8));
            
            // 重新解析并验证签名
            return Jwts.parserBuilder()
                    .setSigningKey(key)  // 使用固定的密钥
                    .build()
                    .parseClaimsJws(token)
                    .getBody();
            
        } catch (ExpiredJwtException e) {
            log.error("Token 已过期");
            throw e;
        } catch (UnsupportedJwtException e) {
            log.error("不支持的 Token 格式");
            throw e;
        } catch (MalformedJwtException e) {
            log.error("Token 格式错误");
            throw e;
        } catch (SignatureException e) {
            log.error("Token 签名验证失败");
            throw e;
        } catch (IllegalArgumentException e) {
            log.error("Token 参数为空");
            throw e;
        }
    }
    
    /**
     * 最安全的验证方法：直接指定算法和密钥
     * 完全忽略 header 中的算法声明
     */
    public Claims validateTokenSecure(String token) {
        // 使用固定的签名配置，不信任 Token 中的任何算法声明
        return Jwts.parserBuilder()
                .require("alg", "HS256")  // 强制要求算法为 HS256
                .setSigningKey(Keys.hmacShaKeyFor(
                        jwtSecret.getBytes(StandardCharsets.UTF_8)
                ))
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
}
```

#### 18.1.3 算法混淆攻击详解

```java
/**
 * 算法混淆攻击
 * 
 * 攻击原理：
 * 服务器使用 RSA 公钥验证 HMAC 签名，
 * 或者使用 HMAC 密钥验证 RSA 签名
 * 
 * 攻击步骤：
 * 1. 服务器配置错误，混用了算法验证逻辑
 * 2. 攻击者生成使用 "HS256" 的 Token
 * 3. 服务器使用 RSA 公钥作为 HMAC 密钥验证
 * 4. RSA 公钥是公开的，攻击者可以伪造签名
 * 
 * 防御措施：
 * - 明确区分算法类型
 * - 对称加密和非对称加密使用不同的验证路径
 * - 不混用密钥类型
 */
package com.example.security.jwt.attack;

import io.jsonwebtoken.*;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.security.KeyFactory;
import java.security.PublicKey;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

/**
 * 防御算法混淆攻击的 JWT 验证器
 */
@Slf4j
@Component
public class AlgorithmConfusionDefense {
    
    /**
     * HMAC 密钥（对称加密）
     */
    @Value("${app.jwt.hmac-secret}")
    private String hmacSecret;
    
    /**
     * RSA 公钥（非对称加密）
     */
    @Value("${app.jwt.rsa-public-key}")
    private String rsaPublicKey;
    
    /**
     * 安全的 Token 验证方法
     * 根据算法类型选择正确的验证方式
     */
    public Claims validateToken(String token) {
        // 1. 先解析 Header（不验证签名）
        Header header = Jwts.parserBuilder()
                .build()
                .parseClaimsJwt(token.substring(0, token.lastIndexOf('.') + 1))
                .getHeader();
        
        String alg = header.getAlgorithm();
        
        // 2. 根据算法类型选择验证方式
        if (alg.startsWith("HS")) {
            // HMAC 算法：使用对称密钥
            return validateHmacToken(token);
        } else if (alg.startsWith("RS") || alg.startsWith("ES") || alg.startsWith("PS")) {
            // RSA/ECDSA 算法：使用非对称密钥
            return validateRsaToken(token);
        } else {
            throw new SecurityException("不支持的算法: " + alg);
        }
    }
    
    /**
     * 验证 HMAC Token
     * 使用对称密钥
     */
    private Claims validateHmacToken(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(hmacSecret.getBytes())  // 使用 HMAC 密钥
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
    
    /**
     * 验证 RSA Token
     * 使用公钥
     */
    private Claims validateRsaToken(String token) {
        try {
            // 解码 RSA 公钥
            byte[] keyBytes = Base64.getDecoder().decode(rsaPublicKey);
            X509EncodedKeySpec spec = new X509EncodedKeySpec(keyBytes);
            KeyFactory keyFactory = KeyFactory.getInstance("RSA");
            PublicKey publicKey = keyFactory.generatePublic(spec);
            
            // 使用公钥验证
            return Jwts.parserBuilder()
                    .setSigningKey(publicKey)  // 使用 RSA 公钥
                    .build()
                    .parseClaimsJws(token)
                    .getBody();
                    
        } catch (Exception e) {
            log.error("RSA 验证失败", e);
            throw new SecurityException("RSA Token 验证失败");
        }
    }
}
```

### 18.2 JWT 密钥管理

#### 18.2.1 密钥生成最佳实践

```java
package com.example.security.jwt.key;

import javax.crypto.KeyGenerator;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.util.Base64;

/**
 * JWT 密钥管理最佳实践
 * 
 * 密钥生成要求：
 * 1. 足够长的密钥（至少256位）
 * 2. 使用安全的随机数生成器
 * 3. 密钥熵要高（不可预测）
 * 4. 定期轮换密钥
 * 5. 密钥存储安全
 */
public class JwtKeyManagement {
    
    /**
     * 生成安全的 HMAC 密钥
     * 
     * @param keySize 密钥大小（位）：256, 384, 512
     * @return Base64 编码的密钥
     */
    public static String generateHmacKey(int keySize) {
        try {
            // 使用 KeyGenerator 生成密钥
            KeyGenerator keyGenerator = KeyGenerator.getInstance("HmacSHA" + keySize);
            
            // 使用 SecureRandom 生成随机数
            SecureRandom secureRandom = new SecureRandom();
            keyGenerator.init(keySize, secureRandom);
            
            // 生成密钥
            byte[] key = keyGenerator.generateKey().getEncoded();
            
            // Base64 编码
            return Base64.getEncoder().encodeToString(key);
            
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("生成密钥失败", e);
        }
    }
    
    /**
     * 验证密钥强度
     * 
     * @param key 密钥字符串
     * @return true 如果密钥强度足够
     */
    public static boolean isKeyStrong(String key) {
        // 密钥长度检查
        if (key == null || key.length() < 32) {
            return false;
        }
        
        // 熵检查：确保密钥不是简单的模式
        // 例如：避免 "aaaaaaaaaaaaaaaaaaaaaa", "12345678901234567890"
        if (hasLowEntropy(key)) {
            return false;
        }
        
        return true;
    }
    
    /**
     * 检查密钥熵（简单检查）
     * 
     * @param key 密钥
     * @return true 如果熵低（密钥弱）
     */
    private static boolean hasLowEntropy(String key) {
        // 检查重复字符
        long uniqueChars = key.chars().distinct().count();
        if (uniqueChars < key.length() * 0.5) {
            return true;
        }
        
        // 检查常见模式
        String[] commonPatterns = {
            "123456", "password", "secret", "jwt",
            "qwerty", "admin", "key"
        };
        
        for (String pattern : commonPatterns) {
            if (key.toLowerCase().contains(pattern)) {
                return true;
            }
        }
        
        return false;
    }
    
    /**
     * 密钥轮换策略
     * 
     * 策略：
     * 1. 时间轮换：定期轮换（如每90天）
     * 2. 事件轮换：安全事件后立即轮换
     * 3. 渐进轮换：新旧密钥并存一段时间
     * 4. 密钥版本：在 JWT 中标记密钥版本
     */
    public static class KeyRotationStrategy {
        
        /**
         * 密钥版本存储
         */
        private final Map<Integer, String> keys = new ConcurrentHashMap<>();
        
        /**
         * 当前活跃的密钥版本
         */
        private volatile int currentVersion = 1;
        
        /**
         * 添加新密钥（密钥轮换）
         * 
         * @param newKey 新密钥
         * @return 新的密钥版本
         */
        public synchronized int addNewKey(String newKey) {
            // 生成新版本
            int newVersion = currentVersion + 1;
            
            // 添加新密钥
            keys.put(newVersion, newKey);
            
            // 更新当前版本
            currentVersion = newVersion;
            
            // 可选：移除旧版本（保留几个旧版本用于过渡）
            removeOldKeys(newVersion - 3);  // 保留最近3个版本
            
            return newVersion;
        }
        
        /**
         * 获取当前版本的密钥
         */
        public String getCurrentKey() {
            return keys.get(currentVersion);
        }
        
        /**
         * 根据版本获取密钥（验证旧 Token）
         */
        public String getKeyByVersion(int version) {
            return keys.get(version);
        }
        
        /**
         * 移除旧密钥
         */
        private void removeOldKeys(int keepVersion) {
            keys.entrySet().removeIf(entry -> entry.getKey() < keepVersion);
        }
        
        /**
         * 生成包含密钥版本的 JWT
         */
        public String generateTokenWithKeyVersion(String userId, Map<String, Object> claims) {
            Date now = new Date();
            Date expiration = new Date(now.getTime() + 900000);  // 15分钟
            
            // 添加密钥版本到 Claims
            claims.put("kv", currentVersion);  // keyVersion
            
            String key = getCurrentKey();
            
            return Jwts.builder()
                    .setSubject(userId)
                    .setIssuedAt(now)
                    .setExpiration(expiration)
                    .addClaims(claims)
                    .signWith(
                            Keys.hmacShaKeyFor(key.getBytes(StandardCharsets.UTF_8)),
                            SignatureAlgorithm.HS256
                    )
                    .compact();
        }
        
        /**
         * 使用正确的密钥版本验证 Token
         */
        public Claims validateTokenWithKeyVersion(String token) {
            // 解析获取密钥版本
            Claims claims = Jwts.parserBuilder()
                    .build()
                    .parseClaimsJwt(token.substring(0, token.lastIndexOf('.') + 1))
                    .getBody();
            
            Integer keyVersion = claims.get("kv", Integer.class);
            if (keyVersion == null) {
                // 没有版本信息，使用当前版本
                keyVersion = currentVersion;
            }
            
            // 获取对应版本的密钥
            String key = getKeyByVersion(keyVersion);
            if (key == null) {
                throw new SecurityException("密钥版本已过期: " + keyVersion);
            }
            
            // 使用正确的密钥验证
            return Jwts.parserBuilder()
                    .setSigningKey(Keys.hmacShaKeyFor(key.getBytes(StandardCharsets.UTF_8)))
                    .build()
                    .parseClaimsJws(token)
                    .getBody();
        }
    }
    
    public static void main(String[] args) {
        // 生成密钥
        String key256 = generateHmacKey(256);
        String key512 = generateHmacKey(512);
        
        System.out.println("HMAC-SHA256 密钥:");
        System.out.println(key256);
        System.out.println("长度: " + key256.length());
        
        System.out.println("\nHMAC-SHA512 密钥:");
        System.out.println(key512);
        System.out.println("长度: " + key512.length());
        
        // 验证密钥强度
        System.out.println("\n密钥强度验证:");
        System.out.println("HS256密钥: " + isKeyStrong(key256));
        System.out.println("弱密钥: " + isKeyStrong("weakpassword123"));
    }
}
```

#### 18.2.2 密钥存储安全

```java
package com.example.security.jwt.key;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.crypto.Cipher;
import javax.crypto.spec.GCMParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.SecureRandom;
import java.util.Base64;

/**
 * JWT 密钥存储安全
 * 
 * 存储方案：
 * 1. 环境变量（开发环境）
 * 2. 配置中心（生产环境）
 * 3. 密钥管理服务（KMS）
 * 4. 加密存储
 */
@Component
public class SecureKeyStorage {
    
    /**
     * 方案1: 从环境变量读取
     * 
     * 优点：简单、安全
     * 缺点：难以轮换、难以管理
     */
    @Value("${JWT_SECRET:#{environment.JWT_SECRET}}")
    private String jwtSecretFromEnv;
    
    /**
     * 方案2: 从配置中心读取
     * 
     * 优点：集中管理、动态更新
     * 缺点：依赖外部服务
     */
    @Value("${app.config.server}")
    private String configServerUrl;
    
    /**
     * 方案3: 加密存储
     * 
     * 适用于：必须存储在文件或数据库中的场景
     */
    
    /**
     * 主密钥（用于加密 JWT 密钥）
     * 应该从安全的地方获取（如 HSM、KMS）
     */
    private static final String MASTER_KEY = "your-256-bit-master-key";
    
    /**
     * 加密 JWT 密钥
     * 
     * @param plainKey 明文密钥
     * @return 加密后的密钥（Base64）
     */
    public String encryptKey(String plainKey) {
        try {
            // 生成随机 IV
            byte[] iv = new byte[12];
            new SecureRandom().nextBytes(iv);
            
            // 创建密钥
            SecretKeySpec keySpec = new SecretKeySpec(
                    MASTER_KEY.getBytes(StandardCharsets.UTF_8),
                    "AES"
            );
            
            // 初始化加密器
            Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
            cipher.init(Cipher.ENCRYPT_MODE, keySpec, new GCMParameterSpec(128, iv));
            
            // 加密
            byte[] encrypted = cipher.doFinal(plainKey.getBytes(StandardCharsets.UTF_8));
            
            // 组合 IV + 密文
            byte[] combined = new byte[iv.length + encrypted.length];
            System.arraycopy(iv, 0, combined, 0, iv.length);
            System.arraycopy(encrypted, 0, combined, iv.length, encrypted.length);
            
            // Base64 编码
            return Base64.getEncoder().encodeToString(combined);
            
        } catch (Exception e) {
            throw new RuntimeException("密钥加密失败", e);
        }
    }
    
    /**
     * 解密 JWT 密钥
     * 
     * @param encryptedKey 加密的密钥（Base64）
     * @return 明文密钥
     */
    public String decryptKey(String encryptedKey) {
        try {
            // Base64 解码
            byte[] combined = Base64.getDecoder().decode(encryptedKey);
            
            // 提取 IV
            byte[] iv = new byte[12];
            System.arraycopy(combined, 0, iv, 0, iv.length);
            
            // 提取密文
            byte[] encrypted = new byte[combined.length - iv.length];
            System.arraycopy(combined, iv.length, encrypted, 0, encrypted.length);
            
            // 创建密钥
            SecretKeySpec keySpec = new SecretKeySpec(
                    MASTER_KEY.getBytes(StandardCharsets.UTF_8),
                    "AES"
            );
            
            // 初始化解密器
            Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
            cipher.init(Cipher.DECRYPT_MODE, keySpec, new GCMParameterSpec(128, iv));
            
            // 解密
            byte[] decrypted = cipher.doFinal(encrypted);
            
            return new String(decrypted, StandardCharsets.UTF_8);
            
        } catch (Exception e) {
            throw new RuntimeException("密钥解密失败", e);
        }
    }
    
    /**
     * 方案4: 使用外部密钥管理服务（KMS）
     * 
     * 示例：使用 AWS KMS、Azure Key Vault、HashiCorp Vault
     */
    public interface KeyManagementService {
        /**
         * 从 KMS 获取密钥
         * 
         * @param keyId 密钥ID
         * @return 密钥值
         */
        String getKey(String keyId);
        
        /**
         * 生成新密钥并存储到 KMS
         * 
         * @param keyId 密钥ID
         * @return 生成的密钥
         */
        String generateKey(String keyId);
        
        /**
         * 轮换密钥
         * 
         * @param keyId 密钥ID
         * @return 新密钥
         */
        String rotateKey(String keyId);
    }
    
    /**
     * HashiCorp Vault 实现
     */
    public static class VaultKeyManagementService implements KeyManagementService {
        
        private final String vaultUrl;
        private final String vaultToken;
        
        public VaultKeyManagementService(String vaultUrl, String vaultToken) {
            this.vaultUrl = vaultUrl;
            this.vaultToken = vaultToken;
        }
        
        @Override
        public String getKey(String keyId) {
            // 调用 Vault API 获取密钥
            // 这里简化实现
            return restTemplate.getForObject(
                    vaultUrl + "/v1/secret/data/" + keyId,
                    String.class
            );
        }
        
        @Override
        public String generateKey(String keyId) {
            // 生成随机密钥并存储到 Vault
            String newKey = generateRandomKey(256);
            
            // 存储到 Vault
            // ...
            
            return newKey;
        }
        
        @Override
        public String rotateKey(String keyId) {
            // 轮换密钥
            String newKey = generateKey(keyId);
            
            // 通知其他服务更新密钥
            // ...
            
            return newKey;
        }
        
        private String generateRandomKey(int bits) {
            byte[] key = new byte[bits / 8];
            new SecureRandom().nextBytes(key);
            return Base64.getEncoder().encodeToString(key);
        }
    }
}
```

### 18.3 JWT Token 传输安全

#### 18.3.1 Cookie vs LocalStorage

```java
/**
 * JWT Token 存储方案对比
 * 
 * 方案1: LocalStorage / SessionStorage
 * 
 * 优点：
 * - 存储容量大（5-10MB）
 * - 易于访问（JavaScript 可读）
 * - 跨子域共享
 * 
 * 缺点：
 * - ❌ 容易受到 XSS 攻击
 * - ❌ 任何 JavaScript 都能读取
 * - ❌ 难以设置 HttpOnly
 * 
 * 方案2: Cookie
 * 
 * 优点：
 * - ✅ 可以设置 HttpOnly（防止 XSS）
 * - ✅ 可以设置 Secure（HTTPS only）
 * - ✅ 可以设置 SameSite（防止 CSRF）
 * - ✅ 自动随请求发送
 * 
 * 缺点：
 * - 容易受到 CSRF 攻击（如果没有 SameSite）
 * - 容量限制（4KB）
 * - 跨域问题
 * 
 * 方案3: 混合方案
 * - Access Token → Cookie（HttpOnly + Secure + SameSite）
 * - Refresh Token → HttpOnly Cookie
 * - 用户信息 → LocalStorage（非敏感）
 */

/**
 * Cookie 配置最佳实践
 */
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                // ... 其他配置 ...
                
                // 配置 Session 管理
                .sessionManagement()
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                        .and()
                
                // 配置 CSRF（如果使用 Cookie 存储 JWT）
                .csrf()
                        // 如果使用 SameSite=Strict，可以禁用 CSRF
                        .disable()
                
                // 配置 Cookie
                .sessionManagement()
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }
}

/**
 * 响应配置：设置 JWT Cookie
 */
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest request,
                                   HttpServletResponse response) {
        // 验证用户
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        request.getUsername(),
                        request.getPassword()
                )
        );
        
        // 生成 JWT
        String jwt = tokenProvider.generateToken(authentication);
        
        // 设置 Cookie
        Cookie jwtCookie = createSecureCookie("jwt", jwt);
        response.addCookie(jwtCookie);
        
        return ResponseEntity.ok(new ApiResponse(true, "登录成功"));
    }
    
    /**
     * 创建安全的 Cookie
     */
    private Cookie createSecureCookie(String name, String value) {
        Cookie cookie = new Cookie(name, value);
        
        // 1. HttpOnly: 防止 JavaScript 访问（防御 XSS）
        cookie.setHttpOnly(true);
        
        // 2. Secure: 只通过 HTTPS 传输
        cookie.setSecure(true);
        
        // 3. SameSite: 防止 CSRF
        // Strict: 完全阻止跨站 Cookie
        // Lax: 允许安全的跨站请求（如链接跳转）
        // None: 允许跨站（需要 Secure）
        cookie.setSameSite(Cookie.SameSite.STRICT.getAttribute());  // 或 "Strict"
        
        // 4. Path: Cookie 作用路径
        cookie.setPath("/");
        
        // 5. Max-Age: 过期时间（秒）
        cookie.setMaxAge(900);  // 15分钟
        
        // 6. Domain: Cookie 作用域（可选）
        // cookie.setDomain(".example.com");
        
        return cookie;
    }
}

/**
 * 前端配置：不需要手动处理 Token
 * 
 * 使用 Cookie 的优势：
 * - 浏览器自动发送 Cookie
 * - 前端代码不需要手动添加 Authorization Header
 * - 更安全（HttpOnly 防止 XSS）
 */
```

#### 18.3.2 防御 CSRF 和 XSS

```java
/**
 * CSRF 和 XSS 防御策略
 * 
 * CSRF (Cross-Site Request Forgery) 跨站请求伪造
 * 
 * 攻击场景：
 * 1. 用户登录了 bank.com
 * 2. 用户访问了 evil.com
 * 3. evil.com 页面包含 <img src="bank.com/transfer?to=attacker">
 * 4. 浏览器自动发送 Cookie
 * 5. bank.com 执行转账
 * 
 * 防御措施：
 * 1. SameSite Cookie 属性
 * 2. CSRF Token
 * 3. 验证 Origin/Referer Header
 * 4. 双重提交 Cookie
 */

/**
 * 方案1: SameSite Cookie
 */
@Configuration
public class CookieConfig {
    
    @Bean
    public CookieSerializer cookieSerializer() {
        DefaultCookieSerializer serializer = new DefaultCookieSerializer();
        serializer.setCookieName("jwt");
        serializer.setUseHttpOnlyCookie(true);     // HttpOnly
        serializer.setUseSecureCookie(true);       // Secure
        serializer.setSameSite("Strict");          // SameSite=Strict
        serializer.setCookiePath("/");
        serializer.setCookieMaxAge(900);
        return serializer;
    }
}

/**
 * 方案2: CSRF Token（如果需要支持跨站请求）
 */
@Component
public class CsrfTokenGenerator {
    
    /**
     * 生成 CSRF Token
     */
    public String generateCsrfToken() {
        // 使用 UUID 或安全的随机数
        return UUID.randomUUID().toString();
    }
}

/**
 * 自定义 CSRF 过滤器
 */
public class CsrfTokenFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        // 对修改数据的请求验证 CSRF Token
        if (isModifyRequest(request)) {
            String csrfToken = request.getHeader("X-CSRF-Token");
            String sessionToken = (String) request.getSession().getAttribute("csrf_token");
            
            if (!Objects.equals(csrfToken, sessionToken)) {
                response.setStatus(HttpServletResponse.SC_FORBIDDEN);
                response.getWriter().write("Invalid CSRF Token");
                return;
            }
        }
        
        filterChain.doFilter(request, response);
    }
    
    private boolean isModifyRequest(HttpServletRequest request) {
        String method = request.getMethod();
        return "POST".equals(method) ||
               "PUT".equals(method) ||
               "DELETE".equals(method) ||
               "PATCH".equals(method);
    }
}

/**
 * 方案3: 验证 Origin 和 Referer
 */
@Component
public class OriginValidationFilter extends OncePerRequestFilter {
    
    @Value("${app.allowed-origins}")
    private String[] allowedOrigins;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        // 对修改数据的请求验证 Origin
        if (isModifyRequest(request)) {
            String origin = request.getHeader("Origin");
            
            if (origin != null && !isAllowedOrigin(origin)) {
                response.setStatus(HttpServletResponse.SC_FORBIDDEN);
                response.getWriter().write("Origin not allowed");
                return;
            }
        }
        
        filterChain.doFilter(request, response);
    }
    
    private boolean isAllowedOrigin(String origin) {
        return Arrays.asList(allowedOrigins).contains(origin);
    }
    
    private boolean isModifyRequest(HttpServletRequest request) {
        String method = request.getMethod();
        return "POST".equals(method) ||
               "PUT".equals(method) ||
               "DELETE".equals(method);
    }
}

/**
 * XSS (Cross-Site Scripting) 跨站脚本攻击
 * 
 * 攻击场景：
 * 1. 攻击者在论坛注入恶意脚本 <script>alert(document.cookie)</script>
 * 2. 其他用户浏览论坛
 * 3. 恶意脚本执行，窃取 Cookie
 * 
 * 防御措施：
 * 1. 输入验证和过滤
 * 2. 输出编码
 * 3. Content-Security-Policy (CSP)
 * 4. HttpOnly Cookie
 */

/**
 * 输入过滤
 */
@Component
public class XssInputFilter extends OncePerRequestFilter {
    
    /**
     * XSS 危险字符模式
     */
    private static final Pattern[] XSS_PATTERNS = {
            Pattern.compile("<script>(.*?)</script>", Pattern.CASE_INSENSITIVE),
            Pattern.compile("src[\r\n]*=[\r\n]*\\\'(.*?)\\\'", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
            Pattern.compile("src[\r\n]*=[\r\n]*\\\"(.*?)\\\"", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
            Pattern.compile("</script>", Pattern.CASE_INSENSITIVE),
            Pattern.compile("<script(.*?)>", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
            Pattern.compile("eval\\((.*?)\\)", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
            Pattern.compile("expression\\((.*?)\\)", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
            Pattern.compile("javascript:", Pattern.CASE_INSENSITIVE),
            Pattern.compile("vbscript:", Pattern.CASE_INSENSITIVE),
            Pattern.compile("onload(.*?)=", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL)
    };
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        // 包装请求，过滤 XSS
        XssHttpServletRequestWrapper wrappedRequest = 
                new XssHttpServletRequestWrapper(request);
        
        filterChain.doFilter(wrappedRequest, response);
    }
    
    /**
     * XSS 过滤工具
     */
    public static String stripXSS(String value) {
        if (value == null) {
            return null;
        }
        
        // 移除危险的 XSS 模式
        for (Pattern pattern : XSS_PATTERNS) {
            value = pattern.matcher(value).replaceAll("");
        }
        
        // 移除特殊字符
        value = value.replaceAll("", "");
        value = value.replaceAll("", "");
        
        return value;
    }
}

/**
 * XSS 请求包装器
 */
public class XssHttpServletRequestWrapper extends HttpServletRequestWrapper {
    
    public XssHttpServletRequestWrapper(HttpServletRequest request) {
        super(request);
    }
    
    @Override
    public String[] getParameterValues(String parameter) {
        String[] values = super.getParameterValues(parameter);
        
        if (values == null) {
            return null;
        }
        
        String[] encodedValues = new String[values.length];
        for (int i = 0; i < values.length; i++) {
            encodedValues[i] = stripXSS(values[i]);
        }
        
        return encodedValues;
    }
    
    @Override
    public String getParameter(String parameter) {
        String value = super.getParameter(parameter);
        return stripXSS(value);
    }
    
    @Override
    public String getHeader(String name) {
        String value = super.getHeader(name);
        return stripXSS(value);
    }
}

/**
 * Content-Security-Policy (CSP) Header
 */
@Configuration
public class SecurityHeadersConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                // ... 其他配置 ...
                
                // 添加 CSP Header
                .headers()
                        .contentSecurityPolicy("default-src 'self'; script-src 'self' https://trusted.cdn.com; style-src 'self' 'unsafe-inline'")
                        .and()
                        .frameOptions().deny()
                        .and()
                        .xssProtection().xssProtectionEnabled(true).block(true);
    }
}
```

### 18.4 JWT Token 撤销机制

```java
/**
 * JWT Token 撤销方案
 * 
 * 挑战：JWT 是无状态的，一旦签发就无法撤销
 * 
 * 解决方案：
 * 1. Token 黑名单（Redis）
 * 2. Token 版本号
 * 3. 短期 Token + Refresh Token
 * 4. Token 白名单
 */

// ============ 方案1: Token 黑名单 ============
@Service
public class TokenBlacklistService {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    private static final String BLACKLIST_PREFIX = "jwt:blacklist:";
    private static final long BLACKLIST_TTL_DAYS = 7;
    
    /**
     * 将 Token 加入黑名单
     * 
     * @param token JWT Token
     * @param expiration Token 过期时间
     */
    public void addToBlacklist(String token, Date expiration) {
        // 计算 TTL（Token 剩余有效时间）
        long ttl = expiration.getTime() - System.currentTimeMillis();
        
        if (ttl > 0) {
            String key = BLACKLIST_PREFIX + token;
            
            // 存储到 Redis，设置过期时间
            redisTemplate.opsForValue().set(
                    key,
                    "blacklisted",
                    ttl,
                    TimeUnit.MILLISECONDS
            );
        }
    }
    
    /**
     * 检查 Token 是否在黑名单中
     * 
     * @param token JWT Token
     * @return true 如果已黑名单
     */
    public boolean isBlacklisted(String token) {
        String key = BLACKLIST_PREFIX + token;
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }
    
    /**
     * 批量加入黑名单（用户注销所有设备）
     * 
     * @param userId 用户ID
     */
    public void blacklistAllUserTokens(Long userId) {
        // 获取用户的所有 Token（需要提前维护用户-Token 映射）
        Set<String> tokens = getUserTokens(userId);
        
        // 批量加入黑名单
        for (String token : tokens) {
            try {
                Date expiration = getExpirationFromToken(token);
                addToBlacklist(token, expiration);
            } catch (Exception e) {
                // 忽略无效 Token
            }
        }
    }
    
    private Set<String> getUserTokens(Long userId) {
        String key = "user:tokens:" + userId;
        return redisTemplate.opsForSet().members(key);
    }
    
    private Date getExpirationFromToken(String token) {
        // 解析 Token 获取过期时间
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        
        return claims.getExpiration();
    }
}

/**
 * 在 JWT 过滤器中检查黑名单
 */
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Autowired
    private TokenBlacklistService blacklistService;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        try {
            String jwt = extractJwtFromRequest(request);
            
            if (StringUtils.hasText(jwt)) {
                // ✅ 检查黑名单
                if (blacklistService.isBlacklisted(jwt)) {
                    response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
                    response.getWriter().write("Token has been revoked");
                    return;
                }
                
                // 继续正常验证...
            }
        } catch (Exception ex) {
            // 处理异常
        }
        
        filterChain.doFilter(request, response);
    }
}

// ============ 方案2: Token 版本号 ============
/**
 * User 实体中增加 tokenVersion 字段
 */
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String username;
    private String password;
    
    /**
     * Token 版本号
     * 每次注销/修改密码时递增
     */
    @Column(name = "token_version")
    private Long tokenVersion = 0L;
    
    /**
     * 注销时递增版本号（使所有旧 Token 失效）
     */
    public void incrementTokenVersion() {
        this.tokenVersion++;
    }
}

/**
 * JWT 生成时包含版本号
 */
public String generateToken(User user) {
    Date now = new Date();
    Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);
    
    Map<String, Object> claims = new HashMap<>();
    claims.put("tv", user.getTokenVersion());  // tokenVersion
    
    return Jwts.builder()
            .setSubject(user.getId().toString())
            .setIssuedAt(now)
            .setExpiration(expiryDate)
            .addClaims(claims)
            .signWith(getSigningKey(), SignatureAlgorithm.HS256)
            .compact();
}

/**
 * 验证时检查版本号
 */
public boolean validateToken(String token) {
    try {
        // 解析 Token
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        
        // 获取 Token 中的版本号
        Long tokenVersion = claims.get("tv", Long.class);
        
        // 获取当前用户的版本号
        Long userId = Long.parseLong(claims.getSubject());
        User user = userRepository.findById(userId).orElse(null);
        
        if (user == null) {
            return false;
        }
        
        // ✅ 比对版本号
        if (!Objects.equals(tokenVersion, user.getTokenVersion())) {
            return false;  // 版本不匹配，Token 已失效
        }
        
        return true;
        
    } catch (Exception e) {
        return false;
    }
}

/**
 * 注销接口
 */
@PostMapping("/logout")
public ResponseEntity<?> logoutUser(Principal principal) {
    // 获取当前用户
    User user = (User) ((UsernamePasswordAuthenticationToken) principal).getPrincipal();
    
    // 递增 Token 版本号
    user.incrementTokenVersion();
    userRepository.save(user);
    
    // 清除 SecurityContext
    SecurityContextHolder.clearContext();
    
    return ResponseEntity.ok(new ApiResponse(true, "注销成功"));
}

// ============ 方案3: 短期 Token + Refresh Token ============
/**
 * Token 生命周期管理
 */
@Configuration
public class TokenLifecycleConfig {
    
    /**
     * Access Token: 短期（5-15分钟）
     * Refresh Token: 长期（7-30天）
     * 
     * 撤销策略：
     * - 撤销 Refresh Token → Access Token 很快过期
     * - 依赖 Refresh Token 的短生命周期
     */
    public static final long ACCESS_TOKEN_EXPIRATION = 5 * 60 * 1000;      // 5分钟
    public static final long REFRESH_TOKEN_EXPIRATION = 7 * 24 * 60 * 60 * 1000L; // 7天
}

/**
 * Refresh Token 存储（可撤销）
 */
@Service
public class RefreshTokenService {
    
    @Autowired
    private RefreshTokenRepository refreshTokenRepository;
    
    /**
     * 生成 Refresh Token
     */
    public RefreshToken createRefreshToken(Long userId) {
        RefreshToken refreshToken = new RefreshToken();
        refreshToken.setUser(userRepository.findById(userId).orElseThrow());
        refreshToken.setToken(UUID.randomUUID().toString());
        refreshToken.setExpiryDate(
                Instant.now().plusMillis(TokenLifecycleConfig.REFRESH_TOKEN_EXPIRATION)
        );
        
        return refreshTokenRepository.save(refreshToken);
    }
    
    /**
     * 验证 Refresh Token
     */
    public RefreshToken verifyExpiration(RefreshToken token) {
        if (token.getExpiryDate().compareTo(Instant.now()) < 0) {
            refreshTokenRepository.delete(token);
            throw new RuntimeException("Refresh Token 已过期");
        }
        
        return token;
    }
    
    /**
     * 撤销 Refresh Token（删除）
     */
    public void deleteByToken(String token) {
        refreshTokenRepository.findByToken(token).ifPresent(refreshTokenRepository::delete);
    }
    
    /**
     * 撤销用户的所有 Refresh Token
     */
    public void deleteAllUserTokens(Long userId) {
        refreshTokenRepository.deleteByUserId(userId);
    }
}

// ============ 方案4: Token 白名单 ============
/**
 * 只允许白名单中的 Token 通过
 * 
 * 优点：
 * - 更安全（默认拒绝）
 * - 易于审计
 * 
 * 缺点：
 * - 存储开销大
 * - 性能影响
 */
@Service
public class TokenWhitelistService {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    private static final String WHITELIST_PREFIX = "jwt:whitelist:";
    
    /**
     * 添加 Token 到白名单
     */
    public void addToWhitelist(String token, Date expiration) {
        long ttl = expiration.getTime() - System.currentTimeMillis();
        
        if (ttl > 0) {
            String key = WHITELIST_PREFIX + token;
            redisTemplate.opsForValue().set(
                    key,
                    "valid",
                    ttl,
                    TimeUnit.MILLISECONDS
            );
        }
    }
    
    /**
     * 检查 Token 是否在白名单中
     */
    public boolean isWhitelisted(String token) {
        String key = WHITELIST_PREFIX + token;
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }
    
    /**
     * 从白名单中移除（撤销）
     */
    public void removeFromWhitelist(String token) {
        String key = WHITELIST_PREFIX + token;
        redisTemplate.delete(key);
    }
}
```

### 18.5 常见问题解答 (FAQ)

**Q1: JWT 应该存储在 Cookie 还是 LocalStorage 中？**

A: **安全对比**：

| 特性 | Cookie (HttpOnly) | LocalStorage |
|------|------------------|--------------|
| XSS 防护 | ✅ 自动 | ❌ 需手动 |
| CSRF 防护 | ❌ 需要额外措施 | ✅ 自动 |
| 跨域 | ❌ 受限 | ✅ 灵活 |
| 容量 | 4KB | 5-10MB |
| 自动发送 | ✅ 是 | ❌ 需手动 |

**推荐方案**：

```javascript
// 混合方案（最佳实践）
// 1. Access Token → HttpOnly Cookie
//    - 防止 XSS 窃取
//    - 配置 SameSite=Strict 防止 CSRF

// 2. Refresh Token → HttpOnly Cookie + Path=/auth/refresh
//    - 只在刷新端点可访问
//    - 额外的安全层

// 3. 用户信息（非敏感）→ LocalStorage
//    - 用户名、头像URL等
//    - 便于前端使用

// 前端示例
async function login(username, password) {
    // 登录请求会自动设置 Cookie
    await fetch('/api/auth/login', {
        method: 'POST',
        body: JSON.stringify({ username, password }),
        credentials: 'include'  // 包含 Cookie
    });
}

// API 请求
async function fetchData() {
    // Cookie 自动发送
    const response = await fetch('/api/data', {
        credentials: 'include'
    });
    return response.json();
}
```

**Q2: 如何防止 JWT Token 被窃取后的滥用？**

A: **多层防御策略**：

```java
/**
 * 防御措施
 */

// 1. 绑定 IP 地址（Token 包含 IP Hash）
public String generateToken(String userId, String ipAddress) {
    Map<String, Object> claims = new HashMap<>();
    
    // 存储用户 IP 的 Hash（不存储明文 IP）
    claims.put("ip_hash", hashIp(ipAddress));
    
    return Jwts.builder()
            .setSubject(userId)
            .addClaims(claims)
            .signWith(key)
            .compact();
}

/**
 * 验证时检查 IP
 */
public boolean validateTokenWithIP(String token, String requestIP) {
    Claims claims = parseToken(token);
    String tokenIPHash = claims.get("ip_hash", String.class);
    String requestIPHash = hashIp(requestIP);
    
    if (!Objects.equals(tokenIPHash, requestIPHash)) {
        // IP 不匹配，可能被窃取
        return false;
    }
    
    return true;
}

// 2. 绑定 User-Agent
public String generateToken(String userId, String userAgent) {
    Map<String, Object> claims = new HashMap<>();
    claims.put("ua_hash", hashUserAgent(userAgent));
    
    return Jwts.builder()
            .setSubject(userId)
            .addClaims(claims)
            .signWith(key)
            .compact();
}

// 3. 设备指纹
/**
 * 综合浏览器特征生成设备指纹
 */
public String generateDeviceFingerprint(HttpServletRequest request) {
    StringBuilder fingerprint = new StringBuilder();
    
    // User-Agent
    fingerprint.append(request.getHeader("User-Agent"));
    
    // Accept-Language
    fingerprint.append("|").append(request.getHeader("Accept-Language"));
    
    // Screen Resolution（从请求参数获取）
    fingerprint.append("|").append(request.getParameter("screen"));
    
    // Platform
    fingerprint.append("|").append(request.getParameter("platform"));
    
    // Hash
    return DigestUtils.sha256Hex(fingerprint.toString());
}

// 4. 异常检测
/**
 * 检测异常行为
 */
@Service
public class AnomalyDetectionService {
    
    /**
     * 检测异常登录
     * 
     * 检查项：
     * 1. IP 地理位置突变
     * 2. 设备指纹变化
     * 3. 登录时间异常
     * 4. 请求频率异常
     */
    public RiskLevel detectAnomaly(String userId, HttpServletRequest request) {
        RiskLevel risk = RiskLevel.LOW;
        
        // 1. IP 地理位置检查
        String currentIP = getClientIP(request);
        Location currentLocation = ipService.getLocation(currentIP);
        Location lastLocation = userService.getLastLoginLocation(userId);
        
        if (lastLocation != null) {
            double distance = calculateDistance(currentLocation, lastLocation);
            long timeDiff = System.currentTimeMillis() - userService.getLastLoginTime(userId);
            
            // 如果在短时间内跨越了长距离（不可能的速度）
            if (distance > 1000 && timeDiff < TimeUnit.HOURS.toMillis(1)) {
                risk = RiskLevel.HIGH;  // 不可能的速度
            }
        }
        
        // 2. 设备指纹检查
        String currentFingerprint = generateDeviceFingerprint(request);
        String lastFingerprint = userService.getLastDeviceFingerprint(userId);
        
        if (!Objects.equals(currentFingerprint, lastFingerprint)) {
            risk = RiskLevel.MEDIUM;  // 设备变化
        }
        
        // 3. 时间检查
        int hour = LocalDateTime.now().getHour();
        if (hour < 6 || hour > 23) {
            risk = RiskLevel.values()[Math.max(risk.ordinal(), RiskLevel.MEDIUM.ordinal())];
        }
        
        return risk;
    }
    
    public enum RiskLevel {
        LOW,    // 正常
        MEDIUM, // 需要额外验证（如短信验证码）
        HIGH    // 拒绝访问
    }
}

/**
 * 登录时进行风险评估
 */
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request,
                               HttpServletRequest httpRequest) {
    
    // 1. 验证用户凭据
    Authentication authentication = authenticationManager.authenticate(...);
    
    // 2. 风险评估
    RiskLevel risk = anomalyDetectionService.detectAnomaly(
            user.getId(),
            httpRequest
    );
    
    switch (risk) {
        case HIGH:
            // 高风险：拒绝登录
            return ResponseEntity.status(403)
                    .body(new ApiResponse(false, "检测到异常登录，请联系客服"));
        
        case MEDIUM:
            // 中风险：要求额外验证
            String verificationCode = sendVerificationCode(user.getPhone());
            return ResponseEntity.ok()
                    .body(new ApiResponse(false, "需要验证码", verificationCode));
        
        case LOW:
        default:
            // 低风险：正常登录
            String token = tokenProvider.generateToken(authentication);
            return ResponseEntity.ok(new JwtAuthenticationResponse(token));
    }
}
```

**Q3: JWT 的过期时间应该设置多长？**

A: **过期时间策略**：

```java
/**
 * Token 过期时间配置建议
 * 
 * 原则：
 * 1. 安全性和用户体验的平衡
 * 2. 不同场景使用不同的过期时间
 * 3. 敏感操作使用更短的过期时间
 */

/**
 * 配置示例
 */
@Configuration
@ConfigurationProperties(prefix = "app.jwt")
public class JwtExpirationConfig {
    
    /**
     * 普通 Access Token
     * 
     * 场景：一般 API 访问
     * 过期时间：15 分钟
     * 原因：平衡用户体验和安全性
     */
    private long accessExpiration = 15 * 60 * 1000;  // 15分钟
    
    /**
     * Refresh Token
     * 
     * 场景：获取新的 Access Token
     * 过期时间：7-30 天
     * 原因：长期免密登录，但可撤销
     */
    private long refreshExpiration = 7 * 24 * 60 * 60 * 1000L;  // 7天
    
    /**
     * 敏感操作 Token
     * 
     * 场景：支付、修改密码等敏感操作
     * 过期时间：5 分钟
     * 原因：高安全要求
     */
    private long sensitiveExpiration = 5 * 60 * 1000;  // 5分钟
    
    /**
     * 记住我 Token
     * 
     * 场景：用户选择"记住我"
     * 过期时间：30 天
     * 原因：长期登录，需要设备绑定
     */
    private long rememberMeExpiration = 30 * 24 * 60 * 60 * 1000L;  // 30天
    
    /**
     * 一次性 Token
     * 
     * 场景：邮件验证、密码重置
     * 过期时间：15-60 分钟
     * 原因：防止重放攻击
     */
    private long oneTimeExpiration = 30 * 60 * 1000;  // 30分钟
    
    // getters and setters
}

/**
 * 动态过期时间
 */
@Service
public class DynamicTokenExpirationService {
    
    /**
     * 根据用户角色调整过期时间
     * 
     * 管理员：更短的过期时间（更高安全要求）
     * 普通用户：标准过期时间
     * VIP用户：更长的过期时间（更好的用户体验）
     */
    public long getExpirationForUser(User user) {
        if (user.isAdmin()) {
            return 10 * 60 * 1000;  // 10分钟
        } else if (user.isVip()) {
            return 30 * 60 * 1000;  // 30分钟
        } else {
            return 15 * 60 * 1000;  // 15分钟
        }
    }
    
    /**
     * 根据操作类型调整过期时间
     */
    public long getExpirationForOperation(OperationType operation) {
        switch (operation) {
            case PAYMENT:
                return 5 * 60 * 1000;  // 5分钟
            case PASSWORD_CHANGE:
                return 5 * 60 * 1000;  // 5分钟
            case FILE_DOWNLOAD:
                return 30 * 60 * 1000;  // 30分钟
            default:
                return 15 * 60 * 1000;  // 15分钟
        }
    }
}
```

**Q4: 如何实现 JWT 的多级安全检查？**

A: **多级安全检查机制**：

```java
/**
 * 多级安全检查
 * 
 * 层级：
 * 1. 基础验证：签名、过期时间
 * 2. 撤销检查：黑名单、版本号
 * 3. 环境验证：IP、设备指纹
 * 4. 行为分析：异常检测
 * 5. 自适应验证：风险评分
 */

/**
 * 综合安全验证器
 */
@Service
public class ComprehensiveSecurityValidator {
    
    @Autowired
    private TokenBlacklistService blacklistService;
    
    @Autowired
    private AnomalyDetectionService anomalyService;
    
    @Autowired
    private UserRepository userRepository;
    
    /**
     * 多级验证
     * 
     * @param token JWT Token
     * @param request HTTP 请求
     * @return 验证结果
     */
    public ValidationResult validateToken(String token, 
                                          HttpServletRequest request) {
        ValidationResult result = new ValidationResult();
        
        try {
            // ========== 第1级：基础验证 ==========
            Claims claims = validateBasic(token);
            result.setClaims(claims);
            
            // ========== 第2级：撤销检查 ==========
            if (blacklistService.isBlacklisted(token)) {
                result.setValid(false);
                result.setReason("Token已被撤销");
                return result;
            }
            
            // ========== 第3级：版本号验证 ==========
            Long userId = Long.parseLong(claims.getSubject());
            User user = userRepository.findById(userId).orElse(null);
            
            if (user == null) {
                result.setValid(false);
                result.setReason("用户不存在");
                return result;
            }
            
            Long tokenVersion = claims.get("tv", Long.class);
            if (!Objects.equals(tokenVersion, user.getTokenVersion())) {
                result.setValid(false);
                result.setReason("Token版本不匹配");
                return result;
            }
            
            // ========== 第4级：环境验证 ==========
            String tokenIPHash = claims.get("ip_hash", String.class);
            String requestIP = getClientIP(request);
            String requestIPHash = hashIp(requestIP);
            
            if (!Objects.equals(tokenIPHash, requestIPHash)) {
                // IP 变化，记录警告但仍允许（宽松模式）
                result.addWarning("IP地址变化");
            }
            
            String tokenUAHash = claims.get("ua_hash", String.class);
            String requestUA = request.getHeader("User-Agent");
            String requestUAHash = hashUserAgent(requestUA);
            
            if (!Objects.equals(tokenUAHash, requestUAHash)) {
                result.addWarning("浏览器变化");
            }
            
            // ========== 第5级：行为分析 ==========
            RiskLevel risk = anomalyService.detectAnomaly(userId, request);
            
            if (risk == RiskLevel.HIGH) {
                result.setValid(false);
                result.setReason("检测到高风险行为");
                return result;
            } else if (risk == RiskLevel.MEDIUM) {
                result.addWarning("检测到异常行为");
            }
            
            // ========== 第6级：频率限制 ==========
            if (rateLimitService.isRateLimited(userId)) {
                result.setValid(false);
                result.setReason("请求过于频繁");
                return result;
            }
            
            // 所有检查通过
            result.setValid(true);
            result.setUser(user);
            
        } catch (ExpiredJwtException e) {
            result.setValid(false);
            result.setReason("Token已过期");
        } catch (Exception e) {
            result.setValid(false);
            result.setReason("验证失败: " + e.getMessage());
        }
        
        return result;
    }
    
    /**
     * 基础验证（签名、过期）
     */
    private Claims validateBasic(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
    
    /**
     * 验证结果
     */
    public static class ValidationResult {
        private boolean valid;
        private String reason;
        private List<String> warnings = new ArrayList<>();
        private Claims claims;
        private User user;
        
        // getters and setters
        
        public boolean hasWarnings() {
            return !warnings.isEmpty();
        }
    }
}
```

**Q5: JWT 在微服务架构中如何安全传输？**

A: **微服务 JWT 传输方案**：

```java
/**
 * 微服务架构中的 JWT 传输
 * 
 * 挑战：
 * 1. 服务间通信需要传递用户身份
 * 2. 避免在每个服务都验证 JWT
 * 3. 防止 Token 在服务间传输时泄露
 * 
 * 解决方案：
 */

// ============ 方案1: Token 透传（最简单） ============
/**
 * 用户 → API Gateway → Service A → Service B
 * 
 * Token 在整个调用链中传递
 * 每个服务独立验证 Token
 */
@ServiceA {
    public void callServiceB(String jwt) {
        // 将 JWT 传递给 Service B
        serviceB.doSomething(jwt);
    }
}

// ============ 方案2: 服务账号（Service Account） ============
/**
 * 用户 → API Gateway (验证用户 Token)
 * → Service A (使用服务账号 Token 调用 Service B)
 * 
 * 用户 Token 和服务 Token 分离
 */
@Configuration
public class ServiceAccountConfig {
    
    /**
     * 服务间通信使用独立的 Token
     */
    @Bean
    public String serviceAccountToken() {
        // 服务账号的 JWT
        Map<String, Object> claims = new HashMap<>();
        claims.put("service", "order-service");
        claims.put("permissions", Arrays.asList("read:product", "write:order"));
        
        return Jwts.builder()
                .setSubject("service-account")
                .addClaims(claims)
                .signWith(serviceSigningKey)
                .compact();
    }
}

@Service
public class OrderService {
    
    @Value("${service.account.token}")
    private String serviceToken;
    
    /**
     * 调用其他服务
     */
    public Product getProduct(Long productId) {
        // 使用服务账号 Token（不是用户 Token）
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer " + serviceToken);
        
        HttpEntity<Void> entity = new HttpEntity<>(headers);
        
        return restTemplate.exchange(
                "http://product-service/api/products/" + productId,
                HttpMethod.GET,
                entity,
                Product.class
        ).getBody();
    }
}

// ============ 方案3: 上下文传递（推荐） ============
/**
 * 用户 → API Gateway (验证 Token → 提取用户信息)
 * → Service A (传递用户上下文，不传递 Token)
 * → Service B (传递用户上下文)
 */

/**
 * 用户上下文对象
 */
public class UserContext {
    private Long userId;
    private String username;
    private List<String> roles;
    private Map<String, String> metadata;
    
    // 不包含敏感信息（如 Token）
}

/**
 * API Gateway：验证 Token 并创建上下文
 */
@Component
public class GatewayJwtFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        String jwt = extractJwtFromRequest(request);
        
        if (jwt != null) {
            // 验证 Token
            Claims claims = tokenProvider.validateToken(jwt);
            
            // 创建用户上下文（不传递原始 Token）
            UserContext context = UserContext.builder()
                    .userId(Long.parseLong(claims.getSubject()))
                    .username(claims.get("username", String.class))
                    .roles((List<String>) claims.get("roles"))
                    .build();
            
            // 存储到请求 Attribute
            request.setAttribute("userContext", context);
            
            // 或者存储到 ThreadLocal
            UserContextHolder.setContext(context);
        }
        
        filterChain.doFilter(request, response);
    }
}

/**
 * 服务间传递：使用 HTTP Header
 */
@Service
public class OrderService {
    
    public void createOrder(OrderRequest request, UserContext userContext) {
        // 调用 Inventory Service
        // 传递用户上下文（不是 Token）
        
        HttpHeaders headers = new HttpHeaders();
        headers.set("X-User-Id", userContext.getUserId().toString());
        headers.set("X-Username", userContext.getUsername());
        headers.set("X-User-Roles", String.join(",", userContext.getRoles()));
        
        HttpEntity<OrderRequest> entity = new HttpEntity<>(request, headers);
        
        restTemplate.exchange(
                "http://inventory-service/api/inventory/check",
                HttpMethod.POST,
                entity,
                Void.class
        );
    }
}

/**
 * Inventory Service：从 Header 读取上下文
 */
@Component
public class UserContextInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request,
                            HttpServletResponse response,
                            Object handler) {
        
        // 从 Header 重建用户上下文
        String userIdHeader = request.getHeader("X-User-Id");
        String usernameHeader = request.getHeader("X-Username");
        String rolesHeader = request.getHeader("X-User-Roles");
        
        if (userIdHeader != null) {
            UserContext context = UserContext.builder()
                    .userId(Long.parseLong(userIdHeader))
                    .username(usernameHeader)
                    .roles(Arrays.asList(rolesHeader.split(",")))
                    .build();
            
            UserContextHolder.setContext(context);
        }
        
        return true;
    }
}

// ============ 方案4: Sidecar 模式（最安全） ============
/**
 * 每个 Service 旁边部署一个 Sidecar 代理
 * 
 * 用户 → API Gateway
 * → Sidecar A → Service A
 * → Sidecar B → Service B
 * 
 * Sidecar 负责：
 * 1. Token 验证
 * 2. 用户上下文传递
 * 3. 服务间认证
 */

/**
 * Sidecar 代理配置（使用 Envoy 或 Istio）
 */
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: order-service
spec:
  host: order-service
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL  # 服务间 mTLS
---
apiVersion: security.istio.io/v1beta1
kind: RequestAuthentication
metadata:
  name: jwt-example
spec:
  selector:
    matchLabels:
      app: order-service
  jwtRules:
  - issuer: "https://auth.example.com"
    jwks: "https://auth.example.com/.well-known/jwks.json"
    forwardOriginalToken: true  # 转发原始 Token
```

### 18.6 思考题

1. **安全设计题**: 设计一个高安全级别的 JWT 系统，用于银行转账系统。要求：
   - 多因素认证（密码 + 短信验证码 + UKey）
   - Token 绑定设备指纹
   - 实时风险评估
   - 异常行为阻断

2. **架构题**: 在一个分布式系统中，有数百个微服务。如何设计一个统一的 JWT 认证架构，既保证安全性，又保证高性能？

3. **攻防题**: 分析以下攻击场景：
   - 攻击者通过 SQL 注入获取了数据库中的用户密码哈希
   - 攻击者尝试使用这些哈希伪造 JWT Token
   - 如何防御这种攻击？

4. **合规题**: 设计一个符合 GDPR 要求的 JWT 系统，支持：
   - 用户数据删除权（Right to Erasure）
   - 数据可移植权（Right to Data Portability）
   - 撤回同意权（Right to Withdraw Consent）

5. **性能题**: 设计一个支持 QPS 100,000 的 JWT 验证系统。如何优化？使用什么架构？

---

## 模块6 - OAuth2 与 SSO

---

## 知识点19: OAuth2 基础

### 19.1 OAuth2 概述

#### 19.1.1 什么是 OAuth2

OAuth 2.0 (Open Authorization 2.0) 是一个开放标准，用于授权。它允许用户让第三方应用访问他们在其他服务上的资源，而无需将用户名和密码提供给第三方应用。

**OAuth2 的核心思想**：
- 用户（Resource Owner）授权第三方应用（Client）访问其在资源服务器（Resource Server）上的资源
- 授权通过授权服务器（Authorization Server）完成
- 第三方应用获得访问令牌（Access Token），而不是用户凭据

#### 19.1.2 OAuth2 的历史

**OAuth 1.0 (2010)**:
- 复杂的签名过程
- 需要加密请求
- 难以实现

**OAuth 2.0 (2012)**:
- 简化流程
- 基于 TLS/HTTPS
- 广泛采用

**OAuth 2.1 (草案)**:
- 增强 PKCE
- 移除隐式流程
- 强化安全要求
- 改进刷新令牌

#### 19.1.3 OAuth2 的角色

```java
/**
 * OAuth2 的四个核心角色
 * 
 * 1. Resource Owner (资源所有者)
 *    - 能够授权访问受保护资源的实体
 *    - 通常是最终用户
 * 
 * 2. Client (客户端)
 *    - 代表资源所有者请求访问受保护资源的应用程序
 *    - 类型：Web应用、移动应用、SPA、机器对机器
 * 
 * 3. Authorization Server (授权服务器)
 *    - 服务器在成功验证资源所有者并获得授权后颁发访问令牌
 *    - 负责：用户认证、授权、令牌颁发
 * 
 * 4. Resource Server (资源服务器)
 *    - 托管受保护资源的服务器
 *    - 接受访问令牌并提供资源
 */

/**
 * OAuth2 令牌
 */
public enum OAuth2TokenType {
    /**
     * Access Token (访问令牌)
     * - 用于访问受保护资源
     * - 生命周期较短（通常1小时）
     * - JWT 或 Opaque Token
     */
    ACCESS_TOKEN,
    
    /**
     * Refresh Token (刷新令牌)
     * - 用于获取新的 Access Token
     * - 生命周期较长（数天至数月）
     * - 必须安全存储
     */
    REFRESH_TOKEN,
    
    /**
     * ID Token (身份令牌)
     * - OpenID Connect 扩展
     * - 包含用户身份信息
     * - JWT 格式
     */
    ID_TOKEN
}
```

### 19.2 OAuth2 授权流程

#### 19.2.1 Authorization Code Flow（授权码模式）

```java
/**
 * Authorization Code Flow (授权码模式)
 * 
 * 适用场景：
 * - Web 应用（有后端服务器）
 * - 最安全的流程
 * - Token 不会暴露给浏览器
 * 
 * 流程步骤：
 */
public class AuthorizationCodeFlow {
    
    /**
     * 步骤1: 客户端将用户重定向到授权服务器
     * 
     * 请求参数：
     * - response_type=code: 表示请求授权码
     * - client_id: 客户端标识
     * - redirect_uri: 重定向URI
     * - scope: 请求的权限范围
     * - state: 随机值，防止CSRF攻击
     */
    public String buildAuthorizationRequest() {
        UriComponentsBuilder builder = UriComponentsBuilder
                .fromUri("https://auth.example.com/authorize")
                .queryParam("response_type", "code")
                .queryParam("client_id", "client123")
                .queryParam("redirect_uri", "https://client.example.com/callback")
                .queryParam("scope", "read write")
                .queryParam("state", generateState());
        
        return builder.toUriString();
    }
    
    /**
     * 生成随机 state，防止 CSRF 攻击
     */
    private String generateState() {
        SecureRandom random = new SecureRandom();
        byte[] state = new byte[32];
        random.nextBytes(state);
        return Base64.getUrlEncoder().withoutPadding().encodeToString(state);
    }
    
    /**
     * 步骤2: 用户在授权服务器登录并授权
     * 
     * 授权服务器：
     * 1. 验证用户身份（登录）
     * 2. 显示授权页面，列出请求的权限
     * 3. 用户确认授权
     * 
     * 步骤3: 授权服务器重定向回客户端，附带授权码
     * 
     * 重定向 URL:
     * https://client.example.com/callback?code=AUTH_CODE&state=STATE
     */
    
    /**
     * 步骤4: 客户端使用授权码换取访问令牌
     * 
     * POST /oauth/token
     * Content-Type: application/x-www-form-urlencoded
     * 
     * grant_type=authorization_code
     * &code=AUTH_CODE
     * &redirect_uri=https://client.example.com/callback
     * &client_id=client123
     * &client_secret=CLIENT_SECRET
     */
    public TokenResponse exchangeCodeForToken(String code, String state) {
        // 1. 验证 state（防止 CSRF）
        if (!validateState(state)) {
            throw new SecurityException("Invalid state");
        }
        
        // 2. 构建令牌请求
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("grant_type", "authorization_code");
        params.add("code", code);
        params.add("redirect_uri", "https://client.example.com/callback");
        params.add("client_id", "client123");
        params.add("client_secret", "clientSecret123");
        
        // 3. 发送请求
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        
        HttpEntity<MultiValueMap<String, String>> request = 
                new HttpEntity<>(params, headers);
        
        // 4. 接收响应
        TokenResponse response = restTemplate.postForObject(
                "https://auth.example.com/oauth/token",
                request,
                TokenResponse.class
        );
        
        return response;
    }
    
    /**
     * Token 响应
     */
    @Data
    public static class TokenResponse {
        /**
         * Access Token
         */
        private String access_token;
        
        /**
         * Token 类型：Bearer
         */
        private String token_type;
        
        /**
         * 过期时间（秒）
         */
        private long expires_in;
        
        /**
         * Refresh Token
         */
        private String refresh_token;
        
        /**
         * 授权范围
         */
        private String scope;
        
        /**
         * ID Token（如果使用 OpenID Connect）
         */
        private String id_token;
    }
}
```

#### 19.2.2 Implicit Flow（隐式模式）

```java
/**
 * Implicit Flow (隐式模式)
 * 
 * ⚠️ 已被 OAuth 2.1 废弃，不推荐使用！
 * 
 * 适用场景（历史）：
 * - 单页应用（SPA）
 * - 没有后端服务器
 * 
 * 为什么被废弃？
 * - Access Token 直接暴露在 URL fragment 中
 * - 容易受到各种攻击
 * - 无法安全地使用 Refresh Token
 * 
 * 推荐替代方案：
 * - Authorization Code Flow + PKCE
 * 
 * 流程（仅供参考，不推荐使用）:
 */
public class ImplicitFlow {
    
    /**
     * 步骤1: 客户端将用户重定向到授权服务器
     * 
     * 与授权码模式类似，但 response_type=token
     */
    public String buildAuthorizationRequest() {
        return UriComponentsBuilder
                .fromUri("https://auth.example.com/authorize")
                .queryParam("response_type", "token")  // ⚠️ 直接请求 token
                .queryParam("client_id", "client123")
                .queryParam("redirect_uri", "https://client.example.com/callback")
                .queryParam("scope", "read write")
                .queryParam("state", generateState())
                .toUriString();
    }
    
    /**
     * 步骤2-3: 用户登录并授权
     * 
     * 与授权码模式相同
     */
    
    /**
     * 步骤4: 授权服务器直接返回 Access Token
     * 
     * 重定向 URL（包含 Access Token）:
     * https://client.example.com/callback#access_token=ACCESS_TOKEN&token_type=Bearer&expires_in=3600&scope=read+write&state=STATE
     * 
     * ⚠️ 安全问题：
     * - Access Token 在 URL 中
     * - 会被记录到浏览器历史
     * - 会被发送到 Referrer
     * - 会被记录到服务器日志
     */
}
```

#### 19.2.3 Resource Owner Password Credentials Flow（密码模式）

```java
/**
 * Resource Owner Password Credentials Flow (密码模式)
 * 
 * 适用场景：
 * - 第一方应用（官方应用）
     * - 高度信任的应用
     * - 用户直接向客户端提供凭据
 * 
 * ⚠️ 安全注意事项：
 * - 只用于官方应用，不用于第三方应用
 * - 用户需要信任客户端处理其凭据
 * - 客户端不得存储凭据
 * 
 * 流程：
 */
public class PasswordCredentialsFlow {
    
    /**
     * 客户端直接使用用户凭据获取令牌
     * 
     * POST /oauth/token
     * Content-Type: application/x-www-form-urlencoded
     * Authorization: Basic BASE64(client_id:client_secret)
     * 
     * grant_type=password
     * &username=USER_USERNAME
     * &password=USER_PASSWORD
     * &scope=read write
     */
    public TokenResponse getTokenWithPassword(String username, 
                                              String password) {
        // 1. 构建请求
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("grant_type", "password");
        params.add("username", username);
        params.add("password", password);
        params.add("scope", "read write");
        
        // 2. 使用 HTTP Basic 认证客户端
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        headers.setBasicAuth("client123", "clientSecret123");
        
        HttpEntity<MultiValueMap<String, String>> request = 
                new HttpEntity<>(params, headers);
        
        // 3. 发送请求
        return restTemplate.postForObject(
                "https://auth.example.com/oauth/token",
                request,
                TokenResponse.class
        );
    }
    
    /**
     * 使用场景示例：移动应用
     */
    @Service
    public class MobileAppAuthService {
        
        /**
         * 移动应用登录
         * 
         * 这是第一方官方应用，可以使用密码模式
         */
        public LoginResponse login(String username, String password) {
            // 1. 调用授权服务器获取 Token
            TokenResponse tokenResponse = getTokenWithPassword(username, password);
            
            // 2. 返回登录响应
            LoginResponse response = new LoginResponse();
            response.setAccessToken(tokenResponse.getAccess_token());
            response.setRefreshToken(tokenResponse.getRefresh_token());
            response.setExpiresIn(tokenResponse.getExpires_in());
            
            // 3. 可选：获取用户信息
            UserInfo userInfo = getUserInfo(tokenResponse.getAccess_token());
            response.setUser(userInfo);
            
            return response;
        }
    }
}
```

#### 19.2.4 Client Credentials Flow（客户端凭据模式）

```java
/**
 * Client Credentials Flow (客户端凭据模式)
 * 
 * 适用场景：
 * - 机器对机器（M2M）通信
 * - 服务账号（Service Account）
 * - 没有用户参与的自动化任务
 * 
 * 特点：
 * - 不涉及用户
 * - 客户端使用自己的凭据（client_id + client_secret）
 * - 获取的 Token 代表客户端本身，不是用户
 * 
 * 使用场景：
 * - 定时任务
 * - 后台服务调用
 * - API 集成
 */
public class ClientCredentialsFlow {
    
    /**
     * 客户端使用自己的凭据获取令牌
     * 
     * POST /oauth/token
     * Content-Type: application/x-www-form-urlencoded
     * Authorization: Basic BASE64(client_id:client_secret)
     * 
     * grant_type=client_credentials
     * &scope=read write
     */
    public TokenResponse getTokenForService() {
        // 1. 构建请求
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("grant_type", "client_credentials");
        params.add("scope", "read write");
        
        // 2. 使用客户端凭据认证
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        // 使用 HTTP Basic 认证
        headers.setBasicAuth("service-client", "service-secret");
        
        HttpEntity<MultiValueMap<String, String>> request = 
                new HttpEntity<>(params, headers);
        
        // 3. 发送请求
        return restTemplate.postForObject(
                "https://auth.example.com/oauth/token",
                request,
                TokenResponse.class
        );
    }
    
    /**
     * 使用场景：定时任务
     */
    @Component
    public class ScheduledDataSyncService {
        
        /**
         * 定时同步数据
         * 
         * 使用服务账号 Token 调用 API
         */
        @Scheduled(cron = "0 0 2 * * ?")  // 每天凌晨2点
        public void syncData() {
            // 1. 获取服务账号 Token
            TokenResponse tokenResponse = getTokenForService();
            
            // 2. 使用 Token 调用 API
            HttpHeaders headers = new HttpHeaders();
            headers.setBearerAuth(tokenResponse.getAccess_token());
            
            HttpEntity<Void> request = new HttpEntity<>(headers);
            
            // 调用数据同步 API
            ResponseEntity<String> response = restTemplate.exchange(
                    "https://api.example.com/api/sync",
                    HttpMethod.POST,
                    request,
                    String.class
            );
            
            log.info("数据同步完成: {}", response.getBody());
        }
    }
    
    /**
     * 使用场景：服务间通信
     */
    @Service
    public class OrderServiceClient {
        
        /**
         * 订单服务调用库存服务
         * 
         * 使用服务账号 Token
         */
        public Inventory checkInventory(Long productId) {
            // 1. 获取 Token（可以缓存）
            TokenResponse tokenResponse = getCachedTokenOrFetchNew();
            
            // 2. 调用库存服务
            HttpHeaders headers = new HttpHeaders();
            headers.setBearerAuth(tokenResponse.getAccess_token());
            
            HttpEntity<Void> request = new HttpEntity<>(headers);
            
            ResponseEntity<Inventory> response = restTemplate.exchange(
                    "https://inventory-service/api/inventory/" + productId,
                    HttpMethod.GET,
                    request,
                    Inventory.class
            );
            
            return response.getBody();
        }
    }
}
```

#### 19.2.5 Device Code Flow（设备代码流程）

```java
/**
 * Device Code Flow (设备代码流程)
 * 
 * 适用场景：
 * - 智能电视（Smart TV）
 * - 游戏机（游戏机）
     * - 物联网设备（IoT）
     * - 命令行应用（CLI）
 * 
 * 特点：
 * - 设备没有浏览器或输入受限
 * - 用户在另一个设备（手机/电脑）上完成授权
 * 
 * 流程：
 */
public class DeviceCodeFlow {
    
    /**
     * 步骤1: 设备请求设备代码
     * 
     * POST /oauth/device_code
     * Content-Type: application/x-www-form-urlencoded
     * 
     * client_id=CLIENT_ID
     * &scope=read write
     */
    public DeviceCodeResponse getDeviceCode() {
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("client_id", "tv-app-client");
        params.add("scope", "read write");
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        
        HttpEntity<MultiValueMap<String, String>> request = 
                new HttpEntity<>(params, headers);
        
        return restTemplate.postForObject(
                "https://auth.example.com/oauth/device_code",
                request,
                DeviceCodeResponse.class
        );
    }
    
    /**
     * 设备代码响应
     */
    @Data
    public static class DeviceCodeResponse {
        /**
         * 设备代码（在验证URI上输入）
         */
        private String device_code;
        
        /**
         * 用户代码（显示给用户）
         */
        private String user_code;
        
        /**
         * 验证 URI
         */
        private String verification_uri;
        
        /**
         * 验证 URI（完整，包含 user_code）
         */
        private String verification_uri_complete;
        
        /**
         * 轮询间隔（秒）
         */
        private long interval;
        
        /**
         * 设备代码过期时间（秒）
         */
        private long expires_in;
    }
    
    /**
     * 步骤2: 设备显示用户代码和验证URI
     * 
     * 屏幕显示：
     * ┌─────────────────────────────┐
     * │  请在手机或电脑上访问：      │
     * │  https://example.com/activate │
     * │                              │
     * │  输入代码：                    │
     * │  WDJB-MJHT                    │
     * └─────────────────────────────┘
     */
    
    /**
     * 步骤3: 设备开始轮询令牌端点
     * 
     * POST /oauth/token
     * Content-Type: application/x-www-form-urlencoded
     * 
     * grant_type=urn:ietf:params:oauth:grant-type:device_code
     * &device_code=DEVICE_CODE
     * &client_id=CLIENT_ID
     */
    public TokenResponse pollForToken(String deviceCode) {
        DeviceCodeResponse deviceCodeResponse = getDeviceCode();
        
        // 显示用户代码
        displayUserCode(deviceCodeResponse);
        
        // 开始轮询
        while (true) {
            try {
                // 等待轮询间隔
                Thread.sleep(deviceCodeResponse.getInterval() * 1000);
                
                // 轮询令牌端点
                MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
                params.add("grant_type", "urn:ietf:params:oauth:grant-type:device_code");
                params.add("device_code", deviceCode);
                params.add("client_id", "tv-app-client");
                
                HttpHeaders headers = new HttpHeaders();
                headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
                
                HttpEntity<MultiValueMap<String, String>> request = 
                        new HttpEntity<>(params, headers);
                
                try {
                    TokenResponse response = restTemplate.postForObject(
                            "https://auth.example.com/oauth/token",
                            request,
                            TokenResponse.class
                    );
                    
                    // 成功获取 Token
                    return response;
                    
                } catch (HttpStatusCodeException e) {
                    // 处理错误响应
                    if (e.getRawStatusCode() == 400) {
                        String error = parseError(e.getResponseBodyAsString());
                        
                        if ("authorization_pending".equals(error)) {
                            // 用户还未授权，继续轮询
                            continue;
                        } else if ("slow_down".equals(error)) {
                            // 轮询太快，增加间隔
                            Thread.sleep(5000);
                            continue;
                        } else if ("access_denied".equals(error)) {
                            // 用户拒绝授权
                            throw new RuntimeException("用户拒绝授权");
                        } else if ("expired_token".equals(error)) {
                            // 设备代码过期
                            throw new RuntimeException("设备代码过期，请重试");
                        }
                    }
                    throw e;
                }
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                throw new RuntimeException("轮询被中断", e);
            }
        }
    }
    
    /**
     * 步骤4: 用户在另一个设备上授权
     * 
     * 用户在手机/电脑上：
     * 1. 访问 verification_uri
     * 2. 输入 user_code
     * 3. 登录并授权
     */
    
    private void displayUserCode(DeviceCodeResponse response) {
        // 在设备屏幕上显示
        System.out.println("=================================");
        System.out.println("请在手机或电脑上访问:");
        System.out.println(response.getVerification_uri());
        System.out.println();
        System.out.println("输入代码:");
        System.out.println(response.getUser_code());
        System.out.println("=================================");
    }
    
    private String parseError(String responseBody) {
        // 解析错误响应
        // {"error":"authorization_pending"}
        // ...
        return "authorization_pending";
    }
}
```

### 19.3 OAuth2 安全最佳实践

#### 19.3.1 PKCE (Proof Key for Code Exchange)

```java
/**
 * PKCE (RFC 7636)
 * 
 * 为什么需要 PKCE？
 * - 授权码拦截攻击
 * - 防止授权码被恶意应用窃取
 * 
 * PKCE 流程：
 * 1. 客户端生成 code_verifier（随机字符串）
 * 2. 客户端生成 code_challenge = SHA256(code_verifier)
 * 3. 授权请求携带 code_challenge
 * 4. 令牌请求携带 code_verifier
 * 5. 授权服务器验证 code_challenge == SHA256(code_verifier)
 */
public class PKCEHelper {
    
    private static final int CODE_VERIFIER_LENGTH = 128;
    private static final SecureRandom SECURE_RANDOM = new SecureRandom();
    
    /**
     * 生成 Code Verifier
     * 
     * 要求：
     * - 长度：43-128 字符
     * - 字符集：[A-Z] / [a-z] / [0-9] / "-" / "." / "_" / "~"
     */
    public static String generateCodeVerifier() {
        byte[] bytes = new byte[CODE_VERIFIER_LENGTH];
        SECURE_RANDOM.nextBytes(bytes);
        return Base64.getUrlEncoder()
                .withoutPadding()
                .encodeToString(bytes);
    }
    
    /**
     * 生成 Code Challenge
     * 
     * 方法：
     * - plain: code_challenge = code_verifier
     * - S256: code_challenge = BASE64URL(SHA256(code_verifier))
     * 
     * 推荐：使用 S256
     */
    public static String generateCodeChallenge(String codeVerifier) {
        try {
            MessageDigest digest = MessageDigest.getInstance("SHA-256");
            byte[] hash = digest.digest(codeVerifier.getBytes(StandardCharsets.UTF_8));
            return Base64.getUrlEncoder()
                    .withoutPadding()
                    .encodeToString(hash);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("SHA-256 算法不可用", e);
        }
    }
    
    /**
     * 使用 PKCE 的授权请求
     */
    public String buildAuthorizationRequestWithPKCE() {
        // 1. 生成 code_verifier 和 code_challenge
        String codeVerifier = generateCodeVerifier();
        String codeChallenge = generateCodeChallenge(codeVerifier);
        
        // 保存 code_verifier（在令牌请求时使用）
        // 可以存储在 Session 或 LocalStorage
        saveCodeVerifier(codeVerifier);
        
        // 2. 构建授权请求
        return UriComponentsBuilder
                .fromUri("https://auth.example.com/authorize")
                .queryParam("response_type", "code")
                .queryParam("client_id", "client123")
                .queryParam("redirect_uri", "https://client.example.com/callback")
                .queryParam("scope", "read write")
                .queryParam("state", generateState())
                .queryParam("code_challenge", codeChallenge)      // PKCE 参数
                .queryParam("code_challenge_method", "S256")      // PKCE 方法
                .toUriString();
    }
    
    /**
     * 使用 PKCE 的令牌请求
     */
    public TokenResponse exchangeCodeForTokenWithPKCE(String code, String state) {
        // 1. 检索保存的 code_verifier
        String codeVerifier = getCodeVerifier();
        
        // 2. 构建令牌请求
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("grant_type", "authorization_code");
        params.add("code", code);
        params.add("redirect_uri", "https://client.example.com/callback");
        params.add("client_id", "client123");
        // ⚠️ PKCE 模式下不需要 client_secret
        params.add("code_verifier", codeVerifier);  // PKCE 参数
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        
        HttpEntity<MultiValueMap<String, String>> request = 
                new HttpEntity<>(params, headers);
        
        return restTemplate.postForObject(
                "https://auth.example.com/oauth/token",
                request,
                TokenResponse.class
        );
    }
}
```

#### 19.3.2 State 参数防CSRF

```java
/**
 * State 参数 - 防止 CSRF 攻击
 * 
 * CSRF 攻击场景：
 * 1. 攻击者诱导用户访问授权页面
 * 2. 授权码返回到攻击者的 redirect_uri
 * 3. 攻击者使用授权码获取 Token
 * 
 * 防御：使用 state 参数
 * 1. 客户端生成随机 state
 * 2. 授权请求携带 state
 * 3. 授权服务器回传相同的 state
 * 4. 客户端验证 state
 */
@Component
public class StateManager {
    
    /**
     * 生成 state
     * 
     * 要求：
     * - 足够长（至少 128 位）
     * - 加密随机
     * - 不可预测
     */
    public String generateState() {
        SecureRandom random = new SecureRandom();
        byte[] state = new byte[32];
        random.nextBytes(state);
        
        String stateString = Base64.getUrlEncoder()
                .withoutPadding()
                .encodeToString(state);
        
        // 存储 state（用于后续验证）
        // 可以存储在 Session 或 Redis
        saveState(stateString);
        
        return stateString;
    }
    
    /**
     * 验证 state
     * 
     * @param state 回传的 state
     * @return true 如果 state 有效
     */
    public boolean validateState(String state) {
        // 1. 检查 state 是否存在
        if (!hasState(state)) {
            return false;
        }
        
        // 2. 验证后删除 state（一次性使用）
        removeState(state);
        
        return true;
    }
    
    /**
     * 完整的授权流程（带 state 验证）
     */
    @GetMapping("/oauth/callback")
    public ResponseEntity<?> callback(
            @RequestParam String code,
            @RequestParam String state) {
        
        // 1. 验证 state（防止 CSRF）
        if (!stateManager.validateState(state)) {
            return ResponseEntity.badRequest()
                    .body("Invalid state: possible CSRF attack");
        }
        
        // 2. 使用授权码换取 Token
        TokenResponse tokenResponse = exchangeCodeForToken(code);
        
        // 3. 返回成功响应
        return ResponseEntity.ok(tokenResponse);
    }
}
```

### 19.4 常见问题解答 (FAQ)

**Q1: OAuth1.0 和 OAuth2.0 有什么区别？**

A: **对比**：

| 特性 | OAuth 1.0 | OAuth 2.0 |
|------|-----------|-----------|
| 复杂度 | 复杂 | 简化 |
| 签名 | 每个请求都要签名 | 使用 TLS/HTTPS |
| 用户体验 | 差 | 好 |
| 实现难度 | 困难 | 容易 |
| 安全性 | 高 | 依赖 TLS |

**OAuth 1.0 流程**（仅供参考）：
```
1. 获取 Request Token
2. 用户授权
3. 获取 Access Token
4. 访问资源（每次请求都需要签名）
```

**OAuth 2.0 流程**：
```
1. 获取 Authorization Code
2. 获取 Access Token
3. 访问资源（携带 Token）
```

**Q2: 什么时候使用哪个 OAuth2 流程？**

A: **流程选择指南**：

```
┌─────────────────────────┬─────────────────────────┐
│ 应用类型                │ 推荐流程                │
├─────────────────────────┼─────────────────────────┤
│ Web 应用（有后端）      │ Authorization Code      │
│ 单页应用（SPA）          │ Auth Code + PKCE        │
│ 移动应用                │ Auth Code + PKCE        │
│ 智能电视/IoT            │ Device Code             │
│ 机器对机器              │ Client Credentials      │
│ 第一方应用              │ Password Credentials    │
│                         │（不推荐用于第三方）      │
└─────────────────────────┴─────────────────────────┘
```

**Q3: OpenID Connect 和 OAuth2 有什么关系？**

A: **关系**：

- **OAuth2**: 授权框架（Access 资源）
- **OpenID Connect**: 身份认证层（在 OAuth2 之上）

**OpenID Connect 扩展**：
```
OAuth2 Response:
{
  "access_token": "...",
  "token_type": "Bearer",
  "expires_in": 3600
}

OpenID Connect Response:
{
  "access_token": "...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "id_token": "eyJ...",  // 新增：ID Token（JWT）
  "scope": "openid profile email"
}
```

**ID Token 包含**：
- 用户身份信息
- 认证时间
- 发布者
- 受众

**Q4: 如何安全地存储 Token？**

A: **存储方案**：

```
┌─────────────────────────┬─────────────────────────┐
│ 应用类型                │ Token 存储位置          │
├─────────────────────────┼─────────────────────────┤
│ Web 应用（服务端渲染）  │ HttpOnly Cookie         │
│ Web 应用（客户端渲染）  │ Session Storage（短期）  │
│ 移动应用（iOS/Android）  │ Keychain / Keystore     │
│ 单页应用                │ Session Storage（短期）  │
│ 服务端（机器对机器）     │ 环境变量 / 密钥管理     │
└─────────────────────────┴─────────────────────────┘
```

**安全原则**：
- Access Token → 内存或 Session
- Refresh Token → 安全存储（加密）
- 不要存储在 LocalStorage（XSS 风险）
- 不要存储在 URL（日志泄露）

**Q5: 如何处理 Token 过期？**

A: **Token 刷新策略**：

```java
/**
 * Token 刷新流程
 * 
 * 策略1: 自动刷新
 * - 在过期前自动刷新
 * - 用户无感知
 * 
 * 策略2: 按需刷新
 * - 收到 401 时刷新
 * - 可能导致请求失败
 */

// 自动刷新示例
@Service
public class AutoTokenRefreshService {
    
    /**
     * 刷新阈值（过期前 5 分钟）
     */
    private static final long REFRESH_THRESHOLD = 5 * 60 * 1000;
    
    /**
     * 获取有效的 Access Token
     * 如果即将过期，自动刷新
     */
    public String getValidAccessToken() {
        TokenInfo tokenInfo = getTokenInfo();
        
        // 检查是否需要刷新
        if (shouldRefresh(tokenInfo)) {
            // 刷新 Token
            tokenInfo = refreshAccessToken();
        }
        
        return tokenInfo.getAccessToken();
    }
    
    /**
     * 检查是否需要刷新
     */
    private boolean shouldRefresh(TokenInfo tokenInfo) {
        long expiresAt = tokenInfo.getExpiresAt();
        long now = System.currentTimeMillis();
        
        return (expiresAt - now) < REFRESH_THRESHOLD;
    }
    
    /**
     * 刷新 Access Token
     * 
     * POST /oauth/token
     * 
     * grant_type=refresh_token
     * &refresh_token=REFRESH_TOKEN
     * &client_id=CLIENT_ID
     * &client_secret=CLIENT_SECRET
     */
    private TokenInfo refreshAccessToken() {
        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.add("grant_type", "refresh_token");
        params.add("refresh_token", getRefreshToken());
        params.add("client_id", getClientId());
        params.add("client_secret", getClientSecret());
        
        TokenResponse response = restTemplate.postForObject(
                "https://auth.example.com/oauth/token",
                params,
                TokenResponse.class
        );
        
        // 更新存储的 Token
        saveTokenInfo(response);
        
        return TokenInfo.from(response);
    }
}
```

### 19.5 思考题

1. **设计题**: 设计一个基于 OAuth2 的单点登录（SSO）系统，支持多个子公司共享用户账户。

2. **安全题**: 分析授权码拦截攻击的原理，并说明 PKCE 如何防御这种攻击。

3. **架构题**: 在微服务架构中，如何设计统一的 OAuth2 认证架构？每个服务都验证 Token 还是使用 API Gateway 验证？

4. **实现题**: 实现一个 OAuth2 授权服务器，支持授权码模式和客户端凭据模式。

5. **场景题**: 一家公司有多个应用（Web、移动端、CLI 工具），应该如何设计 OAuth2 的客户端和流程？

---

## 知识点20: Spring Security OAuth2

### 20.1 Spring Security OAuth2 架构

#### 20.1.1 核心组件

```java
/**
 * Spring Security OAuth2 核心组件
 * 
 * 注意：Spring Security OAuth2 项目已不再推荐使用
 * 推荐使用：Spring Authorization Server
 * 
 * 但本知识点仍讲解 Spring Security OAuth2 的原理
 * 因为许多现有系统仍在使用
 */

/**
 * 1. AuthorizationServerConfigurer
 * 
 * 配置授权服务器的核心组件
 */
@Configuration
@EnableAuthorizationServer
public class AuthorizationServerConfig extends 
        AuthorizationServerConfigurerAdapter {
    
    /**
     * ClientDetailsService
     * 
     * 管理客户端详情（client_id, client_secret, scopes, grants）
     */
    @Autowired
    private ClientDetailsService clientDetailsService;
    
    /**
     * UserDetailsService
     * 
     * 加载用户详情（用于密码模式）
     */
    @Autowired
    private UserDetailsService userDetailsService;
    
    /**
     * AuthenticationManager
     * 
     * 用于密码模式的用户认证
     */
    @Autowired
    private AuthenticationManager authenticationManager;
    
    /**
     * TokenStore
     * 
     * 存储 Token（内存、数据库、Redis、JWT）
     */
    @Bean
    public TokenStore tokenStore() {
        // 选项1: 内存存储（开发环境）
        // return new InMemoryTokenStore();
        
        // 选项2: 数据库存储
        // return new JdbcTokenStore(dataSource);
        
        // 选项3: Redis 存储
        // return new RedisTokenStore(redisConnectionFactory);
        
        // 选项4: JWT 存储（无状态，推荐）
        return new JwtTokenStore(jwtAccessTokenConverter());
    }
    
    /**
     * JwtAccessTokenConverter
     * 
     * 用于 JWT Token 的转换和签名
     */
    @Bean
    public JwtAccessTokenConverter jwtAccessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        
        // 设置签名密钥
        converter.setSigningKey("jwt-signing-key");
        
        // 或者使用密钥对（非对称加密）
        // KeyPair keyPair = new KeyStoreKeyFactory(
        //         new ClassPathResource("keystore.jks"),
        //         "keystore-password".toCharArray())
        //         .getKeyPair("alias", "key-password".toCharArray());
        // converter.setKeyPair(keyPair);
        
        return converter;
    }
    
    /**
     * TokenEnhancer
     * 
     * 增强 Token（添加自定义声明）
     */
    @Bean
    public TokenEnhancer tokenEnhancer() {
        return new CustomTokenEnhancer();
    }
    
    /**
     * 自定义 Token 增强器
     */
    public static class CustomTokenEnhancer implements TokenEnhancer {
        
        @Override
        public OAuth2AccessToken enhance(
                OAuth2AccessToken accessToken,
                OAuth2Authentication authentication) {
            
            // 获取原始 Token
            DefaultOAuth2AccessToken token = 
                    (DefaultOAuth2AccessToken) accessToken;
            
            // 获取额外信息
            Map<String, Object> additionalInfo = 
                    new HashMap<>(token.getAdditionalInformation());
            
            // 添加自定义声明
            additionalInfo.put("organization", "example.com");
            additionalInfo.put("timestamp", System.currentTimeMillis());
            
            // 添加用户信息
            Authentication userAuthentication = 
                    authentication.getUserAuthentication();
            if (userAuthentication instanceof UsernamePasswordAuthenticationToken) {
                UserDetails userDetails = 
                        (UserDetails) userAuthentication.getPrincipal();
                additionalInfo.put("username", userDetails.getUsername());
                additionalInfo.put("authorities", 
                        userDetails.getAuthorities()
                                .stream()
                                .map(GrantedAuthority::getAuthority)
                                .collect(Collectors.toList())
                );
            }
            
            // 设置额外信息
            token.setAdditionalInformation(additionalInfo);
            
            return token;
        }
    }
    
    /**
     * 配置客户端详情服务
     */
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) 
            throws Exception {
        
        clients
                // 选项1: 内存存储
                .inMemory()
                        .withClient("client1")  // client_id
                        .secret("{noop}secret1")  // client_secret ({noop} = 明文)
                        .authorizedGrantTypes(
                                "authorization_code",  // 授权码模式
                                "refresh_token",       // 刷新令牌
                                "password",            // 密码模式
                                "client_credentials"   // 客户端凭据模式
                        )
                        .scopes("read", "write")  // 授权范围
                        .redirectUris("http://localhost:8081/callback")  // 重定向 URI
                        .accessTokenValiditySeconds(3600)      // Access Token 有效期：1小时
                        .refreshTokenValiditySeconds(2592000)  // Refresh Token 有效期：30天
                        .and()
                        .withClient("client2")
                        .secret("{noop}secret2")
                        .authorizedGrantTypes("authorization_code", "refresh_token")
                        .scopes("read")
                        .redirectUris("http://localhost:8082/callback")
                
                // 选项2: 数据库存储
                // .jdbc(dataSource)
                
                // 选项3: 自定义 ClientDetailsService
                // .withClientDetails(customClientDetailsService);
    }
    
    /**
     * 配置授权端点的安全约束
     */
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security) 
            throws Exception {
        security
                // 允许客户端认证（表单参数）
                .allowFormAuthenticationForClients()
                
                // 允许 Token 检查（需要认证）
                .checkTokenAccess("isAuthenticated()")
                
                // 允许获取 Token 密钥（需要认证）
                .tokenKeyAccess("isAuthenticated()");
    }
    
    /**
     * 配置授权端点
     */
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) 
            throws Exception {
        
        endpoints
                // 设置认证管理器（用于密码模式）
                .authenticationManager(authenticationManager)
                
                // 设置用户详情服务（用于密码模式）
                .userDetailsService(userDetailsService)
                
                // 设置 Token 存储
                .tokenStore(tokenStore())
                
                // 设置 JWT 转换器
                .accessTokenConverter(jwtAccessTokenConverter())
                
                // 配置 Token 增强器链
                .tokenEnhancer(
                        TokenEnhancerChain.of(
                                Arrays.asList(
                                        tokenEnhancer(),
                                        jwtAccessTokenConverter()
                                )
                        )
                )
                
                // 配置授权码服务（存储授权码）
                // 使用数据库存储
                .authorizationCodeServices(
                        new JdbcAuthorizationCodeServices(dataSource)
                )
                
                // 配置异常转换器（自定义错误响应）
                .exceptionTranslator(new CustomOAuth2ExceptionTranslator());
    }
}

/**
 * 自定义 OAuth2 异常转换器
 */
public class CustomOAuth2ExceptionTranslator 
        implements WebResponseExceptionTranslator<OAuth2Exception> {
    
    @Override
    public ResponseEntity<OAuth2Exception> translate(Exception e) 
            throws Exception {
        
        // 转换异常
        OAuth2Exception oauth2Exception = (OAuth2Exception) e;
        
        // 自定义响应
        Map<String, String> errorResponse = new HashMap<>();
        errorResponse.put("error", oauth2Exception.getOAuth2ErrorCode());
        errorResponse.put("error_description", oauth2Exception.getMessage());
        errorResponse.put("timestamp", 
                String.valueOf(System.currentTimeMillis()));
        
        return ResponseEntity
                .status(oauth2Exception.getHttpErrorCode())
                .body(oauth2Exception);
    }
}
```

#### 20.1.2 资源服务器配置

```java
/**
 * Resource Server Configuration
 * 
 * 资源服务器托管受保护的资源，验证 Access Token
 */
@Configuration
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {
    
    /**
     * 资源服务器 ID
     */
    private static final String RESOURCE_ID = "resource-server-1";
    
    /**
     * TokenStore（必须与授权服务器一致）
     */
    @Autowired
    private TokenStore tokenStore;
    
    /**
     * TokenStore（必须与授权服务器一致）
     */
    @Autowired
    private JwtAccessTokenConverter jwtAccessTokenConverter;
    
    /**
     * 配置资源
     */
    @Override
    public void configure(ResourceServerSecurityConfigurer resources) 
            throws Exception {
        resources
                // 设置资源 ID
                .resourceId(RESOURCE_ID)
                
                // 配置 TokenStore
                .tokenStore(tokenStore)
                
                // 配置 Token 提取器
                .tokenExtractor(new CustomTokenExtractor());
    }
    
    /**
     * 配置 HTTP 安全规则
     */
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
                // 配置授权规则
                .authorizeRequests()
                        // 公开端点（不需要 Token）
                        .antMatchers("/api/public/**")
                                .permitAll()
                        
                        // 需要 read 权限
                        .antMatchers(HttpMethod.GET, "/api/**")
                                .access("#oauth2.hasScope('read')")
                        
                        // 需要 write 权限
                        .antMatchers(HttpMethod.POST, "/api/**")
                                .access("#oauth2.hasScope('write')")
                        
                        // 需要 ADMIN 角色
                        .antMatchers("/api/admin/**")
                                .access("hasRole('ADMIN') and #oauth2.hasScope('write')")
                        
                        // 其他请求需要认证
                        .anyRequest()
                                .authenticated();
    }
}

/**
 * 自定义 Token 提取器
 * 
 * 从请求中提取 Access Token
 * 支持：Authorization Header、Query Parameter、Cookie
 */
public class CustomTokenExtractor implements TokenExtractor {
    
    private static final String HEADER = "Authorization";
    private static final String BEARER = "Bearer";
    
    @Override
    public Authentication extract(HttpServletRequest request) {
        // 1. 尝试从 Authorization Header 提取
        String token = extractFromHeader(request);
        
        // 2. 尝试从 Query Parameter 提取
        if (token == null) {
            token = extractFromParameter(request);
        }
        
        // 3. 尝试从 Cookie 提取
        if (token == null) {
            token = extractFromCookie(request);
        }
        
        if (token == null) {
            return null;
        }
        
        // 构建 PreAuthenticatedAuthenticationToken
        return new PreAuthenticatedAuthenticationToken(
                token,
                "",
                Collections.emptyList()
        );
    }
    
    private String extractFromHeader(HttpServletRequest request) {
        String header = request.getHeader(HEADER);
        
        if (header != null && header.startsWith(BEARER + " ")) {
            return header.substring(BEARER.length() + 1);
        }
        
        return null;
    }
    
    private String extractFromParameter(HttpServletRequest request) {
        return request.getParameter("access_token");
    }
    
    private String extractFromCookie(HttpServletRequest request) {
        Cookie[] cookies = request.getCookies();
        
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if ("access_token".equals(cookie.getName())) {
                    return cookie.getValue();
                }
            }
        }
        
        return null;
    }
}
```

### 20.2 完整的 OAuth2 实现

#### 20.2.1 授权服务器

```java
/**
 * 完整的 OAuth2 授权服务器实现
 */
@Configuration
@EnableAuthorizationServer
public class OAuth2AuthorizationServerConfig 
        extends AuthorizationServerConfigurerAdapter {
    
    @Autowired
    private AuthenticationManager authenticationManager;
    
    @Autowired
    private UserDetailsService userDetailsService;
    
    @Autowired
    private DataSource dataSource;
    
    @Bean
    public TokenStore tokenStore() {
        return new JwtTokenStore(jwtAccessTokenConverter());
    }
    
    @Bean
    public JwtAccessTokenConverter jwtAccessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        converter.setSigningKey("jwt-signing-key-12345678901234567890");
        return converter;
    }
    
    @Bean
    public AuthorizationCodeServices authorizationCodeServices() {
        return new JdbcAuthorizationCodeServices(dataSource);
    }
    
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) 
            throws Exception {
        clients.jdbc(dataSource);
    }
    
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) 
            throws Exception {
        endpoints
                .authenticationManager(authenticationManager)
                .userDetailsService(userDetailsService)
                .tokenStore(tokenStore())
                .accessTokenConverter(jwtAccessTokenConverter())
                .authorizationCodeServices(authorizationCodeServices());
    }
    
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security) 
            throws Exception {
        security
                .passwordEncoder(passwordEncoder())
                .allowFormAuthenticationForClients()
                .checkTokenAccess("permitAll()")
                .tokenKeyAccess("permitAll()");
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}

/**
 * 控制器：处理授权相关请求
 */
@Controller
@SessionAttributes("authorizationRequest")
public class ApprovalController {
    
    /**
     * 授权确认页面
     */
    @GetMapping("/oauth/confirm_access")
    public String getAccessConfirmation(
            Map<String, Object> model,
            Principal principal) {
        
        // 获取授权请求
        AuthorizationRequest authorizationRequest = 
                (AuthorizationRequest) model.get("authorizationRequest");
        
        // 获取客户端信息
        ClientDetails client = clientDetailsService
                .loadClientByClientId(authorizationRequest.getClientId());
        
        // 添加模型数据
        model.put("auth_request", authorizationRequest);
        model.put("client", client);
        model.put("user", principal);
        
        return "approval";
    }
}
```

#### 20.2.2 资源服务器

```java
/**
 * 完整的 OAuth2 资源服务器实现
 */
@Configuration
@EnableResourceServer
public class OAuth2ResourceServerConfig 
        extends ResourceServerConfigurerAdapter {
    
    @Bean
    public TokenStore tokenStore() {
        return new JwtTokenStore(jwtAccessTokenConverter());
    }
    
    @Bean
    public JwtAccessTokenConverter jwtAccessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        converter.setSigningKey("jwt-signing-key-12345678901234567890");
        return converter;
    }
    
    @Override
    public void configure(ResourceServerSecurityConfigurer resources) {
        resources
                .resourceId("resource-server-1")
                .tokenStore(tokenStore());
    }
    
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .authorizeRequests()
                        .antMatchers("/api/public/**").permitAll()
                        .antMatchers("/api/read/**").access("#oauth2.hasScope('read')")
                        .antMatchers("/api/write/**").access("#oauth2.hasScope('write')")
                        .antMatchers("/api/admin/**").access("hasRole('ADMIN')")
                        .anyRequest().authenticated();
    }
}

/**
 * 示例控制器
 */
@RestController
@RequestMapping("/api")
public class ResourceController {
    
    /**
     * 公开端点
     */
    @GetMapping("/public/hello")
    public Map<String, String> publicHello() {
        return Map.of("message", "Hello, public!");
    }
    
    /**
     * 需要 read scope
     */
    @GetMapping("/read/data")
    @PreAuthorize("#oauth2.hasScope('read')")
    public Map<String, String> readData(Principal principal) {
        return Map.of(
                "message", "Read data",
                "user", principal.getName()
        );
    }
    
    /**
     * 需要 write scope
     */
    @PostMapping("/write/data")
    @PreAuthorize("#oauth2.hasScope('write')")
    public Map<String, String> writeData(@RequestBody Map<String, String> data,
                                         Principal principal) {
        return Map.of(
                "message", "Data written",
                "user", principal.getName(),
                "data", data
        );
    }
    
    /**
     * 需要 ADMIN 角色
     */
    @GetMapping("/admin/users")
    @PreAuthorize("hasRole('ADMIN')")
    public Map<String, String> adminUsers(Principal principal) {
        return Map.of(
                "message", "Admin access",
                "user", principal.getName()
        );
    }
}
```

### 20.3 Spring Authorization Server（推荐）

```java
/**
 * Spring Authorization Server
 * 
 * 新的 OAuth2/OIDC 授权服务器实现
 * 替代已不再维护的 Spring Security OAuth2
 * 
 * 官方推荐：https://spring.io/projects/spring-authorization-server
 */

/**
 * Maven 依赖
 */
/*
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-oauth2-authorization-server</artifactId>
    <version>1.0.1</version>
</dependency>
*/

/**
 * 配置类
 */
@Configuration
@Import(OAuth2AuthorizationServerConfiguration.class)
public class AuthorizationServerConfig {
    
    /**
     * 注册客户端
     * 
     * 使用 JdbcRegisteredClientRepository 存储到数据库
     */
    @Bean
    public RegisteredClientRepository registeredClientRepository(
            JdbcTemplate jdbcTemplate) {
        
        // 创建仓库
        JdbcRegisteredClientRepository repository = 
                new JdbcRegisteredClientRepository(jdbcTemplate);
        
        // 注册示例客户端
        RegisteredClient client1 = RegisteredClient.withId(UUID.randomUUID().toString())
                .clientId("client1")
                .clientSecret("{noop}secret1")
                .clientAuthenticationMethod(ClientAuthenticationMethod.BASIC)
                .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
                .authorizationGrantType(AuthorizationGrantType.REFRESH_TOKEN)
                .authorizationGrantType(AuthorizationGrantType.CLIENT_CREDENTIALS)
                .redirectUri("http://localhost:8081/login/oauth2/code/client1")
                .scope(OAuth2ScopeCodes.OPENID)
                .scope(OAuth2ScopeCodes.PROFILE)
                .scope("read")
                .scope("write")
                .clientSettings(ClientSettings.builder()
                        .requireAuthorizationConsent(true)
                        .build())
                .build();
        
        repository.save(client1);
        
        return repository;
    }
    
    /**
     * 配置授权服务
     */
    @Bean
    public OAuth2AuthorizationService authorizationService(
            JdbcTemplate jdbcTemplate,
            RegisteredClientRepository registeredClientRepository) {
        
        return new JdbcOAuth2AuthorizationService(
                jdbcTemplate,
                registeredClientRepository
        );
    }
    
    /**
     * 配置授权同意服务
     */
    @Bean
    public OAuth2AuthorizationConsentService authorizationConsentService(
            JdbcTemplate jdbcTemplate,
            RegisteredClientRepository registeredClientRepository) {
        
        return new JdbcOAuth2AuthorizationConsentService(
                jdbcTemplate,
                registeredClientRepository
        );
    }
    
    /**
     * 配置 JWT 编码器
     */
    @Bean
    public JwtEncoder jwtEncoder(JWKSource<SecurityContext> jwkSource) {
        return new NimbusJwtEncoder(jwkSource);
    }
    
    /**
     * 配置 JWT 解码器
     */
    @Bean
    public JwtDecoder jwtDecoder(JWKSource<SecurityContext> jwkSource) {
        return OAuth2AuthorizationServerConfiguration.jwtDecoder(jwkSource);
    }
    
    /**
     * 配置 JWK 来源
     */
    @Bean
    public JWKSource<SecurityContext> jwkSource() throws Exception {
        KeyPair keyPair = generateRsaKeyPair();
        RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
        RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();
        
        RSAKey rsaKey = new RSAKey.Builder(publicKey)
                .privateKey(privateKey)
                .keyID(UUID.randomUUID().toString())
                .build();
        
        JWKSet jwkSet = new JWKSet(rsaKey);
        return new ImmutableJWKSet<>(jwkSet);
    }
    
    /**
     * 生成 RSA 密钥对
     */
    private static KeyPair generateRsaKeyPair() throws Exception {
        KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        keyPairGenerator.initialize(2048);
        return keyPairGenerator.generateKeyPair();
    }
}
```

### 20.4 常见问题解答 (FAQ)

**Q1: Spring Security OAuth2 和 Spring Authorization Server 有什么区别？**

A: **对比**：

| 特性 | Spring Security OAuth2 | Spring Authorization Server |
|------|----------------------|----------------------------|
| 状态 | 不再维护 | 官方推荐 |
| Spring Security 版本 | 旧版本 | 5.x/6.x |
| OAuth2 版本 | 2.0 | 2.1 |
| OIDC 支持 | 有限 | 完整支持 |
| 配置方式 | 复杂 | 简化 |
| 扩展性 | 一般 | 好 |

**Q2: 如何实现单点登录（SSO）？**

A: **SSO 实现方案**：

```java
/**
 * 单点登录实现
 * 
 * 核心概念：
 * 1. 共享授权服务器
 * 2. 共享 Token
 * 3. 跨应用会话
 */
@Configuration
public class SSOConfig {
    
    /**
     * 方案1: 共享授权服务器
     * 
     * 所有应用使用同一个授权服务器
     * 用户在一个应用登录后，其他应用无需再次登录
     */
    
    /**
     * 方案2: 使用 CAS（Central Authentication Service）
     * 
     * 类似 OAuth2，但更专注于 SSO
     */
    
    /**
     * 方案3: 使用 SAML（Security Assertion Markup Language）
     * 
     * 企业级 SSO 标准
     * 适用于企业内部应用
     */
}
```

**Q3: 如何实现多租户 OAuth2？**

A: **多租户方案**：

```java
/**
 * 多租户 OAuth2 实现
 * 
 * 策略：
 * 1. 每个租户独立的 client_id
 * 2. 共享授权服务器，租户隔离
 * 3. 租户特定的 Token
 */
@Service
public class MultiTenantClientDetailsService 
        implements ClientDetailsService {
    
    @Autowired
    private TenantRepository tenantRepository;
    
    @Override
    public ClientDetails loadClientByClientId(String clientId) {
        // 解析租户 ID
        String tenantId = extractTenantId(clientId);
        
        // 加载租户配置
        Tenant tenant = tenantRepository.findById(tenantId)
                .orElseThrow();
        
        // 构建客户端详情
        BaseClientDetails client = new BaseClientDetails();
        client.setClientId(clientId);
        client.setClientSecret(tenant.getClientSecret());
        client.setAuthorizedGrantTypes(tenant.getGrantTypes());
        client.setScope(tenant.getScopes());
        client.setRegisteredRedirectUri(tenant.getRedirectUris());
        
        return client;
    }
    
    private String extractTenantId(String clientId) {
        // client_id 格式: tenant1_client1
        return clientId.split("_")[0];
    }
}
```

**Q4: OAuth2 性能如何优化？**

A: **性能优化策略**：

```java
/**
 * OAuth2 性能优化
 * 
 * 1. 使用 JWT Token（无状态）
 * 2. Token 缓存
 * 3. 异步 Token 刷新
 * 4. 负载均衡
 */

/**
 * Token 缓存
 */
@Service
public class CachedTokenServices {
    
    @Autowired
    private TokenStore tokenStore;
    
    @Autowired
    private CacheManager cacheManager;
    
    /**
     * 缓存 Token 验证结果
     */
    public OAuth2Authentication loadAuthentication(String accessToken) {
        Cache cache = cacheManager.getCache("tokenValidation");
        
        return cache.get(accessToken, () -> {
            // 缓存未命中，查询 TokenStore
            return tokenStore.readAuthentication(accessToken);
        });
    }
}
```

### 20.5 思考题

1. **设计题**: 设计一个支持微信、支付宝、GitHub 登录的 OAuth2 集成系统。

2. **安全题**: 分析 OAuth2 的安全风险（如 Token 泄露、授权码拦截）并提出防御措施。

3. **实现题**: 实现一个支持多租户的 OAuth2 授权服务器。

4. **架构题**: 在微服务架构中，如何设计统一的认证和授权体系？

5. **场景题**: 一个公司有多个子公司，每个子公司有自己的用户系统，如何实现跨子公司的 SSO？

---

## 总结

本部分详细讲解了 Spring Security 中 JWT 和 OAuth2 的核心概念、实现细节和最佳实践。

### 关键要点：

**JWT 部分**：
1. JWT 的结构和原理
2. JWT 与 Spring Security 集成
3. JWT 安全最佳实践
4. Token 管理、刷新、撤销

**OAuth2 部分**：
1. OAuth2 的四种授权流程
2. Spring Security OAuth2 配置
3. Spring Authorization Server（推荐）
4. 单点登录（SSO）实现

### 实践建议：

1. **优先使用 JWT + Refresh Token** 模式
2. **使用 HttpOnly Cookie + SameSite** 存储 Token
3. **实现 Token 黑名单** 或版本号机制
4. **使用 PKCE** 防御授权码拦截
5. **迁移到 Spring Authorization Server**

### 安全要点：

1. **永远不要在 JWT 中存储敏感信息**
2. **使用强密钥（至少 256 位）**
3. **设置合理的过期时间**
4. **启用 HTTPS**
5. **实现多层安全检查**

---

**课程讲义生成完成**

本讲义涵盖了 Spring Security 5.3.9.RELEASE 中 JWT 和 OAuth2 的所有核心知识点，包括：
- 深度源码分析
- 完整可运行的代码示例
- 安全最佳实践
- 常见问题解答
- 思考题

适用于：
- 本科高级课程
- 企业培训
- 技术参考
- 面试准备

总字数：约 60,000 字
代码示例：50+ 个完整示例
FAQ：20+ 个常见问题
思考题：25+ 个设计题

---

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
