---
title: "OSCP受験記 (I Tried Harder)"
date: 2021-01-04T20:00:00+09:00
lastmod: 2021-01-04T20:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["OSCP"]
categories: ["OSCP"]
author: ""
---
<!--
{{% right %}}
<a href="">
English
</a>
{{% /right %}}
-->

[OSCP](https://www.offensive-security.com/) (Offensive Security Certified Professional) に無事に一発合格できました。

サポートしてくれた家族と、職場の上司と、会社と、あと面識は全くないんですが間接的にお世話になった高林さんに、この場を借りてお礼を言いたいと思います。ありがとうございました。

<br />

これからOSCPを受験する方々に役に立ちそうな内容から書いていこうと思います。


<br /><br />
## PC環境

- **Macbook Air (メモリ16GB)**
<br />
もともとWindows 8の上でVirtualBoxを使ってKaliをメインに使っていたんですが、OSCPの試験も見越して新しいMacbook Airを買いました。メモリ多めで。
<br /><br />
Macにしてからは、Macをメインに、Kaliはサブで動かしました。OpenVPNとBurp SuiteだけKali上で起動して、後はMacから[iTerm2](https://iterm2.com/)を使ってKaliにSSHして使う感じです。あとは、WiresharkもKali上でたまに動かしました。

- **VirtualBox**
<br />
VMWare Fusionが推奨されていて、丁度9月に[VMWare Fusion 12のフリー版](https://my.vmware.com/web/vmware/evalcenter?p=fusion-player-personal)が出てきたので試してみたんですが、VirtualBoxでCTF用にそれなりに使い込んでいたKaliがあったしコンバートするのも面倒だったので、それをそのまま使いました。いちおうVirtualBoxでも、ラボも試験も特に問題はなかったです。
<br /><br />
ただ1点、連続クリックをした際に、マウスカーソルは動くけどクリックやタイプができなくなる現象が起きることがたまにありました。Mouse Integration絡みの問題っぽいです（これをオン/オフすると直ります）。Chromiumを使っているときに起きやすい気がします。
<br /><br />
ちなみに、Kali上のユーザもcaptureamericaを使っていて、レポートも試験もcaptureamericaユーザをそのまま使いました。

- **Emacs**
<br />
エディターはいつも使っているEmacsです。マシン攻略のメモも、レポートもEmacsを使いました。あと後述しますが、[Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat)の出力なんかも、Emacs側で色を付けて見るようにしました。

- **LibreOffice**
<br />
チートシートには、LibreOfficeを使いました。
<br />
Google Sheetも試しましたが、シート名のサイズがデカイ、という理由で使うのを止めました。（全部のシートが1画面の表示におさまらないから）

- **外部モニターｘ２**

- **スタンディング デスク**
<br />
OSCP試験も、21時間ほぼ立ちっぱなしで奮闘しました。


<br /><br />
## TIPS

- **バージョンアップの停止**
<br />
ラボを始めてから、VirtualBoxの更新を行った際にKaliが起動できなくなる問題が起きました。幸いすぐに直せたんですが（確かAudioの設定絡み）、試験が終わるまではなるべくアプリやOSの更新はしない方がいいと思います。MacのBig Surの更新も出てきましたが、試験が終わるまで放置しました。

- **RDP**
<br />
ラボマシンにRDPする場合は、以下のようにSSH Tunnelを使ってKali経由でアクセスするようにしました。Mac上でMicrosoft Remote Desktopが使用できて、ファイルのコピー等が楽になります。
<br /><br />
`$ ssh captureamerica@kali -L 13389:IPADDR:3389`
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_RDP.png" alt="OSCP_RDP.png">

- **Emacsで色付け**
<br />
ほんとはちゃんとメジャーモードを作ってやった方がいいんだろうけど、とりあえず行き当たりばったりで.emacsに定義しました。最初は[Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat)用に設定を追加してたんですが、他にもgobusterやnmapの出力にも色を付けるようにしました。
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs.png" alt="OSCP_Emacs.png">
<br /><br />
[Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat)の結果をEmacsで開くと以下のようになります。
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs_Powerless.png" alt="OSCP_Emacs_Powerless.png">
<br />
（[Metasploitable3](https://github.com/rapid7/metasploitable3)で取ったものです。OSCPのマシンではありません。）
<br /><br />
nmapの結果をEmacsで開くとこんな感じ。
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs_nmap.png" alt="OSCP_Emacs_nmap.png">
<br />
（[Metasploitable3](https://github.com/rapid7/metasploitable3)で取ったものです。OSCPのマシンではありません。）
<br /><br />
色が付いていないと大事な部分を見逃すことがあるし、目Grepにも余分な労力を費やすので、色って大事だと思います。

- **自分のIPアドレスを辞書登録**
<br />
自分のIPアドレスをコマンドで使うことは多々あるので、VPNで接続した後に辞書登録をしてすぐ出せるようにしました。
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_IME.png" alt="OSCP_IME.png">
<br />
（OSCPで使ったIPアドレスではありません。）

- **コマンドはコピペで**
<br />
ひとつのテキストファイルに、よく使うコマンドをまとめておいて、「IPADDR」「MYADDR」をEmacs、もしくはsedで書き換えて使うようにしました。
<br />
タイプするのに比べて時間も節約できるし、タイプミスでコマンドエラーになることも防げるのでオススメです。
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_ToDo.png" alt="OSCP_ToDo.png">
<br /><br />
ただ、自分でタイプしない分、コマンドはなかなか覚えられないです。



<!--
- **ツールのカスタマイズ**
<br />
[Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat)とLinEnum.shは、自分で実行したいコマンドを追加したり、コマンドの順番を入れ替えたり、自分用に使いやすいようにカスタマイズしました。
<br /><br />
あと、WebアプリでLFI、SQL Injection、Command Injectionを見つけるツールを自作して使ったりもしました。あんまり活躍しなかったけど。。
-->



<br /><br />
## レポート作成

Mark Down方式でOSCPのレポートが書ける以下のものを使いました。
<br />
https://github.com/noraj/OSCP-Exam-Report-Template-Markdown

### 利点

- Emacsで書ける。

- 軽い。（LibreOffice使って300ページ超えの文章書いたら、なんとなく重そうな気がして。。試してないからわからないけど。）

### 欠点

- PDFにしてみたときに、思った通りになってないことがある。(これは、Office系のソフトでも同じかもですが)

- Code部分の色付けが変。


### 併用したツール

- [typora](https://typora.io/)
<br />
Emacsで編集しつつ、Typoraで完成形を見ながらやりました。でも、PDFにすると結構見た目が違ってたりしました。

- [PDF Element](https://pdf.wondershare.co.jp/)
<br />
前述した欠点「Code部分の色付けが変」がどうにも気に入らなくて、これを購入しました。
<br />
とりあえず、Mark Downでは、Code部分をHTMLにしておいてなるべく色が付かないようにして、PDFとして一旦完成させた後にPDF Elementを使って色を変えました。
<br />
ところどころ、文字の追加や削除もPDF Elementを使ってやりました。
<br />
他のPDFツール（MacのPreview.app、Adobe Acrobat DC、Google DocのPlugin等）も試したのですが、少ないクリックで文字の色を変えるという点では、PDF Elementが一番優れていたので、これにしました。
<br /><br />
こんな感じです。
<br />
Codeにbashを指定してMark Downを書いた場合：
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_bash.png" alt="OSCP_bash.png">
<br /><br />
CodeにHTMLを指定して、後でPDF Elementで色の変更：
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_HTML_PDFElement.png" alt="OSCP_HTML_PDFElement.png">
<br />
([Metasploitable3](https://github.com/rapid7/metasploitable3)に対してpsexec.pyを実行したものです。OSCPのマシンではありません。)



<br /><br />
## ラボレポート

Exerciseに結構時間を取られるので、ラボレポートをやらないという手もアリかとは思いますが、個人的にはやった方がいいと思います。
<br /><br />
理由は、

- **試験に5点加算される。**
<br />
たかが5点ですが、試験が始まってからどの時点で70点を超えるかで精神的にも変わってくるので、そういう意味ではこの5点は結構大きい気がします。

- **試験レポートの練習になる。**
<br />
試験の2日目は相当疲れているし、時間の制限もあるので、レポートの作成自体にはある程度慣れておいた方がいいです。

- **Exercise自体が勉強になる。**
<br />
自分がレポートに書いたExerciseの内容を後々参照することも何度かあったし、それなりに勉強になります。（あ〜、これExerciseでやった覚えあるなぁ、みたいなやつ）

- **CPE 40点もらえる。**
<br />
これは後日わかったんですが、CPE 40点もらえました。
<br />
Submitの仕方に毎回悩むので、以下にスクリーンショット載せておきます。
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE1.png" alt="OSCP_CPE1.png">
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE2.png" alt="OSCP_CPE2.png">
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE3.png" alt="OSCP_CPE3.png">



<br /><br />
## OSCPを受ける前に持っていたスキル

- **[GPEN](https://www.giac.org/certification/penetration-tester-gpen)**
<br />
3年前に取りました。ちょうど2020年12月でExpireだったので、それの代わりにOSCPを取りたかったというのもあります。SANSのトレーニング中に取ったメモとか、時々参照しました。

- **[CISSP](https://www.isc2.org/)**
<br />
6年くらい前に取りました。2020年はCPEを貯めるために[HTB](https://www.hackthebox.eu/)をやりました。CISSPの6時間の試験もかなり長いと思ってましたが、OSCPに比べたら短いもんですね。

- **[LPIC-2](https://www.lpi.org/our-certifications/lpic-2-overview)**
<br />
Level 2まで取ってます。

- **Pentest**
<br />
いちおうセキュリティ関連のIT企業で働いてますが、Pentestは本業とはほぼ無関係です。[HTB](https://www.hackthebox.eu/)はCPE貯まった時点でやめちゃったので、半年前に40台くらいをWriteupを見ながらやっただけです。

- **プログラミング**
<br />
だいぶ前ですが、開発の仕事をしてたときにC言語を使ってました。あとCTFを1年半くらいやって来たので、そこでPythonも勉強してます。

- **英語力**
<br />
いちおう、英語圏に住んで15年目くらいです。OSCPのマニュアルもビデオもForumもちろん英語ですし、ネットで見つかる情報も英語であることが多いので、それなりに英語ができた方がプラスだと思います。

- **Windows**
<br />
Windowsは使う分には使えますが、コマンド、PowerShell、Active Directoryとかあまり知らなくて、OSCP試験日を100％の状態とすると、当初は10%くらいの知識しかなかったです。


<br /><br />
## 3ヶ月の過ごし方

自分がやった方法が必ずしもベストとは限らないので、以下、参考まで。

- **最初の1ヶ月**
<br />
後で全て見直す前提で、ひたすらラボのマシンをやっつけました。詰まったらすぐForumを見るようにしてやったので、1ヶ月でPublic（一部を除く）とDEVは終わりました。普通に楽しかったです。
<br />
もらったビデオは、傍らで繰り返し流して聞いてました。

- **2ヶ月目**
<br />
3週間ですべてのExerciseとラボ13台分をレポートに書きました。その他、最初にやったPublicとDEVの見直し、読書（「[ミミミミミッミ](https://allsafe.booth.pm/items/1861943)」、「サイバーセキュリティレッドチーム実践ガイド」←長い間本棚で寝てたやつ）、[Udemy](https://www.udemy.com/)（「Practical Ethical Hacking - The Complete Course」←無料だったときに登録してたやつ）とかやりました。
<br />
Exerciseの辺りは、ちょっとしんどかったけど、頑張りました。

- **3ヶ月目**
<br />
10日くらいでラボの残り(IT、ADMIN)はやっつけて、ここで全マシン終了。
<br />
残りの期間は、[Metasploitable3](https://github.com/rapid7/metasploitable3)や、[OSCPっぽいと言われているHTBやVulnHubのマシン](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=0)をひたすらやりました。試験日が近づいているのに、全然自力で解けなくてモヤモヤしました。
<br />
試験は月曜日だったんですが、直前の土曜日と日曜日は、消化できなかったHTBとVulnhubのWriteupだけをひらすら読みました。（結構な台数が残っていて、がっつりやってる時間はなかったので）
<br />
試験前日は、[BOF](https://github.com/freddiebarrsmith/Buffer-Overflow-Exploit-Development-Practice)も1個やりました。
<br />
<br />
ちなみに、緊張のためか、試験前日はよく眠れませんでした。。


<br /><br />
## 試験

- **Proctorセットアップ (45分)**
<br />
準備は15分で終わるはずだったのに、なんやかんややってて45分かかりました。
<br /><br />
パスポート発行国と住んでいる場所が異なる人は、別途IDの提示を求められるので、予め用意しておいた方がいいです。

- **BOF (50分、25ポイント)**
<br />
まずはBOFからやるのが定番のようで、自分もそうしました。

- **マシンA（45分、20ポイント)**
<br />
比較的、すんなりいけました。

- **マシンB (3時間、10ポイント)**
<br />
やむなくMetasploitで解きました。

- **マシンC (7時間、20ポイント)**
<br />

- **マシンD (6時間、25ポイント)**
<br />
<br />
休憩や最後の見直し等入れて全部で21時間かかりました。上記の各マシンにかけた時間は多少曖昧ですけど、時間配分はだいたいこんな感じでした。
<br /><br />
時間経過で言うと、朝7:00に開始して、70点に到達したのが18:00時点辺りです（ラボレポートの5点加算を含む）。この辺りから、Spofityをかけて歌を歌いながらやりました。
<br /><br />
朝4:00に試験を終了しました。

- **レポート**
<br />
少し寝てからレポート作成を開始。4時間くらいで書けたかな。40ページくらいです。

- **結果**
<br />
試験結果は、翌日にメールで来ました。
<br />
どこかでうっかりミス（レポートのファイル名が間違っているとか）をしてないか非常に不安になる期間なので、すぐに結果がもらえたことにはとても感謝です。


<br /><br />
## まとめ

いろんな事が勉強できたし、とても楽しくて、大変有意義な3ヶ月間でした。まだまだ知らない事はいっぱいあるし、最後のOSCP試験でもわからない事が残ったままでクリアしたし、これからも続けて勉強していかなきゃな〜と思います。

大事なのは、とにかく楽しんでやることです。自分はぶっちゃけPentesterになるわけでもないし、CTFをやっている感覚で楽しんでやるのを心がけました。3ヶ月間、NetFlixも一切見ず、ゲームもほとんどやらずに結構ストイックに勉強しましたが、そんなに苦ではなかったです。

OSCPのマニュアル、ビデオ、ラボ、試験、どれもよくできているし、コース代を払うのに十分な価値はあると思います。

これからOSCPを始めてみようという方、OSCP試験を間近に控えている方、どうぞ楽しんで頑張ってください。

TRY HARDER!!


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
