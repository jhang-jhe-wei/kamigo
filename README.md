# 詳細的說明文件

[Kamigo 詳細說明](https://etrex.tw/kamigo/)

# Kamigo 簡介
Kamigo 是一個基於 Rails 的 Chatbot MVC Framework。

Kamigo 讓你開發 Chatbot 就跟開發網站一樣容易，甚至可以同時開發網站以及 Chatbot 介面，共用 Controller 和 Model，只需要針對 Chatbot 實作 View。

Kamigo 提供了重要的 generator，讓你開發聊天機器人時可以快的跟飛一樣。

以下將說明如何使用 Kamigo 來製作 Todo 的教學文件。

# 建立新的 rails 專案
將以下指令全部複製，直接貼到 bash 執行即可。

```bash
# 建立新專案
rails new kamigo_demo
# 進入專案
cd kamigo_demo
# 安裝套件
bundle add kamigo
bundle add dotenv-rails
# 建立設定檔
rails g kamigo:install
# 新增 resource controller
rails g scaffold todo name desc
rails db:create
rails db:migrate
```

# 設定首頁
在 `config/routes.rb` 當中加入首頁設定：

```
root to: "todos#index"
```

# 安裝 js 套件

若使用 Asset Pipeline（Rails 5），請在 `app/assets/javascripts/application.js` 當中

加入以下程式碼：

```
//= require kamiliff
```

或者加入以下程式碼：

```
/* kamiliff default behavior */
window.addEventListener("liff_ready", function(event){
  register_kamiliff_submit();
});

window.addEventListener("liff_submit", function(event){
  var json = JSON.stringify(event.detail.data);
  var url = event.detail.url;
  var method = event.detail.method;
  var request_text = method + " " + url + "\n" + json;
  liff_send_text_message(request_text);
});
```

若使用 Webpacker（Rails 6），請在 `app/javascript/packs/application.js` 當中

加入以下程式碼：

```
/* kamiliff default behavior */
window.addEventListener("liff_ready", function(event){
  register_kamiliff_submit();
});

window.addEventListener("liff_submit", function(event){
  var json = JSON.stringify(event.detail.data);
  var url = event.detail.url;
  var method = event.detail.method;
  var request_text = method + " " + url + "\n" + json;
  liff_send_text_message(request_text);
});
```

這會影響 LINE Bot 在 LIFF 送出表單時的行為。

# 設定聊天機器人 Webhook URL
本文假設你已經有一個自己的聊天機器人，請將以下網址填入 LINE Bot 的 Webhook URL 欄位中：

```
https://你的網域/line
```

第一次開發 LINE Bot 的人可以服用此帖 [Webhook URL 設定 QA](/doc/07_setting.md#Webhook-URL-設定-QA)。

# 設定聊天機器人環境變數
請在專案根目錄下新增一個 `.env` 檔並且填入以下內容：

```
LINE_CHANNEL_SECRET=這裡填入你的 LINE_CHANNEL_SECRET
LINE_CHANNEL_TOKEN=這裡填入你的 LINE_CHANNEL_ACCESS_TOKEN
LIFF_COMPACT=這裡填入你的 COMPACT_LIFF_URL
LIFF_TALL=這裡填入你的 TALL_LIFF_URL
LIFF_FULL=這裡填入你的 FULL_LIFF_URL
```

- `LINE_CHANNEL_SECRET` 可以在 Messaging API 後台的 Basic settings 分頁中找到。
- `LINE_CHANNEL_ACCESS_TOKEN` 可以在 Messaging API 後台的 Messaging API 分頁中找到。
- `COMPACT_LIFF_URL`、`TALL_LIFF_URL` 和 `FULL_LIFF_URL` 需要到 LINE 後台的 LIFF 分頁新增後，即可獲得一組 LIFF URL。

Kamigo 預設的 LIFF Size 為 Compact，你也可以只新增 Compact LIFF URL。
詳細的 LIFF 設定說明可以服用此帖 [LIFF 設定 QA](/doc/07_setting.md#LIFF-設定-QA)。

至此串接完成。

# Rails 6 注意事項

若使用 ngrok 在本機開發時，需要在 `config/application.rb` 加入以下程式碼：

```ruby
...
module KamigoDemo
  class Application < Rails::Application
    ...
    config.hosts << "你的亂碼.ngrok.io"
  end
end
```

才能正常連線。

# 實際使用
Kamigo 預設使用基本的語意理解模型，會將使用者輸入視為在瀏覽器網址上輸入文字，並且以 LINE Flex Message 來顯示對應的結果。

開啟 LINE 和聊天機器人說 `/`，就能看到首頁的樣子。

# 使用 kamigo 製作的聊天機器人
- [kamigo demo](https://github.com/etrex/kamigo_demo)
  <p><img width="100" height="100" src="/docs/images/kamigo_demo_qrcode.png"></p>
- [健身紀錄機器人: Muscle-Man](https://github.com/louis70109/muscle_man)
  <p><img width="100" height="100" src="https://camo.githubusercontent.com/b8c51b15b20b159d356245277d079c04482acc01/68747470733a2f2f692e696d6775722e636f6d2f7534547675676e2e706e67"></p>
- 守護寵物機器人
  <p><img width="100" height="100" src="/docs/images/pet_loved_qrcode.png"></p>

# 計畫
- 提供多種語意理解模型串接
  - DialogFlow
  - LUIS
- 網站使用者與聊天機器人使用者綁定
  - Devise
- Log / 錯誤追蹤
- 支援其他平台
  - Telegram
  - Facebook Messenger
  - Slack

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
