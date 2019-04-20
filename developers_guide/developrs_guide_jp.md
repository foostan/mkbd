# 開発者向けガイド(JP)
## はじめに
これは Meishi Keyboard を開発する方向けのガイドです。
チュートリアル形式で `mkbd/pcb` のような "名刺型" のキーボードを作る過程を説明します。

## PCB 設計
### 新規プロジェクトの開始

KiCad を起動し、「ファイル > 新規 > プロジェクト」から新規プロジェクトの作成します。

![new_project](./images/new_project/new_project.png)

プロジェクト名を決めて [Save] します。 

![save_new_project](./images/new_project/save_new_project.png)

KiCad プロジェクトのメインメニュー画面が表示されます。
各種ツールはこの画面から起動するので時間があればどのようなものがあるか確認しておきましょう。
今は時間がないので先に進みます。

![global_menu](./images/new_project/global_menu.png)

### 回路設計

まずはキーボードの回路を設計していきます。
グローバルメニューから、「回路図レイアウト エディター」を開きます。

![open_eeschema](./images/eeschema/open_eeschema.png)


これが回路図レイアウトエディター(以降 Eeschema)の作業画面です。

![start_eeschema](./images/eeschema/start_eeschema.png)

今は設計に必要な最低限な説明しかしないのでもし体系的に学びたい場合は「ヘルプ > KiCad ことはじめ」を開いて見てください。
KiCad の使い方を細かく学ぶことができるのでおすすめです。

![kotohajime](./images/eeschema/kotohajime.png)

設計を始める前に、まずはキーボードに必要なパーツ(ライブラリ)を手に入れます。
以下のように必要なライブラリを GitHub 上から取得します。

```
$ git clone https://github.com/foostan/kbd
```

もしくは https://github.com/foostan/kbd にアクセスしてダウンロードしてください。

次に取得したライブラリを「設定 > シンボル ライブラリーを管理...」 から読み込みます。

![manage_lib](./images/eeschema/manage_lib.png)

[ライブラリーを参照...] から `kbd/library/kbd.lib` を [Open] します。

![open_lib](./images/eeschema/open_lib.png)

これで回路を設計する準備が整いました。
Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。
検索フォームに「promicro_r」と入力して、「ProMicro_r」を選択し [OK] します。

![add_promicro_r](./images/eeschema/add_promicro_r.png)

適当な配置します。

![set_promicro_r](./images/eeschema/set_promicro_r.png)

もし手元に実物の「ProMicro」があれば見比べてみましょう。
部品が実装されていない面の向きになっていることが確認できると思います。
回路設計においては実物と形や向きを揃える必要はありませんが、初学者にとっては実物が想像できたほうが理解が捗るので、
もし外部のライブラリーを使う場合は見てわかりやすいものを選ぶのがいいかもしれしれません。

![promicro](./images/eeschema/promicro.png)

次にキースイッチを配置していきます。
今回は6つのキーが付いたキーボードを設計してみます。

Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。

検索フォームに「sw_push」と入力して、「SW_PUSH」を選択し [OK] します。

![add_sw_push](./images/eeschema/add_sw_push.png)

わかりやすいように整列させて並べます。
シンボルを移動させる場合は、シンボルにカーソルを合わせてショートカット `m` を押して移動させたい場所に動かします。 

![set_sw_push](./images/eeschema/set_sw_push.png)

次にキースイッチをProMicroにつなげていきます。
ショートカット `w` を押して下記画像を参考にしながらつなげましょう。
もしもっとスッキリと配線させたい場合は「ラベル」を使ったやり方もあるので「KiCad ことはじめ」等でやり方を探ってみてください。

![wiring_sw_push](./images/eeschema/wiring_sw_push.png)

次にキースイッチのもう片方の足を GND に落とします。
Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。
検索フォームに「gnd」と入力して、「GND」を選択し [OK] します。

![add_gnd](./images/eeschema/add_gnd.png)

下記画像を参考に GND を付けていきます。
見栄えが気になった方は、ショートカット `g` や ショートカット `r` などで調整してみてください(各ショートカットの挙動は実際に確認してみてください)。

