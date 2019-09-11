cookie和token

- cookie：服务员看你的身份证，给你一个编号，以后，进行任何操作，都是出示编号后服务员去查看你是谁
- token：直接给服务员看自己身份证



cookie：登录后后端生成一个sessionid放在cookie中返回给客户端，并且服务端一直记录着这个sessionid，客户端以后每次请求都会带上这个sessionid，服务端通过这个sessionid来验证身份之类的操作。所以别人拿到了cookie拿到了sessionid后，就可以完全替代你



token： 登录后后端返回一个token给客户端，客户端将这个token存储起来，然后每次客户端请求都需要开发者手动将token放在header中带过去，服务端每次只需要对这个token进行验证就能使用token中的信息来进行下一步操作了





XSS：跨站脚本攻击（Cross Site Scripting），为了和层叠样式表CSS（cascading style sheets）区别，所以缩写为XSS

- 攻击者通过各种方式将恶意代码注入到其他用户的页面中，就可以通过脚本获取信息，发起请求等操作
- 劫持cookie或者localStorage，从而伪造用户身份相关信息。
- 前端token存储在cookie、localStorage、sessionStorage，都是通过js代码获取到的。



CSRF：跨站请求伪造（Cross-site request forgery），

- 攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站，并运行一些操作（如发邮件、发消息、购物）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。这利用了web中用户身份验证的一个漏洞：**简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的**。
- csrf并不能拿到用户的任何信息，它只是欺骗用户浏览器，让其以用户的名义进行操作



对于XSS攻击来说，cookie和token没有什么区别，但是对于CSRF来说就有区别了（只劫持cookie，不劫持token的原因）：

- cookie：用户点击了链接，cookie未失效，导致发起请求后后端以为是用户正常操作，于是进行扣款操作
- token：用户点击链接，由于浏览器不会自动带上token，所以即使发了请求，后端的token验证不会通过，所以不会进行扣款操作





- token不是防止XSS的，而是为了防止CSRF
- CSRF攻击的原因是浏览器会自动带上cookie，而不会自动带上token