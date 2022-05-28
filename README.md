# This is a starter template for [Learn Next.js](https://nextjs.org/learn).

---

Vercel へのデプロイ版
https://first-nextjs-blog-pi.vercel.app/

<br>

---

## 各フォルダの役割

<br>

### components

各ページ毎に CSS/JS を付与する場合はこのフォルダに入れる。

本プロジェクトの場合、layout.js(layout.modules.css)は pages/index.js ページのみを修飾している。
<br>

### lib

ページレンダリングそのものには関係の無いツール関連を格納する。

本プロジェクトの場合、ファイルシステムからデータを取得するためのライブラリ(posts.js)が格納されている。
<br>

## pages

クライアントから直接呼び出されるページファイルが格納されている。

特にこのフォルダ内に直接 js ファイルを追加することで自動的にルーティングが作成される。

ただし、\_app.js は全体設定のためのインターフェイスで、直接クライアントから呼び出されない。

(Next.js はページを初期化するために App コンポーネントを使用しており、App コンポーネントをオーバーライドすることで、
ページの初期化をコントロールすることが出来る。)
<br>

### posts

ブログにおける記事を格納している。

各種記事に最初に書かれている---で囲われている箇所は YAML Front-matter と呼ばれ、markdown のヘッダーにメタ情報を配置するなどの処理を行うことが可能。

NextJs においては gray-matter を用いて解析される。
コンテンツを分けることにより、実際に表示を行う pages/内の処理を減らすことが出来る。
<br>

### public

静的ファイルを置くことが出来るフォルダ。

(ドメイン名)/images/profile2.jpg 等で画像のみにアクセスすることを可能。

GatsBy の static フォルダと同じ挙動になるので注意。
<br>

### styles

components と異なり、複数のページに対して css を適用する場合はこちらに格納する。

本プロジェクトではサイト全体に対しての修飾(global.css)と
各ページでヘッダー等のレイアウトを統一するための css(utils.modules.css)が併用されている。
<br>
<br>

---

### 以下補足

process.cwd()より、実行時のワーキングディレクトリパスを取得する事が出来る。

lib/posts.js では、path.join()を用いて実行中のディレクトリ/posts で記事が配置されているディレクトリを特定している。

fs.readdirSync により、/posts 直下のファイル名をすべて取得している。これらは dirent 型の配列で返ってくるため、map 等を用いて一つずつ別途で処理を行う。

動的ルーティングを使用するページ(本プロジェクトでは、pages/posts/[id].js)には、getStaticPaths() / getStaticProps()の両方を該当ページに実装する。これらは各 id 毎にビルド時に静的なファイルを作成し、ページコンポーネントで使用する値を予め用意しておく形になっている。

動的ルーティングページは、角かっこ内に 3 つのドットを追加することで、すべてのパスをキャッチするように拡張できる([...id].js)など

マークダウンコンテンツをレンダリングするために、remark ライブラリを使用する。

日付をフォーマットするために、date-fns ライブラリを使用する。

NextJs におけるエラーページは、pages/404.js(404 エラー時),pages/500.js(500 エラー時),pages/\_error.js(その他エラー時)に振り分ける事が可能。

ただし、Error コンポーネントを使用するページに対しては、pages/\_error.js ではなく、'next/error'より Error クラスをインポートする。
