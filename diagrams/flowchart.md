graph TD
    Start([开始学习])
    M1[模块1 - Spring Security 快速入门]
    Start --> M1
    M2[模块2 - 认证机制深度剖析]
    M1 --> M2
    M3[第二部分：授权机制与会话管理]
    M2 --> M3
    M4[模块5 - JWT 令牌认证]
    M3 --> M4
    M5[模块6 - OAuth2 与 SSO]
    M4 --> M5
    M6[模块7：安全测试与监控]
    M5 --> M6
    M7[模块8：综合实战项目]
    M6 --> M7
    End([完成课程])
    M7 --> End
    classDef startEnd fill:#f9f,stroke:#333,stroke-width:2px
    classDef module fill:#bbf,stroke:#333,stroke-width:2px
    class Start,End startEnd
    class M1,M2,M3,M4,M5,M6,M7 module