![set_gnd](./images/eeschema/set_gnd.png)

次にリセットスイッチを付けていきます。
Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。
検索フォームに「sw_push」と入力して、「SW_Push」を選択し [OK] します。
キースイッチと同じものでも問題ありませんが、見た目が違うものの方がわかりやすいのであえて変えました。

![add_sw_push_reset](./images/eeschema/add_sw_push_reset.png)

キースイッチと同じ方法で配線および GND を付けます。

![set_sw_push_reset](./images/eeschema/set_sw_push_reset.png)

ProMicro_r に VCC および GND を付けます。
Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。
検索フォームに「vcc」と入力して、「VCC」を選択し [OK] します。

![set_vcc_and_gnd](./images/eeschema/set_vcc_and_gnd.png)

必要なパーツをすべて配線したので、ProMicro_r の使わないポートに「未接続フラグ」を付けていきます。


![set_quit_flag](./images/eeschema/set_quit_flag.png)


最後に VCC と GND に PWR_FLAG を接続します。
Eeschema の作業画面でショートカット `a` を押してシンボルの選択ウインドウを開きます。
検索フォームに「pwr_flag」と入力して、「PWR_FLAG」を選択し [OK] します。

![set_pwr_flag](./images/eeschema/set_pwr_flag.png)

次に、設置したシンボルに番号を付けていきます。
上記メニューから「回路図シンボルをアノテーション」を選択し

![annotation](./images/eeschema/annotation.png)

設定はそのままで [アノテーション] します。

![set_annotation](./images/eeschema/set_annotation.png)

次に、設計した回路に不備がないかチェックします。
上記メニューから「エレクトリカル ルールのチェックを実行」を選択し

![erc](./images/eeschema/erc.png)

[実行] します。「メッセージ」に何も警告が表示されず、「終了」とだけ表示されればOKです。

![run_erc](./images/eeschema/run_erc.png)

次に、シンボルと実際の基板の部品(フットプリント)との紐付けを行います。
上記メニューから「回路図シンボルへPCBフットプリントを関連付ける」を選択し

![run_erc](./images/eeschema/relate_to_footprint.png)

「フットプリント ライブラリー テーブルを編集」を選択し、

![run_erc](./images/eeschema/footprint_lib.png)

[ライブラリーを参照...] から `kbd/kbd.pretty` を選択し [OK] します。

![run_erc](./images/eeschema/select_kbd_pretty.png)


これでキーボードに関するフットプリントが使えるようになりました。

![start_relation](./images/eeschema/start_relation.png)

読み込んだ kbd からそれぞれ下記のように割り当てを行います。
今回の例ではSW1がリセットスイッチになっていることに注意してください。
事前にわかりやすいようにアノテーションを「RSW」などに変更しておくとわかりやすいかもしれません
(アノテーションの変更はシンボルにカーソルを合わせて、ショートカット `e` でできます)。

![finish_relate](./images/eeschema/finish_relate.png)

またフットプリントを確認したい場合は、右側の枠からフットプリントを選択して「選択したフットプリントを見る」から見れます。

![show_footprint](./images/eeschema/show_footprint.png)
![show_selected_footprint](./images/eeschema/show_selected_footprint.png)


最後に上記メニューから「ネットリストを生成」を選択して、
![output_netlist](./images/eeschema/output_netlist.png)

[ネットリストを生成] からネットリストと呼ばれる回路情報をファイルとして出力します。
![output_netlist_to_file](./images/eeschema/output_netlist_to_file.png)

回路設計は以上で完了です。


### PCBの作成

グローバルメニューまで戻り、「基板レイアウト エディター」を選択します。
![open_pcbnew](./images/pcbnew/open_pcbnew.png)

これが基板レイアウト エディター(以降 Pcbnew) の作業画面です。
![pcbnew](./images/pcbnew/pcbnew.png)

まずはここに先程作成した回路データを読み込みます。
上記メニューの「ネットリストの読み込み」を選択し
![open_netlist](./images/pcbnew/open_netlist.png)

