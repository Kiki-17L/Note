# 一、Abstract

**JSON Web Token** (**JWT**, suggested pronunciation **[/dʒɒt/]**, same as the word "jot") is a proposed Internet standard for creating data with optional 

signature and/or optional encryption whose payload holds JSON that asserts some number of claims. The tokens are signed either using a private 

secret or a public/private key.



# 二、Structure



## 2.1 Header

Identifies which algorithm is used to generate the signature. In the below example, `HS256` indicates that this token is signed using HMAC-SHA256.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```



## 2.2 Payload

Contains a set of claims. The JWT specification defines seven Registered Claim Names, which are the standard fields commonly included in tokens.

Custom claims are usually also included, depending on the purpose of the token.

This example has the standard Issued At Time claim (`iat`) and a custom claim (`loggedInAs`).

```json
{
  "loggedInAs": "admin",
  "iat": 1422779638
}
```



## 2.3 Signature

Securely validates the token. The signature is calculated by encoding the header and payload using Base64url Encoding RFC 4648 and concatenating the 

two together with a period separator. That string is then run through the cryptographic algorithm specified in the header. This example uses HMAC-

SHA256 with a shared secret (public key algorithms are also defined). The *Base64url Encoding* is similar to base64, but uses different non-alphanumeric 

characters and omits padding.

```json
HMAC_SHA256(
  secret,
  base64urlEncoding(header) + '.' +
  base64urlEncoding(payload)
)
```



The three parts are encoded separately using Base64url Encoding RFC 4648, and concatenated using periods to produce the JWT:

```js
const token = base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)
```



# 三、Standard Fields

| Code | Name            | Description                                                  |
| ---- | --------------- | ------------------------------------------------------------ |
| iss  | Issuer          | Identifies principal that issued the JWT.                    |
| sub  | Subject         | Identifies the subject of the JWT.                           |
| aud  | Audience        |                                                              |
| exp  | Expiration Time | Identifies the expiration time on and after which the JWT **must not** be accepted for processing. The value must be a NumericDate: either an integer or decimal, representing seconds past 1970-01-01 00:00:00Z. |
| nbf  | Not Before      | Identifies the time on which the JWT will start to be accepted for processing. The value must be a NumericDate. |
| iat  | Issued at       | Identifies the time at which the JWT was issued. The value must be a NumericDate. |
| jti  | JWT ID          | Case-sensitive unique identifier of the token even among different issuers. |



# 四、Implement

在 https://jwt.io/ 网站中收录有各类语言的JWT库实现，java 目前有6个实现



1. **java-jwt**

   > Auth0实现的--maven: com.auth0 / java-jwt / 3.3.0
   >
   > Auth0提供的JWT库简单实用, 依赖第三方(如JAVA运行环境)提供的证书信息(keypair);有一问题是在 生成id_token与 校验(verify)id_token时都需要 公钥
   >
   > (public key)与密钥(private key), 个人感觉是一不足(实际上在校验时只需要public key即可)

2. **jose4j**

   > Brian Campbell实现的--maven: org.bitbucket.b_c / jose4j / 0.6.3
   >
   > jose4j提供了完整的JWT实现, 可以不依赖第三方提供的证书信息(keypair, 库本身自带有RSA的实现),类定义与JWT协议规定匹配度高,易理解与上手对称加
   >
   > 密与非对称加密都有提供实现

3. **nimbus-jose-jwt**

   > connect2id实现的--maven: com.nimbusds / nimbus-jose-jwt / 5.7
   >
   > nimbus-jose-jwt库类定义清晰,简单易用,易理解 , 依赖第三方提供的证书信息(keypair), 对称算法 与非对称算法皆有实现.

4. **jjwt**　　

   >  Les Haziewood实现的--maven: io.jsonwebtoken / jjwt-root / 0.11.1
   >
   >  jjwt小巧够用, 但对JWT的一些细节包装不够, 比如 Claims (只提供获取header,body)
   >
   >  ```java
   >  public class JwtUtils {
   >  
   >      //单位秒
   >      public static long expiration_time=60;
   >      public static String key="this is louis";
   >  
   >  
   >      // 生成token
   >      public static String generateToken(String username){
   >  
   >          Date now =new Date();
   >          Date expire = new Date(now.getTime()+expiration_time*5);
   >          return Jwts.builder()
   >                  .setHeaderParam("type","JWT")
   >                  .setSubject(username)
   >                  .setIssuedAt(now)
   >                  .setExpiration(expire)
   >                  .signWith(SignatureAlgorithm.HS256,key)
   >                  .compact();
   >  
   >      }
   >  
   >      // 解析token
   >      public static Claims getClaimsByToken(String token){
   >  
   >          return Jwts.parser()
   >                  .setSigningKey(key)
   >                  .parseClaimsJws(token)
   >                  .getBody();
   >      }
   >  }
   >  ```
   >
   >  

5. **prime-jwt**　　

   > Inversoft实现的--maven: io.fusionauth / fusionauth-jwt / 3.5.0
   >
   > prime jwt库怎么说呢, 有些地方不符合JAVA语言规范, 支持对称算法(HMAC) 与非对称算法(RSA), 也算容易理解

6. **vertx-auth-jwt**　　

   > Vertx实现的--maven: io.vertx / vertx-auth-jwt / 3.5.1 
   >
   > Vertx Auth Jwt 库算是最不容易理解的一个库了.花了不少时间才弄通这一示例. 不容易上手. 并且生成与校验id_token 时都需要公钥与私钥,不足.
