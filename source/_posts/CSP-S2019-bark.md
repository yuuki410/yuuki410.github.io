---
title: CSP-S2019 括号序列
date: 2022-10-04 10:00:00
categories: note
tags:
- CSP-S
- CSP-S2019
---

### 55分：考虑 `father[i]=i-1` 的情况

`Online`

样例：
```
01234567 // Pos
()()(()) // Stdin
01020013 // a[i] 贡献度
```

```
0123456 // Pos
((())() // Stdin
0001102 // a[i] 贡献度
```

说明：
```
for(int i=1; i<=stdin.size(); i++){
case stdin[i]=='(': stack.push(i);
                    break;
case stdin[i]==')': int tmp = stack.pop();
                    a[i]=a[father[tmp]]+1;
                    break;
}
```
