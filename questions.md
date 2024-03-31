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

---

## 說明 Gunicorn 是什麼？該如何使用它？

### Gunicorn 是什麼？

- Gunicorn 是一個 WSGI HTTP 服務器
- Gunicorn 使用 pre-fork worker 模型，能夠在單個伺服器上處理大量的並發請求，提供高效能的 Web 服務。
- Gunicorn 可以與 Nginx、Apache 等 Web 伺服器整合，透過反向代理的方式來處理靜態檔案和負載均衡，提高系統的效能和可靠性。

### Gunicorn 如何使用

- 安裝
  ```shell
  pip install gunicorn
  ```
- 執行

  ```python
  # main.py

  from flask import Flask

  app = Flask(__name__)


  @app.route('/')
  def index():
      return 'hello world'

  def main():
      app.run(debug=True)

  if __name__ == '__main__':
      main()
  ```

  打開瀏覽器，訪問`http://localhost:8000`，你應該會看到"Hello, World!"的輸出

### 使用 Gunicorn 啟動應用程式時，你還可以使用更多的選項

```shell
gunicorn -w 4 -b 127.0.0.1:8000 app:app
```

這將啟動 4 個工作進程（`-w 4`），並將應用程式綁定到`127.0.0.1:8000`（`-b 127.0.0.1:8000`）。

## 參考資料

https://www.cnblogs.com/zongfa/p/12614459.html#:~:text=flask%E4%B8%ADgunicorn%E7%9A%84%E4%BD%BF%E7%94%A8%201%201%E3%80%81%E6%A8%A1%E5%9D%97%E5%AE%89%E8%A3%85%202%202%E3%80%81%E7%94%A8flask%E5%86%99%E4%B8%80%E4%B8%AA%E7%AE%80%E5%8D%95%E7%9A%84web%E6%9C%8D%E5%8A%A1,3%203%E3%80%81%E5%90%AF%E5%8A%A8%204%204%E3%80%81%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%205%205%E3%80%81gunicorn%E5%90%AF%E5%8A%A8flask%E5%90%8E%EF%BC%8C%E8%AE%BF%E9%97%AEapi%E5%8D%B4%E6%8A%A5404

---

## 使用守護進程 systemd 啟動 Gunicorn

1. 創建 systemd 服務文件：在`/etc/systemd/system/`目錄下創建一個以`.service`結尾的文件。

2. 編輯文件內容

   ```shell
   sudo nano /etc/systemd/system/myproject.service
   ```

   ```
   [Unit]
   Description=Gunicorn Service for myproject
   After=network.target

   [Service]
   User=your_username
   Group=your_group
   WorkingDirectory=/path/to/myproject
   ExecStart=/path/to/gunicorn myproject.wsgi:application -c /path/to/gunicorn.conf.py
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

- `your_username`：運行 Gunicorn 的用戶名。
- `your_group`：運行 Gunicorn 的組名。
- `WorkingDirectory`：指定`myproject`專案的根目錄路徑。
- `ExecStart`：指定啟動 Gunicorn 的命令。
  - `/path/to/gunicorn`：替換為 Gunicorn 可執行文件的路徑。
  - `myproject.wsgi:application`：假設你的專案使用了 WSGI 規範，並且在`myproject/wsgi.py`文件中有一個名為`application`的可調用對象。
  - `-c /path/to/gunicorn.conf.py`：如果你有一個 Gunicorn 的設定文件，請提供其路徑。

3. 重新加載 systemd
   ```shell
   sudo systemctl daemon-reload
   ```
4. 啟動 Gunicorn 服務
   ```shell
   sudo systemctl start gunicorn
   ```
5. 設置開機自啟動
   ```shell
   sudo systemctl enable gunicorn
   ```
6. 查看服務狀態
   ```shell
   sudo systemctl status gunicorn
   ```
7. 停止/重起
   ```shell
   sudo systemctl stop gunicorn
   sudo systemctl restart gunicorn
   ```
