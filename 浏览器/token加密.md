如何实现token加密？

JWT(JSON Web Token)，JSON Web 令牌

1. 需要一个secret（随机数）
2. 后端利用secret和加密算法（SHA256）对payload（如账号密码）生成一个字符串（token）返回前端
3. 前端每次request在header中带上token
4. 后端用同样的算法解密