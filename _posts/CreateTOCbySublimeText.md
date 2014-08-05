<h1>Sublime TextによるTOC生成</h1>

{:TOC}

## 参考 { #参考 }
- [SublimeText2 - Markdownの目次自動作成（ST2 PlugIn） - Qiita](http://qiita.com/naokazu_terada/items/daa34bf80fa683d80b6d)
- [コードで一言: SublimeText2でMarkdownをプレビューするプラグイン](http://codedehitokoto.blogspot.jp/2012/04/sublimetext2markdown.html)
- [SublimeText2でMarkdownにTOCを挿入する](http://bellonieta.net/2013/05/sublimetext2%E3%81%A7markdown%E3%81%ABtoc%E3%82%92%E6%8C%BF%E5%85%A5%E3%81%99%E3%82%8B/)

## 目次付きのHTML生成 { #目次付きのHTML生成 }
長文のテキストを公開するに当たっては、目次（Table of Contents）が欲しいものです。どうにか簡単にTOCを生成できないかと考え、一度はStackEdit+WordPressに落ち着きました。

ただし、StackEditが生成するTOCは、`h1`タグに相当する`#`の見出しがないと、**空のリストアイテムを生成する**という欠点があります。

StackEditの動作がかなり重いということもあり、他のソリューションを検討することにしました。

- Pandoc / Sphinx
	- オプションでTOC付けられるらしい
	- コマンドラインツールとか無理
- Notebooks for Mac
	- TOC挿入してHTMLに変換できる
	- 不具合いろいろ
		- 自動生成されるアンカーにおいて、日本語文字が一律に`.`に変換される
			- 文字数が同じだと同名のアンカーになるため、正常にジャンプできない
		- そもそもフォーマット変換の挙動が全体的に怪しい

次に検討したのがSublime Textで、これはうまくいったので手順をまとめます。なお、このドキュメント自体が、記述した方法で生成されています。

## 手順 { #手順 }
以下は、Sublime Text 3前提です。

- [Markdown Preview](https://github.com/revolunet/sublimetext-markdown-preview)パッケージを導入
- Markdownドキュメントを書く
- 任意の位置に`[TOC]`を挿入
- HTML変換時、`[TOC]`位置にアンカー付きの目次が生成される
	- HTML変換系のコマンドは、ブラウザプレビュー、HTMLエクスポートまたはビルド（⌘B）

この方式の利点は、少ない手数で、ローカルに完全なHTMLファイルが残る点です。そのため、Sublime Textの各種パッケージを利用して、FTPアップロードなどが簡単にできます。

### 注意点 { #注意点 }
- TOCからリンクされるヘッダIDにおいて、日本語文字は無視される（削除される）
	- ただし、ヘッダIDが重複した場合、自動的に連番が振られ重複が解消される
- 所定のCSSでは中国語フォントが指定されている


## さらに任意のヘッダIDを与える { #さらに任意のヘッダIDを与える }
上記の通り、日本語の見出しでヘッダIDを自動生成すると、連番のみのヘッダIDが頻出します。TOCからのリンクだけを考えるなら特に問題はありませんが、手動で文中リンクを張る場合、文書構造を変更するとヘッダIDが変更され、リンクが無効化されることになります。

ここで、Markdown PreviewパッケージによるTOCの生成が、どのようなシステムで行われているのか考えてみます。

- TOCの生成においては、Markdown Previewパッケージに組み込まれた[Python Markdown](https://pythonhosted.org/Markdown/)パーサー ライブラリが利用されている
- このパーサーには[Table of Contents拡張](https://pythonhosted.org/Markdown/extensions/toc.html)が存在し、これがMarkdown Previewパッケージから利用されている
- 同様の拡張である[HeaderId](https://pythonhosted.org/Markdown/extensions/header_id.html)を利用することで、任意のヘッダIDを生成することができる。

そこで、HeaderID拡張の仕様に従い、以下のように記述します。

	### さらに任意のヘッダIDを与える { #さらに任意のヘッダIDを与える }

これが、以下のようなHTMLに変換されます。

	<h3 id="さらに任意のヘッダIDを与える"><a name="user-content-さらに任意のヘッダIDを与える" href="#さらに任意のヘッダIDを与える" class="headeranchor-link" aria-hidden="true"><span class="headeranchor"></span></a>さらに任意のヘッダIDを与える</h3>

### 正規表現置換によりヘッダIDフォーマットを生成 { #正規表現置換によりヘッダIDフォーマットを生成 }
ヘッダIDを付けていなかったので、以下のように置換しました。

検索
: `^(#+ )(.*)$`

置換
: `$1$2 { #$2 }`

ただし、これだとヘッダIDにスペースが残ってしまい、正常にリンクされません。そのため、以下の置換を数回実行して削除しました。もう少しうまいやりかたもありそうですが…

検索
: `^(\#+.*) \{ \#(.*) (.* \})`

置換
: `$1 { #$2$3`

ただ、たまたま試したドキュメントにFenced Codeblock内のMarkdownヘッダが含まれていたため、まとめて置換されてしまい、面倒な思いをしました。


## CSSの変更 { #CSSの変更 }
先述の通り、初期状態のMarkdown PreviewによるCSSには、フォント指定の問題があります。

また、目次部分にCSSで自動的に連番を振りたかったため、CSSを変更することにしました。

### 変更方法 { #変更方法 }
Sublime Text 3のPackage Controlでインストールされるパッケージはアーカイブ化されており、そのままでは内部のファイルを編集することができません。ビルトインされたCSSファイル自体を編集するには、以下の方法でパッケージファイルを解凍する必要があります。

- [Sublime Text で Markdown． - sonoshouのまじめなブログ](http://sonoshou.hatenablog.jp/entry/2013/12/20/Sublime_Text_%E3%81%A7_Markdown%EF%BC%8E)

ただし、そこまでしなくとも、Markdown Previewパッケージの設定ファイルに外部CSSを記述することができるため、[Github](https://github.com/revolunet/sublimetext-markdown-preview)からmarkdown.cssを取ってきて、編集した上でDropboxに置き、公開リンクを発行することにしました。

Markdown Previewユーザー設定ファイルに、以下のような記述を追加します。

    "css": "https://dl.dropboxusercontent.com/u/6033699/st_markdown.css",

### markdown.cssの変更内容 { #markdown.cssの変更内容 }
12行目を以下に変更（参考：：[CSSでのフォント指定について考える（2014年） - DTP Transit（Mac OS X, OS X Mavericks, Web Fonts, Web制作, iPhone, フォント）](http://www.dtp-transit.jp/misc/web/post_1881.html)）

	font-family:Verdana, "游ゴシック", YuGothic, "Hiragino Kaku Gothic ProN", Meiryo, sans-serif;


TOC自動連番化のため、以下を追加

	/* TOCに自動連番 */
	div.toc ol,ul{
	  counter-reset: item
	}
	div.toc li { display: block }
	div.toc li a { margin-left: 0.5em;  }
	div.toc li:before { content: counters(item, "."); counter-increment: item }

この際、`H1`をMarkdownで書くとそれ込みで連番化されるので、ここだけHTMLで書いたほうがよりよいでしょう。目次化する見出しレベルを指定する方法は見つかりませんでした。

## 残る課題 { #残る課題 }
この方法で生成したHTMLの`title`要素は、ファイル名が割り当てられます。ファイル名を日本語にすることは好ましくないため、タイトルが英語になってしまうことになります。

Markdown Previewには、`title`要素を指定するために、Python Markdownのmeta拡張およびYAML Front Matterが用意されています。しかし、どちらも正常に展開することができませんでした。これは課題として持ち越し、生成されたHTMLファイルの`title`要素を手動で書き換えています。

## Writer { #Writer }
catfist (@[Twitter](https://twitter.com/catfist))