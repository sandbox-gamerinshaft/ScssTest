# オレオレSass備忘録

_Sassについて調べたことや、忘れたくないことをまとめました。_

## 使う前の前提知識

### Sassの歴史

__Sass__(サース)と読む。
  CSSを効率よく書く為に、はじめSassが作られたが、不評だった為に
Scssが作られた。こちらはよりCSSっぽいシンタックスになっている。

### ファイルを scss -> css にコンパイルする方法###

`sass hoge.scss:foo/hoge.css`
このようにコマンドインタプリタに入力すると,
  `./hoge.scss` が `./foo/hoge.css`
にコンパイルされる。また、

`less hoge.css`

で中身を確認出来る。

#### 見やすい形で出力する

`sass --style expanded hoge.scss:hoge.css`

という風にして__--style__オプションを指定してあげると、
コンパイルされたコードが人間に読みやすい形と成って出力される。

#### 自動的にファイルをコンパイルする

__--watch__オプションをつけると、
ターミナルにプログラムが走り、

`sass --style expanded --watch hoge.scss:hoge.css`

で指定されたファイルがsaveされる度に自動でコンパイルしてくれる。

ディレクトリ以下を自動でapplication.cssにコンパイルする方法はこれ↓
<http://blog.scimpr.com/2013/01/08/rubyguard%E3%81%A7sasscompasscoffeescript%E3%81%AE%E8%87%AA%E5%8B%95%E3%82%B3%E3%83%B3%E3%83%91%E3%82%A4%E3%83%AB%E7%92%B0%E5%A2%83/>

### コメントアウトについて
`/* */`は複数業で書くときによく用いられる。これで書いたコメントはコンパイル後も残る。

`//  `は一行で書くときに使われる。これで書いたコメントはコンパイル後は残らない。

## Scssの機能
### &が親要素の代わりに成る
    #main{
      a{
        &:hover{
          text-decoration: none;
        }
      }
    }

### 変数で扱えるデータ型
#### 数値
    $hoge: 50px;
    .foo{
      width: $hoge;
    }
計算も出来る

    .foo{
      width: ($hoge -2) / 2;
    }
#### 色
    $warning: red;
    .hoge{
      color: $warning;
    }
#### 文字
    $imgDir: "../img/";
    .hoge{
      background-image: url($imgDir + "foo.png");
    }
もしくは

    .hoge{
      background-image: url("#{$imgDir}foo.png");
      /*こういうのも出来る*/
      width: #{12 + 12}px;
   }

__もとから使える便利な関数__
<http://sass-lang.com/documentation/Sass/Script/Functions.html>

#### 真偽(@if,@else文をつかって条件分岐)
    $debugMode: true;
    $x: 10px;
    .hoge{
      @if $debugMode == true{
        color:red;
      } @else if $debugMode == 1{
        color:green;
      } @else {
        color:yellow;
      }
      @if $x > 20 { background-color:blue;}
    }
    @for $i from 10 through 14 {
      .fs#{$i} { font-size: #{$i}px; }
    }
    $i: 10;
    @while $i <= 14{
      .fs#{$i} { font-size: #{$i}px; }
      $i: $i + 1;
    }
#### リスト
    $animals: cat, dog, tiger;
    @each $animal in $animals{
      .#{animal}-icon { background: url("#{$animal}.png")}
    }
### 関数
    $totalWidth: 940px;
    $columnCount: 5;
    @function getColumnWidth($width, $count){
    $padding: 10px;
    $columnWidth: floor(($width - ($padding * ($count -1))) / $count);
      /*数値だけ確認*/
      @debug: $columnWidth;
      @return $columnWidth;
    }
    .grid{
      float:left;
      width: getColumnWidth($totalWidth, $columnCount);
    }
### mixin
    @mixin round($px:4px){
      border-radius: $px;
    }
    .label{
      font-size: 12px;
      @include round(8px);
    }
### extend
    .box{
      width: 50px;
    }
    .box1{
      @extend .box
      height: 10px
    }