# Q1
請幫我寫一個簡單的HTML加原生javascript小程式，在這個HTML網頁裡有三個“選擇檔案”的按鈕，並且下方有一個“上傳”的按鈕，當點選時三個選擇檔案若有傳入檔案就會被上傳，注意這裡並不需要真的上傳檔案，只要以彈出視窗，程式上傳的每個檔案的檔名以及路徑等上傳至程式後端必備的資訊
然後這個網頁的下半部分還有一個連結指像一個檔案的網址，這個網址先以Google.com代替之後我再填入，網頁的最下方有一個按鈕叫做轉檔，當這個轉檔案按鈕點下去時，網頁的javascript會執行下載這個頁面中的連結檔案，並佔存到瀏覽器的快取目錄或是適合暫時存放檔案的儲存空間，然後js繼續執行檢查上方三個選擇檔案的地方，是否有檔案被選擇了，如果還有可上傳的地方，這會把剛剛這個下載來的檔案放入那個空的上傳地方，並且會自動執行點選上傳按鈕的操作

# A1
以下是一個符合你需求的簡單 HTML + 原生 JavaScript 小程式範例。這個程式具備：

1. 三個「選擇檔案」欄位  
2. 一個「上傳」按鈕：顯示已選檔案資訊  
3. 一個檔案下載連結（目前是指向 `https://www.google.com`）  
4. 一個「轉檔」按鈕：會模擬下載檔案，放入未填的檔案欄位並觸發上傳

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>檔案上傳與轉檔示範</title>
</head>
<body>
  <h2>選擇檔案</h2>
  <input type="file" id="file1"><br>
  <input type="file" id="file2"><br>
  <input type="file" id="file3"><br><br>

  <button onclick="uploadFiles()">上傳</button>

  <hr>

  <h2>檔案連結</h2>
  <a id="fileLink" href="https://www.google.com" download target="_blank">下載檔案</a><br><br>

  <button onclick="convertAndUpload()">轉檔</button>

  <script>
    function uploadFiles() {
      const files = [
        document.getElementById("file1").files[0],
        document.getElementById("file2").files[0],
        document.getElementById("file3").files[0]
      ];

      let message = "即將上傳的檔案資訊：\n";

      files.forEach((file, index) => {
        if (file) {
          message += `檔案 ${index + 1}:\n`;
          message += `- 名稱：${file.name}\n`;
          message += `- 類型：${file.type}\n`;
          message += `- 大小：${file.size} bytes\n\n`;
        }
      });

      alert(message);
    }

    async function convertAndUpload() {
      const fileLink = document.getElementById("fileLink").href;

      try {
        const response = await fetch(fileLink);
        const blob = await response.blob();

        const tempFile = new File([blob], "downloaded_file", { type: blob.type });

        const inputs = [
          document.getElementById("file1"),
          document.getElementById("file2"),
          document.getElementById("file3")
        ];

        let inserted = false;

        for (let input of inputs) {
          if (!input.files.length) {
            // 建立新的 DataTransfer 並插入檔案
            const dt = new DataTransfer();
            dt.items.add(tempFile);
            input.files = dt.files;
            inserted = true;
            break;
          }
        }

        if (inserted) {
          // 自動觸發上傳
          uploadFiles();
        } else {
          alert("三個上傳欄位都已有檔案，無法放入下載檔案。");
        }

      } catch (error) {
        alert("下載檔案失敗：" + error);
      }
    }
  </script>
</body>
</html>
```

---

### 使用說明：
- 可直接儲存成 `.html` 檔案並打開瀏覽器查看。
- 目前下載檔案用的是 `https://www.google.com`，之後你可以更換 `<a id="fileLink">` 的 `href`。
- `轉檔` 會自動填入空白的上傳欄位並觸發上傳功能。

如果你想加強功能，例如支援多種檔案格式、進度條、上傳進度模擬等，也可以進一步擴充。需要我幫你改進嗎？

