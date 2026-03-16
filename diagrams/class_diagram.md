classDiagram
    class Spring Security 课程讲义 {
        +学习目标
        +总学时
        +授课对象
    }
    class 模块1 {
        +模块1 - Spring Security 快速入门
        +知识点数量: 4
        +知识点1: Spring Security 是什么？
        +知识点2: 第一个 Spring Security 应用
        +知识点3: 核心架构概念
    }
    Spring Security 课程讲义 --> 模块1
    class 模块2 {
        +模块2 - 认证机制深度剖析
        +知识点数量: 4
        +知识点5: 认证核心原理
        +知识点6: 用户详情服务
        +知识点7: 密码编码机制
    }
    Spring Security 课程讲义 --> 模块2
    class 模块3 {
        +第二部分：授权机制与会话管理
        +知识点数量: 56
        +1. 授权概述
        +2. 授权架构
        +3. 授权流程
    }
    Spring Security 课程讲义 --> 模块3
    class 模块4 {
        +模块5 - JWT 令牌认证
        +知识点数量: 19
        +16.1 JWT 概述
        +16.2 JWT 的结构
        +16.3 JWT 的工作原理
    }
    Spring Security 课程讲义 --> 模块4
    class 模块5 {
        +模块6 - OAuth2 与 SSO
        +知识点数量: 10
        +19.1 OAuth2 概述
        +19.2 OAuth2 授权流程
        +19.3 OAuth2 安全最佳实践
    }
    Spring Security 课程讲义 --> 模块5
    class 模块6 {
        +模块7：安全测试与监控
        +知识点数量: 2
        +知识点21：安全测试
        +知识点22：安全审计与监控
    }
    Spring Security 课程讲义 --> 模块6
    class 模块7 {
        +模块8：综合实战项目
        +知识点数量: 2
        +知识点23：企业级安全系统实战
        +核心知识点
    }
    Spring Security 课程讲义 --> 模块7