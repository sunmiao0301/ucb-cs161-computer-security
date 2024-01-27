##### lecture-01-web-security



##### URLs - Uniform Resource Locator

Every resource (webpage, image, PDF, etc.) on the web is identified by a URL (Uniform Resource Locator). A typical URL consists of three parts: http://www.example.com/index.html

![full URL](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401260940689.png)

第一部分是协议，告诉你浏览去如何去检索资源，一般是http以及https（是通过TLS形成的更安全的http，称之为https）（此外还有协议mailto:` (to open a mail client), so don't be surprised if you see other protocols.）

第二部分是域名，www.example.com, tells your browser which web server to contact to retrieve the resource. Sometimes the domain name will also include a port number, such as www.example.com:81, to distinguish between different applications running on the same web server. 

第三部分是路径, index.html, tells your browser which page on the web server to request. The web server uses the path to determine which page or resource should be returned to you.

需要解释的是anchor：类似于一个书签，常见的有段落名，如一个网页包含大段大段的文字，每段文字都有一个子标题，那么anchor就可以通过这个子标题来将网页scroll到对应的位置，而不再需要手动滚动网页。

`#SomewhereInTheDocument` is an anchor to another part of the resource itself. An anchor represents a sort of "bookmark" inside the resource, giving the browser the directions to show the content located at that "bookmarked" spot. On an HTML document, for example, the browser will scroll to the point where the anchor is defined; on a video or audio document, the browser will try to go to the time the anchor represents. It is worth noting that the part after the **#**, also known as the **fragment identifier**, is never sent to the server with the request.

##### Structure of a Request

![image-20240126105307850](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401261053938.png)

##### GET vs. POST 这一部分可参见原文，有意义。

##### **https://su20.cs161.org/assets/notes/web_notes.pdf**

GET requests are intended for “getting” information from the server and generally do not change anything on the server’s end. POST requests are intended for sending information to the server that somehow modifies its internal state, such as adding a comment in a forum or changing your password

##### HTML

You are not expected to know HTML syntax for this course, but some basics are useful for some of the attacks we will cover. Here are some examples of what HTML can do:

![image-20240126105837655](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401261058688.png)

其中最后一项frame是危险的，所以现代的浏览器一般执行frame isolation，内部page和外部page之间无法修改内容。

##### Same-Origin Policy

Browsing multiple webpages poses a security risk. For example, if you have a malicious website (www.evil.com) and Gmail (www.gmail.com) open, you don’t want the malicious website to be able to access any sensitive emails or send malicious emails with your identity. Modern web browsers defend against these attacks by enforcing the same-origin policy, which isolates every webpage in your browser, except for when two webpages have the same origin

-->Two websites have the same origin if their protocols, domains, and ports all match. 

Some examples of the same origin policy: 

- http://wikipedia.org/a/ and http://wikipedia.org/b/ have the same origin. The port (http), domain (wikipedia.org), and port (none), all match. Note that the paths are not checked in the same-origin policy. 
- http://wikipedia.org and http://www.wikipedia.org do not have the same origin, because the domains (wikipedia.org and www.wikipedia.org) are different. 
- http://wikipedia.org and https://wikipedia.org do not have the same origin, because the protocols (http and https) are different. 
- http://wikipedia.org:81 and http://wikipedia.org:82 do not have the same origin, because the ports (81 and 82) are different.
- **If a port is not specified, the port defaults to 80.** This means http://wikipedia.org has the same origin as http://wikipedia.org:80, but it does not have the same origin as http://wikipedia.org:81.

##### Exceptions

In general, the origin of a webpage is defined by its URL. However, there are a few exceptions to this rule:

![image-20240126112050917](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401261120965.png)

##### Code Injection

##### SQL Injection - 建议看！

https://su20.cs161.org/assets/notes/web_notes.pdf

##### XSS

XSS利用了同源策略的漏洞，也就是说XSS攻击是在same-origin policy允许范围之内的。
XSS分为1.stored XSS，2.Reflected XSS

防御手段有：1.输入消毒，2.Content-Security-Policy一种disallow scripts的response header

##### Cookies and Session Mgmt.

由于http是无状态的，所有我们需要cookie
关于Cookie，参见下链接

https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

##### Cookie Attributes

- The Domain and Path attributes tell the browser which URLs to send the cookie to. See the next section for more details.
- The Secure attribute tells the browser to only send the cookie over a secure HTTPS connection. 
- The HttpOnly attribute prevents Javascript from accessing and modifying the cookie. 
- The expires field tells the browser when to stop remembering the cookie

An HTTP cookie remembers stateful information for the [stateless](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#http_is_stateless_but_not_sessionless) HTTP protocol.

##### Session

When a user sends a login request with a valid username and password, the server will generate a new session token and send it to the user as a cookie. 
It is easy to confuse session tokens and cookies. 
Session tokens are the values that the browser sends to the server to associate the request with a logged-in user. Cookies are how the browser stores and sends session tokens to the server. Cookies can also be used to save other state, as discussed earlier. 
In other words, session tokens are a special type of cookie that keep users logged in over many requests and responses.

##### CSRF

由于Session和Cookie的存在，CSRF攻击应运而生。

##### Defense CSRF - Cross-Site Request Forgery --> CSRF Token & Referer Validation

A good defense against CSRF attacks is to include a CSRF token on webpages.

https://owasp.org/www-community/attacks/csrf