#Q2
下載回來的檔案名稱可以保留目標網址原本的檔名嗎?
並且在下載檔案連結的下方新增兩個網址輸入框，可以自行輸入連結，並且按下轉檔時會一起檢查該兩個網址輸入框是否有值，若有則嘗試原本的轉檔動作，注意一樣是要上方三個選擇檔案的地方還沒滿才會放上去，如果已經有選一個檔案，並且兩個網址輸入框都有填入值時，則會選擇下載連結的內容，與第一個網址輸入框的內容放入剩餘空的選擇檔案
```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>檔案上傳與轉檔示範</title>
</head>
<body>
  <h2>選擇檔案</h2>
  <input type="file" id="file1"><br>
  <input type="file" id="file2"><br>
  <input type="file" id="file3"><br><br>

  <button onclick="uploadFiles()">上傳</button>

  <hr>

  <h2>檔案連結</h2>
  <a id="fileLink" href="https://img.4gamers.com.tw/news-image/8043dc2a-bcfa-4265-934d-4657d05b67a3.jpg" download target="_blank">下載檔案</a><br><br>

  <button onclick="convertAndUpload()">轉檔</button>

  <script>
    function uploadFiles() {
      const files = [
        document.getElementById("file1").files[0],
        document.getElementById("file2").files[0],
        document.getElementById("file3").files[0]
      ];

      let message = "即將上傳的檔案資訊：\n";

      files.forEach((file, index) => {
        if (file) {
          message += `檔案 ${index + 1}:\n`;
          message += `- 名稱：${file.name}\n`;
          message += `- 類型：${file.type}\n`;
          message += `- 大小：${file.size} bytes\n\n`;
        }
      });

      alert(message);
    }

    async function convertAndUpload() {
      const fileLink = document.getElementById("fileLink").href;

      try {
        const response = await fetch(fileLink);
        const blob = await response.blob();

        const tempFile = new File([blob], "downloaded_file", { type: blob.type });

        const inputs = [
          document.getElementById("file1"),
          document.getElementById("file2"),
          document.getElementById("file3")
        ];

        let inserted = false;

        for (let input of inputs) {
          if (!input.files.length) {
            // 建立新的 DataTransfer 並插入檔案
            const dt = new DataTransfer();
            dt.items.add(tempFile);
            input.files = dt.files;
            inserted = true;
            break;
          }
        }

        if (inserted) {
          // 自動觸發上傳
          uploadFiles();
        } else {
          alert("三個上傳欄位都已有檔案，無法放入下載檔案。");
        }

      } catch (error) {
        alert("下載檔案失敗：" + error);
      }
    }
  </script>
</body>
</html>
```

# A2
以下是你需求加強版的完整 HTML，功能包含：

1. 保留從網址中抓到的原始檔案名稱。
2. 增加兩個輸入框可自填檔案網址。
3. 點「轉檔」時，會依據三個選擇檔案欄位是否尚有空位，依序放入：  
   - 固定連結中的檔案  
   - 輸入框 1 的檔案  
   - 輸入框 2 的檔案  
4. 若成功放入任一欄位，會自動觸發「上傳」。

---

### ✅ 改良版 HTML + JS：

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>檔案上傳與轉檔示範</title>
</head>
<body>
  <h2>選擇檔案</h2>
  <input type="file" id="file1"><br>
  <input type="file" id="file2"><br>
  <input type="file" id="file3"><br><br>

  <button onclick="uploadFiles()">上傳</button>

  <hr>

  <h2>檔案連結</h2>
  <a id="fileLink" href="https://img.4gamers.com.tw/news-image/8043dc2a-bcfa-4265-934d-4657d05b67a3.jpg" download target="_blank">下載檔案</a><br><br>

  <input type="text" id="url1" placeholder="請輸入自訂下載連結 1" style="width: 80%;"><br><br>
  <input type="text" id="url2" placeholder="請輸入自訂下載連結 2" style="width: 80%;"><br><br>

  <button onclick="convertAndUpload()">轉檔</button>

  <script>
    function uploadFiles() {
      const files = [
        document.getElementById("file1").files[0],
        document.getElementById("file2").files[0],
        document.getElementById("file3").files[0]
      ];

      let message = "即將上傳的檔案資訊：\n";

      files.forEach((file, index) => {
        if (file) {
          message += `檔案 ${index + 1}:\n`;
          message += `- 名稱：${file.name}\n`;
          message += `- 類型：${file.type}\n`;
          message += `- 大小：${file.size} bytes\n\n`;
        }
      });

      alert(message);
    }

    async function fetchFile(url) {
      try {
        const response = await fetch(url);
        const blob = await response.blob();

        // 嘗試從網址取得檔名
        const urlObj = new URL(url);
        const pathParts = urlObj.pathname.split('/');
        const filename = pathParts[pathParts.length - 1] || "downloaded_file";

        return new File([blob], filename, { type: blob.type });
      } catch (error) {
        console.error("下載失敗：", url, error);
        alert("下載失敗：" + url);
        return null;
      }
    }

    async function convertAndUpload() {
      const urls = [];

      // 固定連結
      const staticUrl = document.getElementById("fileLink").href;
      urls.push(staticUrl);

      // 使用者輸入連結
      const url1 = document.getElementById("url1").value.trim();
      const url2 = document.getElementById("url2").value.trim();

      if (url1) urls.push(url1);
      if (url2) urls.push(url2);

      const inputs = [
        document.getElementById("file1"),
        document.getElementById("file2"),
        document.getElementById("file3")
      ];

      let inserted = false;

      for (let i = 0; i < urls.length; i++) {
        const file = await fetchFile(urls[i]);
        if (!file) continue;

        for (let input of inputs) {
          if (!input.files.length) {
            const dt = new DataTransfer();
            dt.items.add(file);
            input.files = dt.files;
            inserted = true;
            break;
          }
        }
      }

      if (inserted) {
        uploadFiles();
      } else {
        alert("三個上傳欄位都已有檔案，無法放入下載檔案。");
      }
    }
  </script>