[現在のネットリストを読み込む]を選択します。
![load_netlist](./images/pcbnew/load_netlist.png)

下記のように各フットプリントが読み込まれていればOKです。
![loaded_netlist](./images/pcbnew/loaded_netlist.png)

まずは名刺の枠を作っていきます。
ちなみに名刺のサイズは 91mm x 55mm です。

上記メニューから作業するレイヤーを 「Edge.Cuts」 に変更し、
![select_edge_cut](./images/pcbnew/select_edge_cut.png)

右メニューから「図形ラインを追加」を選択します。
![add_line](./images/pcbnew/add_line.png)

まずは大まかに四角形を作ります。
![draw_lines](./images/pcbnew/draw_lines.png)


次に線にカーソルを合わせてショートカット `e` で「配線セグメントのプロパティ」を表示します。
この始点 X、始点 Y、終点 X、終点 Yで線の位置を決定していきます。``
![edit_xy](./images/pcbnew/edit_xy.png)

4本線があると思うのでそれぞれ、

1本目
- 始点 X: 0
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 0

2本目
- 始点 X: 91
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 55

3本目
- 始点 X: 91
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 55

4本目
- 始点 X: 0
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 0

を指定します。

![edge_cut](./images/pcbnew/edge_cut.png)

これだと角が尖っていて痛いので丸めます。

まずは作業がしやすいように、グリッドの設定をします。
0.1mm でいいと思います。
![select_0.1mm](./images/pcbnew/select_0.1mm.png)

「次に円弧を追加」を選択して角を丸めます。

![add_enko](./images/pcbnew/add_enko.png)

![set_enko](./images/pcbnew/set_enko.png)

次にキースイッチのフットプリントを並べていきます。
キースイッチのピッチはCherryMX系の場合は一般的には19.05mmです。

まずはカスタムのグリッド設定を行うため、「表示  > グリッド設定」を選び、サイズ X およびサイズ Y を 1.905 にします。

![grid_setting](./images/pcbnew/grid_setting.png)

その後上記メニューのグリッド設定を「カスタム ユーザー グリッド」にすると、19.05mm 単位のグリッドが表示されます。
あとはこのグリッドに沿ってキースイッチを並べます。
なお、フットプリントの移動はフットプリントにカーソルを合わせて、ショートカットキー `m` でできます。

![set_key_sw](./images/pcbnew/set_key_sw.png)


次にグリッド設定を0.1mm等に戻して、下記のようにProMicroとリセットスイッチを配置します。
基本的には好みで構わないのですが、ProMicroについてはUSB側が外形に重なるように配置します
(もっと内側に設置してしまうと、USBケーブルが干渉してさせなくなります)。

![set_promicro_and_reset](./images/pcbnew/set_promicro_and_reset.png)

次に配線をしていきます。ラッツネスト(ホールやパッドをつなぐ細長い白い線)が、配線すべき箇所を表しています。
きれいに配線するにはこのラッツネストを参考にして、どう配線していくか計画をたてるとうまくいくと思います。

配線はまず、上記メニューから 「F.Cu」レイヤに切り替えて 
![select_fcu](./images/pcbnew/select_fcu.png)

右メニューの「配線」から行います。

![line](./images/pcbnew/line.png)


なお GND は別途処理をするのでそれ以外の配線を行います。
配線にカーソルを合わせて、ショートカットキー `d` や、
フットプリントにカーソルを合わせて、ショートカットキー `r` などの操作を行い、
なるべくシンプルな配線になるように心がけます。

配線の例はこちらです。

![finished_line](./images/pcbnew/finished_line.png)

次に GND の処理をします。右メニューから「塗りつぶしゾーンを追加」を選択し、外形を覆うように配置します。

![beta](./images/pcbnew/beta.png)

設定は以下のとおりです。レイヤーを 「F.Cu」にし、ネットを「GND」にしてあとはデフォルトのままです。

![set_zone](./images/pcbnew/set_zone.png)

