<template>

  <div class="container">
    <h1> WebSocket Client </h1>
    <div>
      <button @click="startConnect">啟動連線</button>
      <button @click="terminateConnect">終止連線</button>
      <button @click="currentStatus">查看目前狀態</button>
    </div>
    <p>連線狀態: {{ connectStatus }}</p>
    <p>readyState: {{(readyStateCode)}}</p>
    <div>
      <label for="">傳送訊息給伺服器 </label>
      <input type="text" v-model="inputData">
      <button @click="sendData">送出</button>
    </div>
    <p>接收訊息: {{ msg }}</p>
  </div>

  <div class="container">
    <table>
      <caption>readyState 代碼對照表</caption>
      <thead>
        <tr>
          <th>常數</th>
          <th>值</th>
          <th>描述</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>CONNECTING</code></td>
          <td><code>0</code></td>
          <td>連線尚未打開。</td>
        </tr>
        <tr>
          <td><code>OPEN</code></td>
          <td><code>1</code></td>
          <td>連線已打開，可以進行通訊。</td>
        </tr>
        <tr>
          <td><code>CLOSING</code></td>
          <td><code>2</code></td>
          <td>連線正在進行關閉程序。</td>
        </tr>
        <tr>
          <td><code>CLOSED</code></td>
          <td><code>3</code></td>
          <td>連線已關閉／連線不能打開。</td>
        </tr>
      </tbody>

    </table>
  </div>

  <div class="container">
    <table>
      <caption>WebSocket 屬性一覽表</caption>
      <thead>
        <tr>
          <th>屬性</th>
          <th>形態</th>
          <th>描述</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>binaryType</code></td>
          <td><a href="/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String"><code>DOMString</code></a></td>
          <td>表示連線傳輸的二進制資料形態的字串，若使用 <a href="/zh-TW/docs/Web/API/Blob"><code>Blob</code></a> 物件則為 "blob"，使用 <a
              href="/zh-TW/JavaScript_typed_arrays/ArrayBuffer"><code>ArrayBuffer</code></a> 物件則為 "arraybuffer"。</td>
        </tr>
        <tr>
          <td><code>bufferedAmount</code></td>
          <td><a href="/zh-TW/unsigned_long" class="page-not-created"
              title="This is a link to an unwritten page"><code>unsigned long</code></a></td>
          <td>呼叫 <a href="#send"><code>send()</code></a> 隊列但尚未傳輸至網路上資料的位元數。連線關閉時此值不會重設為零。連續呼叫 <a
              href="#send"><code>send()</code></a> 會讓此值不斷上升。<strong>唯讀</strong></td>
        </tr>
        <tr>
          <td><code>extensions</code></td>
          <td><a href="/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String"><code>DOMString</code></a></td>
          <td>伺服器選擇的擴展。目前僅有空字串或表示資料經過壓縮的 "deflate-stream"。<strong>唯讀</strong></td>
        </tr>
        <tr>
          <td><code>onclose</code></td>
          <td><a href="/zh-TW/docs/Web/API/EventTarget/addEventListener"><code>EventListener</code></a></td>
          <td>當 WebSocket 連線的 <code>readyState</code> 切換至 <code>CLOSED</code> 時呼叫的事件監聽器。監聽器接收命名為 "close" 的 <a
              href="/zh-TW/WebSockets/WebSockets_reference/CloseEvent"><code>CloseEvent</code></a>。</td>
        </tr>
        <tr>
          <td><code>onerror</code></td>
          <td><a href="/zh-TW/docs/Web/API/EventTarget/addEventListener"><code>EventListener</code></a></td>
          <td>當錯誤發生時呼叫的事件監聽器。事件為命名 "error" 的簡單事件。</td>
        </tr>
        <tr>
          <td><code>onmessage</code></td>
          <td><a href="/zh-TW/docs/Web/API/EventTarget/addEventListener"><code>EventListener</code></a></td>
          <td>當瀏覽器接收伺服器的訊息時呼叫的事件監聽器。監聽器接收命名為 "message" 的 <a
              href="/zh-TW/WebSockets/WebSockets_reference/MessageEvent"><code>MessageEvent</code></a>。</td>
        </tr>
        <tr>
          <td><code>onopen</code></td>
          <td><a href="/zh-TW/docs/Web/API/EventTarget/addEventListener"><code>EventListener</code></a></td>
          <td>當 WebSocket 連線的 <code>readyState</code> 切換至 <code>OPEN</code> 時呼叫的事件監聽器，表示連線已準備傳送、接收資料。事件為命名 "open" 的簡單事件。
          </td>
        </tr>
        <tr>
          <td><code>protocol</code></td>
          <td><a href="/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String"><code>DOMString</code></a></td>
          <td>伺服器選擇的子協定，這是建立 WebSocket 物件時 <code>protocols</code> 參數裡的其中一個字串。</td>
        </tr>
        <tr>
          <td><code>readyState</code></td>
          <td><a href="/zh-TW/unsigned_short" class="page-not-created"
              title="This is a link to an unwritten page"><code>unsigned short</code></a></td>
          <td>連線的目前狀態，是就緒狀態常數的其中一個。<strong>唯讀</strong></td>
        </tr>
        <tr>
          <td><code>url</code></td>
          <td><a href="/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String"><code>DOMString</code></a></td>
          <td>建構方法解析出來的 URL，總是絕對 URL。<strong>唯讀</strong></td>
        </tr>
      </tbody>
    </table>


  </div>


  <div class="ref">
    <p>參考資料來源: </p>
    <ul>
      <li>
        <b>JavaScript | WebSocket 讓前後端沒有距離</b><br>
        https://medium.com/enjoy-life-enjoy-coding/javascript-websocket-%E8%AE%93%E5%89%8D%E5%BE%8C%E7%AB%AF%E6%B2%92%E6%9C%89%E8%B7%9D%E9%9B%A2-34536c333e1b
      </li>
      <li>
        <b>How to read BLOB data from a WebSocket which is not an image</b><br>
        https://stackoverflow.com/questions/21338263/how-to-read-blob-data-from-a-websocket-which-is-not-an-image
      </li>
      <li>
        <b>MDN WebSocket</b><br>
        https://developer.mozilla.org/zh-TW/docs/Web/API/WebSocket
      </li>
      <li>
        <b>BLOB（Binary Large Object）</b><br>
        https://developer.mozilla.org/zh-TW/docs/Web/API/Blob
      </li>
    </ul>
  </div>


