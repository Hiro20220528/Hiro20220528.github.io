URLはブラウザを使用してどのWebサイトを閲覧したいかを指定するために使用する.

以下がURLの構成である.

```
https://<host>:<port>/<path>?<searchpart>
```

以下がURLの例である

```
https://www.google.com/search?q=browser
```

以下がURLの構成を表すRustのコードである.

```rust
pub struct Url {
    url: String, // https://www.google.com/search?q=browser&test=test
    host: String, // www.google.com
    port: String, // 80
    path: String, // /search
    serchpart: String, // q=browser&test=test
}
```

`Url`構造体は文字列をパースして各フィールドに値を設定する.

以下は`host`フィールドの値を取得するRustのコードである.

```rust
fn extract_host(&self) -> String {
        // ex: url = "http://example.com:8080/path/to/resource?query=string"
        let url_parts: Vec<&str> = self
            .url
            .trim_start_matches("http://")
            .splitn(2, "/")
            .collect();
        // urlをhttp://の部分を取り除いて, 次の/を探して分割する
        // ex: url_parts = ["example.com:8080", "path/to/resource?query=string"]

        if let Some(index) = url_parts[0].find(':') {
            // ポート番号が存在する場合, ポート番号の前の部分をhostとする
            url_parts[0][..index].to_string()
        } else {
            // ポート番号が存在しない場合, そのままhostとする
            url_parts[0].to_string()
        }
    }
```

上のような関数呼び出しを各フィールドごとに呼び出して初期化を行い, Url構造体を作成する.

