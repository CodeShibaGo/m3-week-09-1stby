## 說明什麼是 WSGI，跟 ASGI 又有甚麼關係？

### WSGI 是什麼?

- WSGI (Web Server Gateway Interface ) ，一種協議，是為 Python 定義了 Web 伺服器(server)和 Web 應用程式(application)之間溝通的規範。
- 這樣的協議可以讓開發者選擇不同的 web server 跟 python web application 去做搭配使用，只需符合 WSGI

### WSGI 跟 WSGI Server 有什麼不一樣？

- WSGI 是一種協議。
- WSGI server 是指遵守 WSGI 協議的 server

### ASGI 又是什麼？

- ASGI (Asynchronous Server Gateway Interface) | (非同步伺服器閘道介面)，是 WSGI 的繼承者。
- ASGI 支持非同步編程，使得網頁應用能夠同時處理大量的並發請求，提高系統的效能和可擴展性。
- ASGI 與 WSGI 相容，現有的 WSGI 應用程式可以在 ASGI 伺服器上運行，減少了遷移的成本。

### 參考資料

- [什麼是 WSGI & ASGI ? (Python 面試題). 使用過 Python 相關後端框架開發過的人，一定會常常看到 WSGI… | by Hs | Medium](https://medium.com/@eric248655665/%E4%BB%80%E9%BA%BC%E6%98%AF-wsgi-%E7%82%BA%E4%BB%80%E9%BA%BC%E8%A6%81%E7%94%A8-wsgi-f0d5f3001652)
- [花了兩個星期，我終於把 WSGI 給搞明白了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/269456318)
- [Web 伺服器閘道器介面 - 維基百科，自由的百科全書 (wikipedia.org)](https://zh.wikipedia.org/zh-tw/Web%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%BD%91%E5%85%B3%E6%8E%A5%E5%8F%A3)