</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        connectStatus: "未啟動",
        readyStateCode: "---",
        msg: "等待中",
        wsObj: null,
        inputData: ""
      }
    },
    methods: {
      disConnect() {
        return this.wsObj == null || this.wsObj.readyState != 1 ? true : false;
      },

      // 啟動連線
      startConnect() {
        //使用 WebSocket 的網址向 Server 開啟連結
        let ws = new WebSocket('ws://localhost:3000');


        // 讓VUE其他函式可以存取WebSocket
        this.wsObj = ws;

        ws.onmessage = event => {
          console.log(event);
          if (event.data instanceof Blob) {
            let reader = new FileReader();

            reader.onload = () => {
              console.log("Result: " + reader.result);
              this.msg = reader.result;
            };

            reader.readAsText(event.data);
          } else {
            console.log("Result: " + event.data);
          }
        }

        //開啟後執行的動作，指定一個 function 會在連結 WebSocket 後執行
        ws.onopen = () => {
          this.connectStatus = "已連線"
        }

        //關閉後執行的動作，指定一個 function 會在連結中斷後執行
        ws.onclose = () => {
          this.connectStatus = "連線已停止"
        }
      },

      // 送出訊息
      sendData() {
        // 檢查是否連線，未連線則提示
        console.log(this.disConnect());
        if (this.disConnect()) {
          this.msg = "(尚未連線!)";
          return;
        }

        this.wsObj.send(String(this.inputData));
      },

      currentStatus() {
        try {
          this.wsObj.readyState == null ? this.readyStateCode = "未啟動" : this.readyStateCode = this.wsObj.readyState;
        } catch (error) {
          this.readyStateCode = "未啟動";
        }

      },

      // 終止連線
      terminateConnect() {
        // 檢查是否連線，未連線則提示
        if (this.disConnect) {
          this.wsObj.close();
        }


      },
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  button {
    margin: 0 8px 0 8px;

  }

  .container {
    padding: 20px;
  }

  table{
    margin: auto;
  }

  table,
  th,
  td {
    border: 1px solid black;
    text-align: left;
  }

  li {
    padding-bottom: 10px;
  }

  .ref {
    width: 50%;
    margin: auto;
    text-align: left;
    word-wrap: break-word;
  }
</style>