正しくゾーンが設置できると以下のようになります。
未接続だった GND が全てつながっているはずです。

![finished_zone](./images/pcbnew/finished_zone.png)

今回の場合ゾーンは F.Cu レイヤだけで動作的には問題ありませんが、
裏表の仕上がりに差が出てしまうので B.Cu レイヤにもゾーンを配置します。

![finished_line_and_zone](./images/pcbnew/finished_line_and_zone.png)

次に「名刺」として機能するようにシルクで名前を入れていきます。
好みのドローソフトやペイントソフトを使用して名刺画像を作成します。

今回は PhotoShop を使います。
幅 91mm x 高さ 55mm で 300 ppi で新規作成します。 

![new_meishi](./images/pcbnew/new_meishi.png)

今回作成したものはこちらです。
背景を黒にして2色で作成します。

![mkbd](./images/pcbnew/mkbd.png)

一旦グローバルメニューにもどり「インポート ビットマップ」を選択します。

![import_bitmap](./images/pcbnew/import_bitmap.png)

ここで先ほど作成した画像を読み込みます。
設定はデフォルトのままで大丈夫だと思いますが、一応サイズと解像度は確認しておきましょう。

![import_meishi](./images/pcbnew/import_meishi.png)


[エクスポート] で Pcbnew で読み込めるシルクデータとして保存します。
なお注意としてはかならず「*.pretty」というフォルダを新規に作成してその中に保存してください。

今回の場合は「mkbd.pretty/meishi.kicad_mod」として保存しています。

![export](./images/pcbnew/export.png)

Pcbnewの画面に戻り、「設定 > フットプリント ライブラリーを管理」を選択します。
[ライブラリーを参照] から先程作成した「mkbd.pretty」を選択します。

![load_lib](./images/pcbnew/load_lib.png)

これで作成した画像がシルクとして使えるようになりました。
「配置 > フットプリント」から配置してみます。
下記のように選択できるようになっているはずです。

![set_meishi_footprint](./images/pcbnew/set_meishi_footprint.png)

あとは好みに配置して完成です。
今回は部品を実装しない裏側に設定しました。
なおフットプリントを裏返す場合は、フットプリントにカーソルを合わせてショートカット `e` から編集ウインドウを開き、
「基板側::表側」となっているのを「裏側」に変更します。

![finished](./images/pcbnew/finished.png)

以上で完成です。
最後にミスがないかチェックします。

上記メニューの「デザインルール チェックを実行」から

![drc](./images/pcbnew/drc.png)

[DRCの開始] を押してチェックします。問題や未配線がなければOKです。

![run_drc](./images/pcbnew/run_drc.png)

完成したPCBを「表示 > 3Dビューアー」から確認してみます。

![3d](./images/pcbnew/3d.png)

![3d_back](./images/pcbnew/3d_back.png)

### PCBの発注

完成したPCBのデータを業者へ発注する過程を説明します。

「ファイル > プロット」から製造用ファイルを生成します。

「含まれるレイヤー」にて、

- F.Cu
- B.Cu
- F.SilkS
- B.SilkS
- F.Mask
- B.Mask
- Edge.Cuts

を選択します。
あとはデフォルトのままです。
出力ディレクトリーをしていして [製造ファイル出力] をします。

![plot](./images/order/plot.png)

また、ドリルデータも必要なので、[ドリル ファイルを生成...] から

![drill](./images/order/drill.png)

[ドリル ファイルを生成] をします。
また生成した製造ファイル(ガーバーデータ)は zip 等で圧縮しておきます。


今回発注には Seeed の FusionPCB を利用します。
https://www.fusionpcb.jp/prototype-pcb-sale.html

こちらではオプションの制約はありますが、100mm x 100mm 以下のPCBの発注を $7.9 で行えるプランが有り、
またOCSを選択しても $12.90 と格安です(少し前までは更に安くて$10を切っていた)。

なお同じ構成で Elecrow に頼むと $18.13 でした。

![fusionpcb](./images/order/fusionpcb.png)

あとは [カートに追加] し、注文すれば完了です。
