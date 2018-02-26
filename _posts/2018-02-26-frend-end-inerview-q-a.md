---
layout: post
title:  "前端面试-html篇"
---

## How should I set the language of the content in my HTML page?

### QUICK ANSWER
1. Always use a language attribute on the html tag to declare the default language of the text in the page. When the page contains content in another language, add a language attribute to an element surrounding that content.

2. Use the lang attribute for pages served as HTML, and the xml:lang attribute for pages served as XML. For XHTML 1.x and HTML5 polyglot documents, use both together.

3. Use language tags from the IANA Language Subtag Registry. You can find subtags using the unofficial Language Subtag Lookup tool.

4. Use nested elements to take care of content and attribute values on the same element that are in different languages.

## Consider HTML5 as an open web platform. What are the building blocks of HTML5?

- 语义：能够让你更恰当地描述你的内容是什么。
连通性：能够让你和服务器之间通过创新的新技术方法进行通信。
离线 & 存储：能够让网页在客户端本地存储数据以及更高效地离线运行。
- 多媒体：使 video 和 audio 成为了在所有 Web 中的一等公民。
- 2D/3D 绘图 & 效果：提供了一个更加分化范围的呈现选择。
性能 & 集成：提供了非常显著的性能优化和更有效的计算机硬件使用。
- 设备访问 Device Access：能够处理各种输入和输出设备。
- 样式设计: 让作者们来创作更加复杂的主题吧！

###### References
- https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5

## Describe the difference between a cookie, sessionStorage and localStorage.

All the above mentioned technologies are key-value storage mechanisms on the client side. They are only able to store values as strings.

|                                        | `cookie`                                                 | `localStorage` | `sessionStorage` |
| -------------------------------------- | -------------------------------------------------------- | -------------- | ---------------- |
| Initiator                              | Client or server. Server can use `Set-Cookie` header     | Client         | Client           |
| Expiry                                 | Manually set                                             | Forever        | On tab close     |
| Persistent across browser sessions     | Depends on whether expiration is set                     | Yes            | No               |
| Have domain associated                 | Yes                                                      | No             | No               |
| Sent to server with every HTTP request | Cookies are automatically being sent via `Cookie` header | No             | No               |
| Capacity (per domain)                  | 4kb                                                      | 5MB            | 5MB              |
| Accessibility                          | Any window                                               | Any window     | Same tab         |

###### References

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
* http://tutorial.techaltum.com/local-and-session-storage.html
