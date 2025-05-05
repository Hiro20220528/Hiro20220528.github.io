
HTTP レスポンスのボディにはHTMLなどのリソースの内容が含まれる.

この章ではHTMLを解析する. HTMLからDOMツリーを生成する.

HTMLは以下の構成で成り立っている.
- タグ ... \<p> や \<div> など
- コンテンツ ... タグとタグの間に配置されるテキストなど
- 属性 ... タグによっては属性を持つことができる. 例えば \<p id="content"> の id が属性である.

文字列をトークンに解析する軸解析は以下のような状態遷移で行われる.

<div class="mermaid">
stateDiagram
    [*] --> DataState
    DataState --> TagOpenState: < (タグ開始)
    DataState --> CharacterReferenceState: & (文字参照)
    TagOpenState --> TagNameState: a-z, A-Z
    TagOpenState --> EndTagOpenState: /
    TagNameState --> AttributeNameState: 空白文字
    TagNameState --> DataState: \>
    AttributeNameState --> AttributeValueState: =
    AttributeValueState --> AttributeNameState: 値終了
    AttributeNameState --> DataState: \>
    EndTagOpenState --> TagNameState: a-z, A-Z
    CharacterReferenceState --> DataState: 参照完了
</div>

イメージとしては文字列を一文字づつ読み込み, その文字によって状態を遷移させる.

その後, DOMツリーを生成する.

以下の手順でDOMツリーを生成する.
<div class="mermaid">
stateDiagram
    [*] --> Initial
    Initial --> BeforeHTML
    BeforeHTML --> BeforeHEAD
    BeforeHEAD --> InHEAD
    InHEAD --> AfterHEAD
    AfterHEAD --> InBody
    InBody --> InBody
    InBody --> AfterBody
    AfterBody --> AfterAfterBody
    AfterAfterBody --> [*]
</div>

例えば上の手順で以下のsample.htmlを実行すると次のようなDOMツリーのイメージとなる

sample.html
```html
<html>
    <head>
    </head>
    <body>
        <h1>Hello World</h1>
        <div>
            <p>This is a sample html.</p>
            <ul>
                <li>item1</li>
                <li>item2</li>
                <li>item3</li>
            </ul>
        </div>
    </body>
</html>
```

DOMツリーのイメージ
<div class="mermaid">
graph LR
    html[HTML] --> head[HEAD]
    html --> body[BODY]
    body --> h1[H1]
    h1 --> hw[Hello World]
    body --> div[DIV]
    div --> p[P]
    p --> p_text[This is a sample html.]
    div --> ul[UL]
    ul --> liA[LI]
    liA --> liA_text[item1]
    ul --> liB[LI]
    liB --> liB_text[item2]
    ul --> liC[LI]
    liC --> liC_text[item3]
</div>

アルゴリズムにはFIFOを用いる.
bodyの文字列を一文字ずつ見ていきトークン化する. そのトークンによってDOMツリーを形成していく. 基本的にEndTagが出現するとキューから要素を取り出す.

以下がイメージ図
```
|    |    |    |    |    |    |    |
|    | -> |head| -> |    | -> |body| -> 
|html|    |html|    |html|    |html|
|____|    |____|    |____|    |____|


|    |    | H1 |    | p  |
|body| -> |body| -> |body|　...
|html|    |html|    |html|
|____|    |____|    |____|
```


