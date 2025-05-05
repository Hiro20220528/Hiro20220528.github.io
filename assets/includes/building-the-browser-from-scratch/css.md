
cssは以下のような構成で成り立っている.

```
セレクタ {
    プロパティ : 値;
}
```

以下が実際の例である.
```
div {
    background-color: red;
}
```

CSSはCSSOMというCSS Object Modelのツリー構造にパースされる.

例えば, 以下のCSSは次のツリー構造となる.

```
#id {
    color: red;
}

.class {
    color: blue;
}
```

<div class="mermaid">
graph TD
    StyleSheet --> Rule1[Rule]
    Rule1 --> Selector1[Selector]
    Selector1 --> t1[#id]
    Rule1 --> Declaration1[Declaration]
    Declaration1 --> Color1[color]
    Declaration1 --> red
    StyleSheet --> Rule2[Rule]
    Rule2 --> Selector2[Selector]
    Selector2 --> t2[.class]
    Rule2 --> Declaration2[Declaration]
    Declaration2 --> Color2[color]
    Declaration2 --> blue
</div>


アルゴリズムはHTMLのパースと似ている.
先頭からトークン化していき, オブジェクトのツリーを作成する.

### レイアウトツリーの構築

DOMツリーとCSSOMツリーを結合してレイアウトツリーを作成する.
DOMツリーのうち, display: none の要素はレイアウトツリーに含めない.

レイアウトツリーを構築するアルゴリズムは, 端的に説明すると, DOMのBodyのノードからスタートし, first childとnext siblingを再帰的にたどっていく.

レイアウトツリーは以下の `LayoutView`オブジェクトと `LayoutObject`オブジェクトから成り立っている.

```rust
pub struct LayoutView {
    root: Option<Rc<RefCell<LayoutObject>>>,
}

pub struct LayoutObject {
    kind: LayoutObjectKind,
    node: Rc<RefCell<Node>>,
    first_child: Option<Rc<RefCell<LayoutObject>>>,
    next_sibling: Option<Rc<RefCell<LayoutObject>>>,
    parent: Weak<RefCell<LayoutObject>>,
    style: ComputedStyle,
    point: LayoutPoint,
    size: LayoutSize,
}
```

レイアウトツリーの作成後表示用の, display_itemsを作成する. これはレイアウトツリーから作成される. 最終的にPage構造体（ブラウザの各ページに対応する）は以下のようになる.

```rust
pub struct Page {
    browser: Weak<RefCell<Browser>>,
    frame: Option<Rc<RefCell<Window>>>,
    style: Option<StyleSheet>,
    layout_view: Option<LayoutView>,
    display_items: Vec<DisplayItem>,
}
```

ここから実際のGUIの描写に移行する.







