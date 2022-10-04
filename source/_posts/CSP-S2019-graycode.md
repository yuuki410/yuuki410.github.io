---
title: CSP-S2019 格雷码
date: 2022-10-04 10:00:00
categories: note
tags:
- CSP-S
- CSP-S2019
---

```cpp
#include<bits/stdc++.h>
using namespace std;

unsigned long long n, k, ans=0;
bool a[100];

int main(){
    cin>>n>>k;
    while(n){
        if(k<(1ull<<(n-1))){
            a[n--]=0;
            cout<<0;
            k=k&((1ull<<n)-1);
        }else{
            a[n--]=1;
            cout<<1;
            k=k&((1ull<<n)-1);
            k=((1ull<<n)-1)-k;
        }
    }
    cout<<endl;
    return 0;
}
```