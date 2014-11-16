---
title: FoldingText 2 レポート
layout: post
---

<!-- # FoldingText 2 レポート -->

---

## 目次

* toc
{:toc}

---

## はじめに {#first}
本稿は、FoldingTextの私的な検証の成果であり、FoldingTextの設計思想を理解し、基本的な機能を使いこなすための手助けとなることを意図したものです。そのため、プラグイン、スクリプト、テーマなどの拡張、コマンドモードなどの高度な機能には、基本的に触れません。

正直なところ、それでもなお難解な内容になってしまったことが否めません。本稿の内容を理解せずとも、FoldingTextをそれなりに使うことは可能だと思います。

しかしながら、3,000円という決して安くないアプリである以上、**どこまでできるのか？**ということを理解した方が買いやすいのも事実ではないでしょうか？

本稿自体も、FoldingTextで作成されています。[公式サイト](http://www.foldingtext.com)ではデモ版も提供されていますので、興味を持たれた方は、ぜひ本稿を参考にFoldingTextをお試しください。

### 注意点 {#attentions}
- 本稿は、2014年6月22日現在の最新版である、2.0.2正式版の内容に基いています。
	- このバージョンにおいて、折り返し位置での日本語入力時、変換前テキストが折り返し先の行に送られる不具合があります。本件についてはサポートフォーラムに報告済みです。
- 文中のショートカットキー記述における「-」記号は、同時押しを意味します。（「- -」の記述は、マイナスキー同時押しを意味します。）
- 文中リンクは「#（節タイトル）」の形式で示します。

## Plaintext Productivity {#PlaintextProductivity}
FoldingTextの[公式サイト](http://www.foldingtext.com)では、このアプリケーションについて、以下のように説明されています。

> For Mac users who love plain text. 
> FoldingText is the markdown text editor with productivity features. Unlike other editors, FoldingText does outlining, Todo lists, and more.

このように、FoldingTextはまずMarkdownテキストエディタである、と定義されています。Markdownについては、次のリンク先をご参照ください。[Markdown - Wikipedia](http://ja.wikipedia.org/wiki/Markdown)

簡単にまとめれば、MarkdownとはHTMLの省略記法であり、一方で、そのままでも文書の構造を把握しやすくし、可読性を高めるようデザインされています。Markdownとは、HTMLでもプレーンテキストでもあるのです。

FoldingTextは、Markdownのプレーンテキストとしての性質に重きを置いています。すなわち、簡単に記述でき、他のあらゆるテキストエディタで扱うことができ、一方で構造的な文書の作成に適し、作成中の可読性を高めるための記法としてです。

実のところFoldingTextは、**HTML文書を作成するようデザインされてはいません**。その端的な表れとして、FoldingText 2.0では、HTMLプレビュー機能が実装されていません。（1.2までは存在したが、削除されている。）

Markdown記法によって文書を構造的に記述することにより、FoldingTextの高度な編集機能を活用することができます。Fold/Focusによる文書の部分表示、List記法によるアウトライン作成、文書構造の操作などです。

記法の活用はそれに留まらず、チェックボックス付きのタスクリストや、通知付きのスケジュールなどを作成することも可能です。一方で、これらの機能を擁する文書もまたプレーンテキストであり、コピー・ペースト、他のエディタによる編集、クラウドサービスによる共有などが簡単にできます。

以上のことから、FoldingTextは、以下のような用途に適するアプリケーションです。

- 論理的な文章の執筆
- 長文の執筆
- 自分向けのドキュメンテーション
- アウトラインプロセッサの代替

## Markdownおよび拡張記法 {#markdownandextensions}
本稿では、FoldingTextがサポートしているMarkdownその他の記法についてまとめます。

対応記法の一覧は、以下の通りです。

- Markdown
- [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown)の一部
	- Fenced code blocks
	- Syntax highlighting
	- Task list
- [MultiMarkdown](http://fletcherpenney.net/multimarkdown/)の一部（ハイライトのみ、HTML変換不可）
	- Footnotes
	- definition lists
- [CriticMarkup](http://criticmarkup.com)
- HTML
- 独自拡張記法（[＠拡張記法による独自機能](#extensivesyntax)）
	- Mode
	- Tag
	- Property

これらの記法は、**スマートハイライト**（[＠スマートハイライトによるテキスト装飾](#smarthighlight)）で装飾されます。また、Markdown記法は、「Copy as HTML」および「Copy as Rich Text」コマンドに反映されます。

また、以下の記法（シンタックス）は、編集時の挙動において区別されています。ここでは、仮に総称して**インラインシンタックス**と呼びます。

- Markdown
	- Bold（太字）
	- Italic（斜体）
	- Code（コード）
	- Link（リンク）
	- Image（画像）
- CriticMarkup

## 基本的な編集機能 {#basicediting}
この節では、FoldingTextのMarkdownテキストエディタとしての基本的な機能をまとめます。

### スマートハイライトによるテキスト装飾 {#smarthighlight}
スマートハイライトについては、次の記事をご参照ください。

- [カッコイイMarkdownエディタの機能にいい感じの呼び名をつけよう - 豆腐メンタルは崩れない](http://wbtmiu.hatenablog.com/entry/2014/04/30/223951)

概ね、装飾結果が編集中のテキストに反映されるとお考えください。これにより、ライブプレビューのように視線を動かすことなく、装飾の適用を確認しながら文書を書き進めることができます。

また、特筆すべき仕様を以下に列挙します。

- Horizontal Line（水平線）はセンタリングされる
- 半角スペース×2による強制改行は、改行記号を表示する
- 画像は表示されず、単にリンクとしてハイライトされる
- Listは適切にインデントされる（[＠行（アイテム）の種類](#kindofitem)）

また、FoldingTextにおいては、太字・斜体の書体を持たないフォントを適用した場合も、これらの文字装飾を適用することができます。フォントに起因する文字装飾の問題については、以下の記事をご参照ください。

- [和文フォントでMarkdownスマートハイライトが利かないのはフォントが起因だった！＋その対策 - 豆腐メンタルは崩れない](http://wbtmiu.hatenablog.com/entry/2014/05/17/220733)

日本語環境において、FoldingTextは最高級のスマートハイライト能力を持つエディタであるといえます。

また、以下のメニューコマンドにより、Markdown記号の表示をカスタマイズすることができます。

- **View**
	- **Hide Syntax**
	- **Show List Bullets**

「Hide Syntax」を有効にすると、インラインシンタックスのフォーマット文字が表示されなくなり、リッチテキストエディタのような表示も実現できます。特にリンク記法においては、表示される文字が少なくなり、文書の可読性が大幅に向上します。  
カーソルを装飾部分に合わせるとフォーマット文字が表示され、削除・再編集することもできます。

「Show List Bullets」を有効にすると、順序なしリストの記号の代わりに、一律にBullet（黒丸）を表示します。

### メニュー/ショートカットキーによるMarkdown入力支援 {#markdowninputsupport}
以下に、Markdown入力支援関係のメニューと、対応するショートカットキーを記します。それぞれの記法の内容、意味については、「Markdownおよび拡張記法」をご参照ください。

- **Format**
	- **Bold** (Command-B)
	- **Italic** (Command-I)
	- **Code** (Shift-Command-C)
	- **Add Link** (Command-K)
	- **Blockquote**
	- **Codeblock**
	- **headings**
		- **Heading1** (Command-1)
		- **Heading2** (Command-2)
		- **Heading3** (Command-3)
		- **Heading4** (Command-4)
		- **Heading5** (Command-5)
		- **Heading6** (Command-6)
	- **Lists**
		- **Unordered List** (Command-L)
		- **Ordered List** (Shift-Command-L)
	- **Addition**
	- **Deletion**
	- **Substitution**
	- **Comment**
	- **Highlight**
	- **Clear Formatting** (Control-C)

「Clear Formatting」コマンドは、フォーマット文字およびインデントを削除します。カーソル行、選択中の複数行、選択中のテキストに対して有効です。

これらのコマンドにより、MarkdownおよびCritic Markupのフォーマット文字を簡単に挿入することができます。以下に、挿入時の挙動について注意すべき点を記します。

- テキスト選択されていない状態でインラインシンタックスのフォーマット文字を挿入した場合、挿入後に中央にカーソル移動します。
- テキスト選択した状態でインラインシンタックスのフォーマット文字を挿入した場合、選択中のテキストを記号で囲みます。
	- 選択中のテキストが、既に挿入すべきフォーマット文字で囲まれている場合、フォーマット文字を削除します。
- インラインでない記法（Blockquote,Codeblock,Headings,Lists）は排他です。適用済みの行で実行した場合、既存のフォーマット文字を削除します。
	- Blockquote,Listsへの変換では、**構造レベル**が保持されます。（[＠構造レベルと親子関係](#structurelevel_parentsandchildren)）
- Horizontal Line（水平線）の挿入コマンドが存在しませんが、空行でBoldを挿入することで代用できます。

### 改行時におけるフォーマット自動補完、タブキーによるインデント {#autocomplete_indent}
FoldingTextにおいては、リスト行で改行した際、次の行に、自動的にリストのフォーマット文字が挿入されます。これは、順序なしリスト・順序付きリストの両方で有効です。順序付きリストの場合、フォーマットの数字は自動的に連番化されます。

また、行頭またはリストフォーマットの直後でタブキーを入力した場合、その行をインデントします。「Shift-Tab」でインデントを削除します。  
これらの機能により、高速にリストを入力することができます。

同様の動作は、Blockquote、Codeblockでも発生します。また、インデントされたパラグラフ（通常の行）で改行した場合、自動的に同レベルでインデントします。

この後、さらに改行を入力した場合、フォーマット文字とインデントが削除され、通常の行になります。また、これらの動作をエスケープするには、「Option-Return」「Shift-Return」「Control-Return」のいずれかを入力します。

### 文書の高度な操作 {#advancedhandling}
この節では、コマンドによる文書の編集や、クリック時の特殊な挙動について解説します。

#### 行の移動/削除 {#movedeleteitem}
以下のメニュー/ショートカットキーで、カーソル行または選択中の複数行を操作できます。「Move Branch」コマンド（[＠ツリー構造の操作](#structurehandling)）と異なり、**該当行のみ**を操作します。

- **Edit**
	- **Delete Item** (Option-Command-Delete)：行を削除
- **Organize**
	- **Move Left** (Command-[)：インデントを削除
	- **Move Right** (Command-])：インデント
	- **Move Up** (Option-Command-[)：上に移動
	- **Move Down** (Option-Command-])：下に移動

#### テキスト選択 {#selecttext}
FoldingTextでは、メニュー/ショートカットキーで、テキストの選択を行うことができます。

- **Edit**
	- **Select**
		- **Undo Select** (Control-Z)：テキストの選択を解除します。
		- **Redo Select** (Control-Shift-Z)
		- **Select Word** (Control-W)：単語（半角スペース/記号で区切られた範囲）を選択します。
		- **Select Sentence** (Control-S)：一文（行頭/ピリオドからピリオド行末まで）を選択します。日本語ではほぼ機能しません。
		- **Select Paragraph**：行を選択します。
		- **Select Branch**：Branchを選択します。（[＠構造レベルと親子関係](#structurelevel_parentsandchildren)）
		- **Expand Selection** (Option-Command-Up)：選択範囲を、「Word < Sentence < Paragraph < Branch < All」の順に拡大します。Branchは多段階です。
		- **Contract Selection** (Option-Command-Up)：同様に、選択範囲を縮小します。
		- **Select All** (Command-A)

#### 特定箇所のクリック {#specficclick}
- URLなどをクリックすると、対応するアプリケーションで開きます。
	- URL
	- Linkフォーマットのテキスト
	- `tel:`プロトコル
	- `mailto:`プロトコルおよびメールアドレス
- `#`などをクリックすることで、Fold/解除を行います。（[＠Fold](#Fold)）
- MultiMarkdown Footnoteのマーカーをクリックすると、脚注にカーソルジャンプします。
- これらの動作をエスケープするには、オプションキーを押しながらクリックします。

#### マルチカーソル {#multiplecursor}
コマンドキーを押しながらクリックすると、カーソルを増やすことができます。マルチカーソルモードでは、次の操作が有効です。

- キーボードによる基本操作
	- テキストの入力、削除
	- 矢印キー（＋α）によるカーソル移動
		- ＋シフトキーによるテキスト選択
- 一部のコマンド
	- Cut/Copy/Paste
	- インラインシンタックスフォーマット挿入

コマンドキーを押さずにクリックすると、マルチカーソルを解除します。

### 文書表示のカスタマイズ {#customizeappearance}
「View」メニューには、文書の表示をカスタマイズするコマンドが含まれます。以下に、メニュー内容およびショートカットキーを記します。

- **View**
	- **Word Count**：右下にワードカウントを表示します。ただし、日本語ではほぼ機能しません。
	- **Typewriter Scrolling**：入力中の行を画面中央に表示します。
	- **Text Wrap Width**：テキストの折り返し位置を選択します。文字数は半角で計算されます。（Column 60は全角30文字相当。）そのため、「Wrap to Window」以外では、等幅フォントの使用が推奨されます。「折り返しなし」は選択できません。
		- **Column 60**
		- **Wrap to Window**
		- **Wrap to Window**
		- **Wrap to Window**
		- **Wrap to Window**
		- **Wrap to Window**
		- **Wrap to Window**
	- **Actual Size** (Command-+)：文字サイズを大きくします。画面端に達しない限り、1行あたりの文字数を保持します。
	- **Actual Size** (Command--)：文字サイズを小さくします。
	- **Actual Size** (Command-0)：文字サイズを標準にリセットします。

また、CSSの記述によって、より詳細な外観のコントロールが可能です。（[＠スタイルのカスタマイズ](#customizestyle)）

### 異なる形式でコピー {#varietyofcopy}
以下のメニュー/ショートカットキーで、選択中のMarkdownテキストを変換してコピーすることができます。簡易的なエクスポート機能として利用できます。

- **Edit**
	- **Copy**
		- **Copy as HTML** (Option-Command-C)
		- **Copy as Rich Text** (Control-Command-C)

### ファイルの保存 {#savefiles}
ローカルおよびiCloudに保存可能です。

初期設定では、新規ファイル保存の際、独自の拡張子`.ft`が自動的に挿入されます。`.ft`ファイルは、内容的にはプレーンテキストファイルですから、一般的なテキストエディタで問題なく表示・編集できます。  
拡張子の自動挿入をオフにするには、ファイル保存の際、「If no extension is provided, use ".ft".」のチェックを外します。

また、一般的なプレーンテキスト（Markdown）ファイルの拡張子である`.txt`、`.md`等を指定した場合でも、[拡張記法による独自機能](#extensivesyntax)を含むFoldingTextの全機能を問題なく利用することができます。

## 構造的な文書の作成 {#documentsstructure}
FoldingTextの強みは、構造的な文書の作成にあります。Markdown記法自体がそのような用途に適していますが、FoldingTextはMarkdownの独特の解釈と編集支援機能により、それをさらに推し進めています。

具体的な機能の解説に入る前に、FoldingTextにおける文書構造の定義について解説します。

### 構造レベルと親子関係 {#structurelevel_parentsandchildren}
最初に、FoldingTextは、HTML文書を作成するようデザインされてはいないと述べました。これは、HTMLにおける、**ブロック要素**と**インライン要素**の概念をほぼ無視しているためです。例えば、次のテキストをご覧ください。

	- list
	- list
	- list

これはMarkdownリストであり、HTML変換すると、ひとつの`ul`タグで括られるブロック要素になります。その場合のソースは以下の通りです。

<pre><code>
&lt;ul&gt;
	&lt;li&gt;list&lt;/li&gt;
	&lt;li&gt;list&lt;/li&gt;
	&lt;li&gt;list&lt;/li&gt;
&lt;/ul&gt;
</code></pre>

しかし、FoldingTextにおいては、このような解釈にほとんど意味がありません。

むしろ重要なのは、**行同士の関係**における、**構造レベル**および**親子**の概念です。次のテキストをご覧ください。

	- list1
		- list2
			- list3

このテキストにおいては、各行に構造レベルの差があります。すなわち、各行は「list1 < list2 < list3」の構造レベルを持ちます。

また、list3はlist2の、list2はlist1の、それぞれ**子**として扱われます。list3はlist1の孫（子の子）です。

このふたつの概念によって、FoldingTextにおける文書は、**行を単位とするツリー構造を成します**。ブロック要素が単位にならないことにご注意ください。

#### 行（アイテム）の種類 {#kindofitem}
FoldingTextにおける**行の種類**には、次のようなものがあります。

1. Headings（見出し）
2. Pragraphs（パラグラフ、通常の行）
    - Blockquote（引用ブロック）
    - Codeblock（コードブロック）
3. Lists（リスト）
    - Unodered Lists（順序なしリスト）
    - Ordered Lists（順序付きリスト）

順序付きリストで記述したのは、行の種類によって、**構造レベルの差**があるためです。数字が少ないほど構造レベルが低く（_文書における重要度が高く_）なります。また、低位の行の下に高位の行を記述すると、**親子関係が成立します**。

例えば、以下のようなテキストにおいては、各行に親子関係が成立しています。

	# Heading1

	Paragraph1

	- List1

親子関係をツリー構造で整理すると、以下の通りです。

- Heading1
	- Paragraph1
		- List1

List1はParagraph1の子であり、Paragraph1はHeading1の子であり、List1もまたHeading1の子孫です。

文書における、このようなツリー構造を持つ**部分**を、**Branch（枝）**と呼びます。この部分において、「Paragraph1およびList1」はBranchです。また、文書全体において、この部分はBranchです。

さらに、以下の要因によって、同種の行における構造レベルの差が生じます。Codeblockのみ、構造レベルの変化がありません。

- `#`の数による見出しレベルの差
- 引用ブロック内の引用ブロック
- Paragraphのインデント
- Listのインデント

見出しは絶対的な最下位（最重要）アイテムであり、その他の種類の行と等しい構造レベルを持つことはありません。  
「n段階インデントされたList」の構造レベルは、「n+1段階インデントされたParagraph」または「n+1段階引用されたBlockquote」と等しくなります。つまり、Listフォーマット自体が、Paragraph（Blockquote,Codeblock）より1レベル高い構造レベルを持つことになります。

特にListおよびParagraphにおいては、**構造レベルの等しい行は同じインデント幅**で表示されます。これにより、構造レベルを視覚的に把握しやすくなっています。

例えば、次のスクリーンショットにおいて、Pragraph2とList1、Paragraph3とList2は同じ構造レベルを持ちます。行頭の位置にご注目ください。

![構造レベル](http://cl.ly/W3fv/image.jpg)

ただし、半角スペースによってListをインデントした場合、表示にずれが生じるばかりでなく、スマートハイライトや、コマンドによる編集に支障を来す場合があります。特に、他のエディタと併用する場合に注意が必要です。

#### 構造レベルと親子関係の例 {#examplestructure}
以下のような文書があるとします。

	# Heading1

	## Heading2-1
	Paragraph1

	- list1-1
		- list1-2

	### Heading3
	Paragraph2-1
		Paragraph2-2

	- list2-1
		- list2-2
			- list2-3

この文書の**親子関係**は、以下の通りです。

- Heading1
	- Heading2-1
		- Paragraph1
			- list1-1
			- list1-2
	- Heading2-2
		- Paragraph2-1
			- Paragraph2-2
			- list2-1
				- list2-2
					- list2-3

Paragraph1とlist1-1が親子関係にある一方、Paragraph2-2とlist2-1が親子関係にないことに注目してください。

さらに、構造レベルを以下に示します。行頭の数字が等しいアイテムは、同じ構造レベルを持ちます。

	1.Heading1
		2.Heading2-1
				4.Paragraph1
					5.list1-1
						6.list1-2
			3.Heading3
				4.Paragraph2-1
					5.Paragraph2-2
					5.list2-1
						6.list2-2
							7.list2-3

構造レベル5の部分に注目してください。Paragraph2-2とlist2-1が親子関係とみなされないのは、**構造レベルが等しい**ためです。このように、同じ親と等しい構造レベルを持つ行同士は、**兄弟関係**になります。

FoldingTextにおける文書は、このような、Branchの集合であるツリー構造を成しています。  
ツリー構造を把握することによって、次に述べる**ツリー構造の操作**や、**Fold/Focus**機能（#[Fold/Focus](#FoldFocus)）を使いこなすことができます。

### ツリー構造の操作 {#structurehandling}
アイテムの構造レベルを変更する第一の方法は、既に述べたフォーマット挿入、またはインデントを行うことです。

あるいは、以下のメニューによって、その行の構造レベルを変更することができます。

- **Organize**
	- **Decrease Level**
	- **Decrease Level**

つまり、見出しおよび引用ブロックに対してはフォーマットを追加/削除し、パラグラフおよびリストに対してはインデントを追加/削除します。パラグラフおよびリストに対しては、Move Left/Rightと同様の効果です。

一方で、以下のメニュー/ショートカットキーで、ツリー構造を直接的に操作することができます。

- **Organize**
	- **Move Branch Down** (Control-Option-Left)
	- **Move Branch Down** (Control-Option-Right)
	- **Move Branch Down** (Control-Option-Up)
	- **Move Branch Down** (Control-Option-Down)

以上のコマンドはMoveコマンドと似ていますが、その対象は**行ではなくBranch**になります。すなわち、**対象のBranchの構造を保持したまま**、何らかの操作を加えます。

これらを使いこなすことにより、アウトラインプロセッサのような文書構造の変更を容易に行うことができます。ただし、コードブロックに対しては無効です。（コードブロック内では構造が無視されるため。）

#### Move Branch Left/Right {#MoveBranchLeftRight}
これらのコマンドは、対象となるアイテムの構造レベルを減少/増加させます。対象となるアイテムは、カーソル行（選択された複数行）および**その同種の子**です。ただし、操作結果が不正な構造になる場合は無効です。

例えば、以下のような文書があるとします。

	- list1
		- list2
			- list3
		- list4
			- list5

list4に対して「Move Branch Right」を実行すると、以下のようになります。

	- list1
		- list2
			- list3
			- list4
				- list5

その他の行に対しては、操作結果が不正な構造になるため、「Move Branch Right」は無効です。

また、list2に対して「Move Branch Left」を実行した場合は、以下のようになります。

	- list1
		- list4
			- list5
	- list2
		- list3

list1とlist2が同レベルになったため、構造の正しさを保つため、自動的に並べ替えが行われます。

さらに、次のような文書があるとします。

	# Heading1
	Paragraph
	## Heading2-1
	Paragraph
	## Heading2-2
	Paragraph
	### Heading3
	Paragraph

Heading2-2に対して「Move Branch Right」を実行すると、以下のようになります。子であるHeading3の構造レベルが変化していることにご注目ください。

	# Heading1
	Paragraph
	## Heading2-1
	Paragraph
	### Heading2-2
	Paragraph
	#### Heading3
	Paragraph

このように、Move Branch Left/Rightは、**親子関係を変化させ（入れ替え）ます**。親子を兄弟に、また兄弟を親子にすることができます。また、Blockquote内Blockquoteを作成するためにも利用できます。

#### Move Branch Up/Down {#MoveBranchUpDown}
これらのコマンドは、対象となるアイテムを上下に移動、あるいは**同じ親を持つ（兄弟である）**上下のアイテムと入れ替えます。対象となるアイテムは、カーソル行（選択された複数行）および**その子全て**です。**異種のアイテムを含む**ことにご注意ください。

例えば、以下のような文書があるとします。

	- list1
		- list2
			- list3
		- list4
			- list5

list4に対してMove Branch Upを実行すると、以下のようになります。list5が同時に移動します。

	- list1
		- list4
			- list5
		- list2
			- list3

さらに、次のような文書があるとします。

	# Heading1
	## Heading2-1
	Paragraph1
	- list1-1
	- list1-2
	Paragraph2
	- list2-1
	- list2-2
	## Heading2-2
	Paragraph3

Paragraph2に対してMove Branch Upを実行すると、以下のようになります。list2-1/list2-2が同時に移動します。

	# Heading1
	## Heading2-1
	Paragraph2
	- list2-1
	- list2-2
	Paragraph1
	- list1-1
	- list1-2
	## Heading2-2
	Paragraph3

さらに、Heading2-2に対してMove Branch Upを実行した場合は、以下のようになります。Heading2-2のセクションと、Heading2-1のセクションが、丸ごと入れ替えられます。

	# Heading1
	## Heading2-2
	Paragraph3
	## Heading2-1
	Paragraph1
	- list1-1
	- list1-2
	Paragraph2
	- list2-1
	- list2-2

このように、Move Branch Up/Downは、**兄弟同士を入れ替えます**。親子関係にあるアイテム同士を入れ替えるには、Move Branch Left/Rightによって、あらかじめ兄弟関係に変更する必要があります。

#### MoveとMove Branchの使い分け {#moveormovebranch}
「Move Up/Down」および「Move Branch Up/Down」コマンドは、いずれも行（アイテム）を並べ替えます。ただし、「Move」は構造を無視し、「Move Branch」は構造を保持します。

このため、「Move Branch」コマンドは、子アイテムを含めたBranch単位の並べ替えに適します。一方、「Move」コマンドは、アイテムをBranchから別のBranchへ移動するのに適します。特に、別のHeadingセクションへ移動する場合に有用です。

## Fold/Focus {#FoldFocus}
Fold/Focusは、いずれも、文書の一部を非表示にすることで、文書を視覚的に把握しやすくする機能です。ただし、以下のような違いがあります。

- Fold：非表示にするアイテムを指定する。
- Focus：表示するアイテムを指定し、その他のアイテムを自動的にFoldする。

この説の記述は簡易に留めてあります。それは、Fold/Fucusに関する理解は、FoldingTextにおける構造の理解に等しいと考えるためです。Fold/Fucusの詳しい挙動については、前節をご参照ください。

### Fold {#Fold}
Foldは、基本的に見出しによって区切られたセクションを対象とします。見出しアイテムの行頭にある`#`をクリックすることで、その子全てを非表示にします。これには、その他の（よりレベルの高い）見出しアイテムも含まれます。

Foldを解除するには、`#`を再度クリックするか、Fold部分を示す`…`記号をクリックします。

また、以下のメニュー/ショートカットキーでFold/解除することもできます。

- **View**
	- **Fold** (Command-/)

このコマンドは、見出し以外の構造に対しても有効です。あらゆるBranchにおいて、その子全てをFoldします。特に、リストにおいて有用です。

なお、Fold部分を示す`…`記号をCut/Copy/削除した場合、操作結果がFoldされたテキスト全てに適用されます。

#### 構造レベルに基づく順次Fold {#inorderfold}
以下のメニュー/ショートカットキーによって、対象のBranch（カーソル行の子全て）において、構造レベルの高い（重要度の低い）アイテムから順に、解除/Foldすることができます。

- **View**
	- **Collapse by Level** (Option-Command-Right)
	- **Collapse by Level** (Option-Command-Left)

Branch内の親子関係が無視されることにご注意ください。また、文書内の他のBranchは、もっと構造レベルの高いアイテムがある場合であっても無視されます。

このコマンドは、文書の大半をFoldしておき、その内容を**重要度に応じて読み進める**場合に特に有用だと考えられます。例えば、見出しアイテムのみを残してCollapseし、目的の見出し以下のアイテムを徐々にExpandするなど。

### Focus {#Focus}
Focusは、**対象のBranchのみを表示させ**、その他のアイテムを全てFoldします。以下のメニュー/ショートカットキーで実行できます。

- **View**
	- **Focus** (Command-U)

Foldの挙動は、カーソル行が子を持つかどうかによって分かれます。

- 子を持つ：**カーソル行とその子**のみを表示する
- 子を持たない：**その直接の親を対象とし**、対象とその子のみを表示する

このコマンドは、作成中の文書において、その一部のみを編集する場合などに特に有用だと考えられます。

#### TagによるFocus {#foldbytag}
行（アイテム）には、Tagを付けることができます。Tagは、`@done`のように、`@`によって記述します。（[＠Tag](#Tag)）

このTagをクリックすることで、同じTagを持つアイテムを対象にFocusすることができます。しかし、TagによるFocusは、通常のFocusと挙動が大きく異なります。

- 子がFoldされる
- 直接の親でないもの（祖父・曽祖父…）も含め、全ての親アイテムが表示される
- 親/子のいずれも持たないアイテムもFocusされる

以下のメニュー/ショートカットキーで、文書内のTagを一覧表示し、選択したTagをFocusすることができます。

- **View**
	- **Focus Heading…** (Control-Command-U)

#### HeadingによるFocus {#foldbyheading}
一方、以下のメニュー/ショートカットキーで、文書内の見出しを一覧表示し、選択した見出しをFocusすることができます。この場合の挙動は、通常のFocusと同様です。

- **View**
	- **Focus Heading…** (Option-Command-U)

この際、見出しがツリーで表示されるため、簡易的なTable of Contents生成機能としても利用できます。また、擬似的な見出しへのジャンプ機能としても利用できます。特に長文の校正時に有用です。

### 全てのFoldを解除 {#removeallfold}
Foldを含む文書では、左上隅に`…`記号を含む三角形が表示されます。これをクリックすることで、まず**Focusによる自動的Fold**が解除されます。自動的Foldがない状態でクリックすると、FoldコマンドによるFoldが全て解除されます。

さらに、以下のメニュー/ショートカットキーでは、全てのFoldを一括解除できます。

- **View**
	- **Show All** (Shift-Command-A)

特に「Focus Heading…」と併用することで、文書の校正を効率的に行うことができます。

## 拡張記法による独自機能 {#extensivesyntax}
これまで、FoldingTextのテキストエディタとしての側面について述べてきました。しかし、FoldingTextはそれに留まらず、特有の拡張記法によって、さらに機能を拡張することができます。

本節では、FoldingText特有の拡張記法について解説します。

### Mode {#Mode}
Mode記法を利用することで、FoldingTextに追加の機能を与えることができます。Mode記法は、以下のような形になります。

    （任意のテキスト）.（Modeを指定する語）

この形式で書かれた行は、その下にある**特定形式の行**に対して、通常とは異なるbehavior（振る舞い）をさせます。対応しない形式の行が書かれた場合、それより下の行にModeは影響しません。  
なお、Mode行は見出しにすることもできます。任意のテキスト部分については、インラインシンタックスによる装飾も可能です。

最初から実装されているModeとして、Todo ModeとTimer Modeがあります。Modeは、ユーザーが追加することも可能なようです。

#### Todo {#Todo}
Todo Modeは、チェックボックス付きのタスクリストを作成することができます。  
このModeを利用するには、`.Todo` で終わる行の下に、順序なしリストを書きます。例えば、以下の通りです。

	## [FoldingText](http://www.foldingtext.com)概論.Todo
	- toc
	- リストのツリー化
		- HTML変換
		- コード書き換え
		- CSS配置
	- 内容
		- マルチカーソルが有効なコマンドについて
		- Critique Markup箇所のチェック
			- Fold記号

このリストのフォーマット文字（またはBullets）は、チェックボックスに置き換えられます。  
チェックボックスをクリックすると、`@done`タグおよび終了日が挿入され、完了タスク扱いになります。完了タスクは、ボックスがチェックされ、打ち消し線が表示されます。

また、直接`@done`を入力することもできます。この場合、終了日は手動で入力しない限り挿入されません。また、**テキストへの打ち消しはTodo Mode以外でも有効です**。全ての`@done`アイテムは、自動的に打ち消し線で修飾されます。

以下が、そのスクリーンショットです。

![Todo](http://cl.ly/W3hb/image.jpg)

また、日時の記録、TagによるFocus等の高度な処理は行えませんが、以下のようなGithub Flavored Markdown形式のタスクリストも作成できます。この際、「Show List Bullets」が適用されないことにご注意ください。

	- [ ] 未完了タスク
	- [x] 完了タスク。打ち消しされる。

##### 完了タスクのアーカイブ {#archivecompletedtask}
これは高度な操作ですが、極めて有用なため、例外的に記述します。

次の手順により、完了タスク（`@done`アイテム）を文書最下部に移動することができます。

1. コマンドモードを起動  
「**Edit** > **Run Command** (Command-')」
2. 「archive @done」を実行  
インクリメンタルサーチ可能

結果として、「# Archive」セクションへ、全ての完了タスクが移動されます。これには、Todo Mode以外のアイテムも含まれます。「# Archive」セクションが存在しない場合、文書最下部に自動生成されます。これにより、タスクリストの内容を、未完了タスクのみに保つことができます。

タスクリストが多階層であり、完了タスクが子を持つ場合、**子も含めて移動される**ことは注目に値します。このため、全ての子タスクが完了した場合、親タスクを完了済みにするだけで、全ての子タスクを「アクティブな」タスクリストから排除できるのです。

これは、同作者のタスク管理アプリケーション「TaskPaper」で初めて実装された機能です。TaskPaperは、現在もMac App Storeで利用できます。

#### Timer {#Timer}
Timer Modeでは、スケジュールを作成することができます。

このModeの記述方法は、実際のテキストからはわかりにくいため、先に手順を記します。

1. `.Timer` フォーマットを含む行を記述
2. その下にスケジュールを記述
3. スケジュール行は、**インデントし、所要時間を英語で記入する**
	- 所要時間の形式は「数字＋単位（day/hour/minute/second）」
		- 行末でなくてもよい
		- 数字は小数でもよい。ただし計算されるのは1秒単位まで
		- 数字が1であるかにかかわらず、単位は単数形でも複数形でもよい
		- 数字と単位の間には、スペースを入れても入れなくてもよい
4. スケジュール行の先頭に開始時刻が、スケジュール行の下に終了時刻が、それぞれ自動的に表示される
	- 時刻は現在時刻を基準にする
	- 時刻を編集することはできない
5. いずれかのスケジュールの**開始時刻をクリックする**と、それを現在時刻にリセットした上でカウントを開始する
	- 開始日時が、 `.Timer` 行の直下に挿入される
		- これは編集できる
	- それより過去のスケジュールがある場合、その開始時刻も合わせてリセットされる
6. 実行中は、以下の挙動を行う
	- 次のスケジュールの開始時刻に、ポップアップ通知を行う
	- 実行中のスケジュールをハイライトする
	- 他のスケジュールの開始時刻をクリックすると、改めて時刻をリセットしてカウント開始
		- スケジュールの前倒しが簡単にできる
	- 実行中にスケジュールを編集可能
		- その場合、時刻は適宜リセットされる

まとめると、記述例は以下のようになります。

	朝.Timer
		Start : Jun 8, 2014 7:00:00 AM
		朝食 0.4hour 冷蔵庫
		風呂 2000second
		体操 10 minutes
		洗濯 0.01 days

実際の表示は、以下のスクリーンショットのようになります。

![Timer](http://cl.ly/W3fk/image.jpg)

### Tag {#Tag}
`@`によるTagは、FocusとModeによって利用されます。例えば、`@done`タグは、「Focus Tag…」コマンドでFocusされるだけでなく、Todo Modeで完了タスクとして解釈されます。

Tagの直後に`()`を記述すると、その中身が**Value**としてTagに与えられます。Valueは、Modeによってメタデータとして利用される一方、Focusの対象Tagを絞り込むためにも利用されます。  
あるTagのValueをクリックすると、同じTagのうち、等しいValueを持つものだけをFocusします。同じValueを持つ異なるTagはFocusされません。

例えば、次の文書で1行目の`a`をクリックすると、aaaだけがFocusされます。

	aaa @sample(a)
	bbb @sample(b)
	ccc @sample
	ddd @sample2(a)

なお、以下の単語はアプリケーションに予約されているため、Tag名として利用することができません。Tag名に「含める」ことは可能です。

"id", "class", "type", "line", "mode", "modeContext", "level", "property", "name", "value"

### Property {#Property}
Propertyは、Modeにメタデータとして利用されます。Focusには利用されません。例えば、先述のTimer Mode使用例における、以下の部分がPropertyです。

    Start : Jun 8, 2014 7:00:00 AM

Propertyを作成するには、Property名を入力し、続けて " : " (半角スペース,コロン,半角スペース)をフォーマットとして入力します。
Propertyは独立した行であり、Property名は一単語（半角スペースを含まない）である必要であります。フォーマットの後ろに入力されたテキストは、全てValueになります。

例えば、先の例で言えば、「Start」がProperty名、「Jun 8, 2014 7:00:00 AM」がValueに当たります。

## スタイルのカスタマイズ {#customizestyle}
FoldingTextはCodeMirrorを採用しており、エディタの外観（入力テキストの表示を含む）は、LESS形式のCSSでコントロールされています。CSSを記述することで、エディタの外観を変更することができます。本稿では、CSS/LESSの詳細については解説しません。

ユーザーCSSは、「~/Library/Application Support/FoldingText」に置かれている「user.less」に記述します。このフォルダは、以下のメニューで開くことができます。

- **File**
	- **Open Application Folder**

LESS変数は、コメントアウトした状態で、あらかじめ「user.less」に記述されています。これを編集することで、容易にエディタの外観を変更できます。

例として、以下のように記述すると、フォントを変更することができます。

    @fontFamily: "MigMix 1M";

変更は、保存後に新たに開いたファイルに対して適用されます。アプリケーションの再起動は必要ありません。

## Writer {#Writer}
catfist (@[Twitter](https://twitter.com/catfist))