## *cookie session webStorage*

## *cookie重要的信息*

+ 保存在浏览器端
+ 保存文本，单个不超过4kb
+ 大小受限
+ 用户可以操作（禁用）cookie，使功能受限
+ 安全性较低
+ 有些状态不可能保存在客户端。
+ 每次访问都要传送cookie给服务器，浪费带宽。

## *session重要的信息*

+ 保存在服务端

+ 大小没有限制

+ session的安全性大于cookie。

  原因: 

  > 1. sessionID存储在cookie中，若要攻破session首先要攻破cookie；
  > 2. sessionID是要有人登录，或者启动session_start才会有，所以攻破cookie也不一定能得到sessionID；
  > 3. 第二次启动session_start后，前一次的sessionID就是失效了，session过期后，sessionID也随之失效。
  > 4. sessionID是加密的
  > 5. 综上所述，攻击者必须在短时间内攻破加密的sessionID，这很难。

+ 缺点

  > 1. Session保存的东西越多，就越占用服务器内存，对于用户在线人数较多的网站，服务器的内存压力会比较大。
  > 2. 依赖于cookie（sessionID保存在cookie），如果禁用cookie，则要使用URL重写，不安全
  > 3. 创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护

## *HTML5 webStorage*

###  sessionStorage localStorage

# 链接：[cookies、sessionStorage和localStorage解释及区别(很全面)](https://www.cnblogs.com/pengc/p/8714475.html)