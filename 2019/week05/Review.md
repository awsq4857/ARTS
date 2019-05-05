# [密码学](https://mp.weixin.qq.com/s/YD6QvCxqvXl3HSnybsRFHg)

## 什么是密码学

1976年以前，加密、解密使用同一算法、秘钥。

1976年，“迪菲赫尔曼密钥交换”算法，可以在不直接传递密钥的情况下，完成密钥交换。

1977年，实现非对称加密，RSA。

## RSA数学原理

用到的公式定理
1. 取模运算
2. 欧拉函数
3. 欧拉定理，费马小定理
4. 模反元素
5. 迪菲赫尔曼秘钥交换

## RSA终端命令

1. 生成RSA私钥，`openssl genrsa -out private.pem 1024`
2. 从私钥中提取公钥，`openssl rsa -in private.pem -pubout -out public.pem`
3. 将私钥转换成为明文，`openssl rsa -in private.pem -text -out private.txt cat private.txt`
4. 通过公钥加密数据`openssl rsautl -encrypt -in message.txt -inkey public.pem -pubin -out enc.txt`，私钥解密数据`openssl rsautl -decrypt -in enc.txt -inkey private.pem -out dec.txt`
5. 通过私钥加密数据`openssl rsautl -sign -in message.txt -inkey private.pem -out enc_2.txt`，公钥解密数据`openssl rsautl -verify -in enc_2.txt -inkey public.pem -pubin -out dec_2.txt
`

## 总结

1. 由于RSA加密解密用的不是一套数据，所以其保证了安全性。
2. 由于私钥过大，所以效率较低。一般公钥加密，私钥解密。
3. 如果有一天量子计算机被普及(计算速度极快)，那么1024位已经不足以让RSA安全。

