+++
date = '2025-04-10T21:07:47+09:00'
draft = false
title = 'Building the Browser From Scratch'
+++

「作って学ぶ」ブラウザのしくみ を読んだ

Amazonのリンクは[こちら](https://www.amazon.co.jp/dp/4297145464?ref=ppx_yo2ov_dt_b_fed_asin_title)

以下に簡単に内容をまとめる

<details>
<summary>目次</summary>

- [1. ブラウザを知る](#1-ブラウザを知る)
- [2. URLを分解する](#2-urlを分解する)
- [3. HTTPを実装する](#3-httpを実装する)
- [4. HTMLを解析する](#4-htmlを解析する)
- [5. CSSで装飾する](#5-cssで装飾する)
- [6. GUIを実装する](#6-guiを実装する)
- [7. JavaScriptを実装する](#7-javascriptを実装する)

</details>

### 1. ブラウザを知る
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
```
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


### 2. URLを分解する

### 3. HTTPを実装する

### 4. HTMLを解析する

### 5. CSSで装飾する

### 6. GUIを実装する

### 7. JavaScriptを実装する