</body>
</html>
```

---

### 🔧 說明

- 檔案名稱會自動從網址中最後一段取出（如 `xxx.jpg`）。
- 若有三個以上網址，只會選填到剩下的空欄位為止。
- 錯誤處理若下載失敗會提醒是哪個網址出錯。

---

需要我幫你加進進度條、拖曳上傳、或是進一步串後端 API 嗎？也可以轉成中文語系錯誤提示或美化 UI～

#Q3
填入某些連結會出現CORS error的錯誤，請問有辦法解決嗎?

#A3
這是一個常見的問題，讓我幫你完整解析並提供可能的解法：

---

## 🔥 問題：為什麼會出現 CORS 錯誤？

當你在瀏覽器中用 JavaScript（`fetch()` 或 `XMLHttpRequest`）去下載一個「**不是你網站網域**的資源」，如果對方伺服器沒有允許你的網頁（`Origin`）跨域存取，就會發生 **CORS（Cross-Origin Resource Sharing）錯誤**。

這不是你的錯，而是對方伺服器沒開放 CORS。

### 🔒 例如這種錯：
```
Access to fetch at 'https://some-external-site.com/file.pdf' from origin 'http://your-site.com' has been blocked by CORS policy...
```

---

## ✅ 解法（依你狀況排列）：

### ✅ 方法 1：用「代理伺服器（CORS Proxy）」繞過  
這是最通用的方式，可以在 JS 中這樣改寫網址：

```javascript
const proxy = "https://cors-anywhere.herokuapp.com/";
const realUrl = "https://some-site.com/file.pdf";
const proxiedUrl = proxy + realUrl;

const response = await fetch(proxiedUrl); // 用這個 instead
```

> ⚠️ 注意：Heroku 的 `cors-anywhere` 免費服務常有流量限制，若要穩定使用建議架設自己的 CORS Proxy。

你也可以自己部署一個，程式碼超簡單（Node.js/Express 版我可以幫你寫）。

---

### ✅ 方法 2：讓你控制的後端伺服器來下載檔案  
你可以把這個工作從「前端 JS」丟給「後端」來做：

1. 前端把網址丟給你後端（POST /download-url）
2. 後端用 `curl` 或 `requests` 下載該檔案
3. 再傳回來或處理它

這樣就完全不會碰到 CORS，因為瀏覽器就不參與下載這個跨域檔案。

---

### ✅ 方法 3：請對方伺服器支援 CORS（你若有控制權）  
設定 HTTP headers 讓對方支援跨域，例如（伺服器端）：

```http
Access-Control-Allow-Origin: *
```

但這通常只適合自己控制的 API 或靜態檔案伺服器，公開網站幾乎不會主動開。

---

### ✅ 方法 4：透過 `<iframe>` 或 `<a download>`（僅限特定情境）  
這不能取得 Blob、檔案資訊或動態處理，但你可以觸發下載：

```html
<a href="https://example.com/file.pdf" download>點我下載</a>
```

缺點是你不能把它塞進 `<input type="file">` 中。

---

## ❌ 無法解決的情況？

如果對方網站有：
- 強制驗證（Token、Cookie、Header）
- 不允許 download header
- 拒絕非自己來源

那就**沒辦法直接用前端下載**。

---

## 🚀 總結：你的選項

| 解法                 | 適用場景                             | 成功率 | 難度 |
|----------------------|--------------------------------------|--------|------|
| CORS Proxy            | 一般測試、前端繞過限制                | 中     | 低   |
| 後端下載（推薦）     | 正式環境、穩定控制                    | 高     | 中   |
| 對方開放 CORS         | 你控制資源、私有 API                  | 高     | 低   |
| `<a download>`       | 不處理檔案，只是觸發瀏覽器下載        | 中     | 低   |

---

### 如果你願意，我可以幫你部署一個簡單的 Node.js CORS Proxy，只要一個檔案，5 分鐘搞定！

要試試看嗎？或者你是用哪種環境開發（Node.js、Python Flask、PHP）？我可以幫你選最適合的解法。