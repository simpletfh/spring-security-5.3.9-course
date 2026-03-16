graph LR
    C0[

]
    C1[

1. ]
    C2[

A: 
- ]
    C3[

A: 使用多种工具：

1. ]
    C4[

A: 可以使用以下方案：

]
    C5[

A: 有多种方式：

]
    C6[ | ⚠️ | ✅ | ✅ |
| ]
    C7[ | ⚠️ | ✅ | ✅✅ |
| ]
    C8[ | ❌ | ✅ | ✅ |
| ]
    C9[ | ❌ | ✅ | ✅✅ |
| ]
    C10[ 和 /]
    C11[")  // 只处理 /api/]
    C12[", "/api/admin/]
    C13[", "/api/public/]
    C14[", "/api/user/]
    C0 --> C1
    C1 --> C2
    C2 --> C3
    C3 --> C4
    C4 --> C5
    C5 --> C6
    C6 --> C7
    C7 --> C8
    C8 --> C9
    C9 --> C10
    C10 --> C11
    C11 --> C12
    C12 --> C13
    C13 --> C14
    classDef concept fill:#e1f5ff,stroke:#01579b,stroke-width:2px
    class C0,C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C14 concept