craftinginterpreters.com

[* インタプレタを作れるとどうなるか。]

データ構造とアルゴリズムがしっかりとわかるようになる。
> Implementing a language is a real test of programming skill. The code is complex and performance critical. You must master recursion, dynamic arrays, trees, graphs, and hash tables. You probably use hash tables at least in your day-to-day programming, but do you really understand them? Well, after we’ve crafted our own from scratch, I guarantee you will.
> While I intend to show you that an interpreter isn’t as daunting as you might believe, implementing one well is still a challenge. Rise to it, and you’ll come away a stronger programmer, and smarter about how you use data structures and algorithms in your day job.

プログラミング言語はカーテンで覆われているけど、インタプリタ/コンパイラを学んでそのカーテンを開けたら、それはシンプルな仕組みだったことが分かる。
>This last reason is hard for me to admit, because it’s so close to my heart. Ever since I learned to program as a kid, I felt there was something magical about languages. When I first tapped out BASIC programs one key at a time I couldn’t conceive how BASIC itself was made.
> Later, the mixture of awe and terror on my college friends’ faces when talking about their compilers class was enough to convince me language hackers were a different breed of human—some sort of wizards granted privileged access to arcane arts.
> It’s a charming image, but it has a darker side. I didn’t feel like a wizard, so I was left thinking I lacked some inborn quality necessary to join the cabal. Though I’ve been fascinated by languages ever since I doodled made-up keywords in my school notebook, it took me decades to muster the courage to try to really learn them. That “magical” quality, that sense of exclusivity, excluded me.
> When I did finally start cobbling together my own little interpreters, I quickly learned that, of course, there is no magic at all. It’s just code, and the people who hack on languages are just people.
> There are a few techniques you don’t often encounter outside of languages, and some parts are a little difficult. But not more difficult than other obstacles you’ve overcome. My hope is that if you’ve felt intimidated by languages and this book helps you overcome that fear, maybe I’ll leave you just a tiny bit braver than you were before.

🗣️これは自分も同じことを思っていたかも。作れるけど、どうなっているのかよく分からない感じ。

コンパイラ: 一度に全部変換. エラー箇所が分かりづらい
インタプリタ: 一行ずつ読んでいく。
↑まだいまいちわかっていない。


# [* 全体像]

このソースコードがどうなっていくのか見ていく。
code:javascript
 var average = (min + max) / 2

- [* Scanning]: 最小単位に分解
空白やコメントを無視して、var, average,=,(,min,+,max~と各wordごとに切り取っていく。wordはtoken(プログラムの最小単位)とも呼ばれる。

- [* Parsing]: 分解したものを木構造を使って理解
トークンを受け取った後に、それらを木構造に変換していく。(syntax trees, ASTs,シンプルにtreeとも呼ぶ)
この技術は、AIの研究者が私たち人間の言葉をAIに理解させるために考案したシステムが現在の多くのプログラミング言語のParsingの仕組みとして使われている。

人間の言葉は複雑で、文脈によって意味が変わるが、プログラミング言語の場合はこの仕組みがぴったり合う。少しでも文脈が異なるとSyntax errorを吐くようになっている。

[* Static Analysis]:(binding: そのtokenがどんなものなのか調査する)
a + bは、コンピューター側でどのように定義されてabになっているのでしょうか?
binding(resolution): 各tokenはどの変数なのか、どこに保存したらいいのか、グローバルか、ローカルか、関数かclassのメンバか、型は合っているか、そのtokenのscopeを確認したりしてそれぞれを解析していく

[* Intermediate Representation]:
そもそもこれらの各ステージは、次の処理をしやすくするためにデータの構造を整理して渡していくことです。
フロントエンド: 開発者が書いている言語のソースコード
"intermediate": どちらの言語にも定義されていないデータの中間的な表現(control flow graph) 
バックエンド: プログラムが実行される最終的なアーキテクチャ

中間的な表現を用意しておくことで、複数の言語やOSを少ない労力で管理することができる。

つまりここでやっているのは、今までのトークンにしたり木構造にしたり、そのトークンがどういうものなのか調査したりした後に、ここでは一旦中間的な表現で保存することで最初にも書いたように、次の処理をしやすくするためのデータ構造に変えておく。

[* optimization:]

