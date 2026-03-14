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
