# 📘 Spring Security 5.3.9.RELEASE 课程讲义

**课程名称**: Spring Security
**教材版本**: 5.3.9.RELEASE
**课程层次**: 本科高级
**学分**: 3 学分
**总学时**: 48 学时（理论 36 + 实验 12）
**授课对象**: 计算机科学与技术专业 高年级本科生
**授课教师**: 汤
**课程学期**: 2026年春季学期

---

## 📚 课程讲义

### 📊 讲义规模

- **总字数**: 约 **270,000 字**
- **总行数**: **26,000+ 行**
- **知识点**: **22 个核心知识点**
- **代码示例**: **200+ 个可运行示例**
- **FAQ**: **110+ 个常见问题解答**
- **思考题**: **110+ 个思考题**

---

## 📖 在线阅读（飞书文档）

**📘 总索引**: [点击访问](https://feishu.cn/docx/Adzvdd9ntojLeWxV1VKcMlLknDg)

### 分部分文档

| 部分 | 内容 | 飞书文档 |
|------|------|---------|
| Part 1 | 模块1-2（快速入门+认证机制） | [📘 阅读](https://feishu.cn/docx/FONidtJuGoFQDqx48j2cGcXunPf) |
| Part 2 | 模块3-4（授权机制+会话管理） | [📘 阅读](https://feishu.cn/docx/QPZtdi3XzoN6bqxLAbvc5U7Insw) |
| Part 3 | 模块5-6（JWT+OAuth2） | [📘 阅读](https://feishu.cn/docx/Df3Udlye6o0jcEx5giicWJgkn62) |
| Part 4 | 模块7-8（安全测试+综合实战） | [📘 阅读](https://feishu.cn/docx/Qr0Wdi9KgoeGKjxAYW9c2hxzntd) |

---

## 📁 本地文件

**⭐ 推荐阅读**：
- `spring-security-course-complete.md` - **完整合并版本**（792KB，27万字，包含所有模块）

**分片文件**：
- `spring-security-part1.md` - 模块1-2（快速入门+认证机制）
- `spring-security-part2.md` - 模块3-4（授权机制+会话管理）
- `spring-security-part3.md` - 模块5-6（JWT+OAuth2）
- `spring-security-part4.md` - 模块7-8（安全测试+综合实战）

---

## 🎯 课程大纲

### 模块1️⃣: Spring Security 快速入门
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点1: Spring Security 是什么？
- 知识点2: 第一个 Spring Security 应用
- 知识点3: 核心架构概念
- 知识点4: 配置方式详解

### 模块2️⃣: 认证机制深度剖析
**理论学时**: 8学时 | **实验学时**: 2学时

- 知识点5: 认证核心原理
- 知识点6: 用户详情服务
- 知识点7: 密码编码机制
- 知识点8: 常用认证方式

### 模块3️⃣: 授权机制详解
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点9: 授权核心概念
- 知识点10: 过滤器链详解
- 知识点11: 表达式式访问控制
- 知识点12: 基于角色的访问控制

### 模块4️⃣: 会话管理与 CSRF 防护
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点13: 会话管理
- 知识点14: CSRF 防护
- 知识点15: 安全头配置

### 模块5️⃣: JWT 令牌认证
**理论学时**: 6学时 | **实验学时**: 2学时

- 知识点16: JWT 基础
- 知识点17: JWT 集成 Spring Security
- 知识点18: JWT 安全最佳实践

### 模块6️⃣: OAuth2 与 SSO
**理论学时**: 4学时 | **实验学时**: 1学时

- 知识点19: OAuth2 基础
- 知识点20: Spring Security OAuth2

### 模块7️⃣: 安全测试与监控
**理论学时**: 4学时 | **实验学时**: 1学时

- 知识点21: 安全测试
- 知识点22: 安全审计与监控

### 模块8️⃣: 综合实战项目
**理论学时**: 6学时 | **实验学时**: 0学时

- 知识点23: 企业级安全系统实战

---

## 🎯 教学方法

- **理论授课**（60%）：深入讲解核心原理、源码分析、架构设计
- **实验操作**（25%）：动手实践、代码实现、案例分析
- **项目实战**（15%）：企业级项目开发、最佳实践

---

## 📊 考核方式

- **平时成绩**（30%）：实验作业、课堂参与
- **期中考试**（30%）：理论知识测试
- **期末项目**（40%）：综合项目实战

---

## 💡 使用建议

### 学习路径

1. **第一遍**（快速浏览）：了解整体架构和核心概念
2. **第二遍**（重点研读）：深入理解认证、授权机制
3. **第三遍**（实践应用）：动手实现JWT、OAuth2等
4. **第四遍**（项目实战）：完成企业级安全系统

### 配套资源

- 官方文档：https://docs.spring.io/spring-security/site/docs/5.3.9.RELEASE/reference/html5/
- 源码仓库：https://github.com/spring-projects/spring-security
- 推荐书籍：《Spring Security 实战》

---

## 🔧 技术栈

- Spring Security 5.3.9.RELEASE
- Spring Boot 2.3.12.RELEASE
- JWT（java-jwt）
- OAuth2
- Redis
- JPA/Hibernate
- WebSocket
- Prometheus

---

## ✨ 内容特色

### 深度源码分析

- 30+ 个核心类源码分析（逐行注释）
- FilterChainProxy 工作原理
- AuthenticationManager 体系
- OncePerRequestFilter 深度解析
- SecurityContext 机制

### 完整代码示例

- 200+ 个可运行代码示例
- JWT 完整实现（生成、验证、刷新）
- OAuth2 四种授权流程详解
- 完整 RBAC 实现（数据库设计+代码）
- 企业级安全系统完整代码

### 实战导向

- Redis 集成方案
- WebSocket 实时监控
- Prometheus 集成
- 前后端分离集成
- 生产级安全最佳实践

---

## 📝 更新日志

- **2026-03-14**: ✅ 课程讲义完成并上传到飞书和GitHub
  - 生成27万字完整讲义
  - 上传到4个飞书文档
  - 创建GitHub仓库

---

## 📄 许可证

本项目采用 MIT 许可证 - 详见 LICENSE 文件

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📧 联系方式

- 授课教师：汤
- 课程学期：2026年春季学期

---

_本课程讲义由 AI 辅助生成，内容基于 Spring Security 5.3.9.RELEASE 官方文档和最佳实践。_

_生成时间: 2026-03-14 | 完成状态: ✅ 全部完成_
