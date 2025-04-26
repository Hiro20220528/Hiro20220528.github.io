まずは、ブラウザが画面を表示するまでの全体像をまとめる。

1. ブラウザはまず、以下の流れでデータを取得する。
<div class="mermaid">
sequenceDiagram
    participant ブラウザ
    participant DNSサーバ
    participant Webサーバ
    ブラウザ->>DNSサーバ: DNSクエリ（問い合わせ）の送信
    activate DNSサーバ
    DNSサーバ-->>ブラウザ: IPアドレスを返す
    deactivate DNSサーバ
    ブラウザ->>Webサーバ: HTTPリクエスト
    activate Webサーバ
    Webサーバ-->>ブラウザ: HTTPレスポンス
    deactivate Webサーバ
</div>

1. その次に、ブラウザは取得したデータ（HTTPレスポンス）を解析し、HTMLの部分を取り出す。

取得したHTTPレスポンスの例（HTTPはテキストベースのプロトコル）
```
HTTP/1.1 200 OK\nContent-Type: text/html; charset=UTF-8\n\n<html><head><title>My First Web Page</title></head><body><h1>Hello, World!</h1><div><p>This is a paragraph.</p><ul><li>Item 1</li><li>Item 2</li><li>Item 3</li></ul></div></body></html>
```

見やすいようにすると以下の形式となる
```html
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8

<html>
    <head>
        <title>My First Web Page</title>
    </head>
    <body>
        <h1>Hello, World!</h1>
        <div>
            <p>This is a paragraph.</p>
            <ul>
                <li>Item 1</li>
                <li>Item 2</li>
                <li>Item 3</li>
            </ul>
        </div>
    </body>
</html>
```

3. ブラウザは取得したHTMLを以下の流れで解析する。

<div class="mermaid">
graph LR
    Text[文字列] --> Token[トークン]
    Token --> DOM[DOMツリー]
</div>

解析の流れについては後の章で詳しく説明する。

4. CSS, javascriptも同様に解析する。

解析の流れについては後の章で詳しく説明する。