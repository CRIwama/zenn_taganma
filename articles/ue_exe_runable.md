---
title: "エージェントさんのGPUがなさそうなノートPCでUnreal Engineの実行ファイル（.exe）は動くのか"
emoji: "😸"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["ue5","unrealengine"]
publication_name: "coyote_tateam"
published: true
---

## 経緯

そろそろ1ステージに必要なゲームの要素が仕上がってきました。
エージェントさんにもデバッグに参加してもらえないかサードパーソンテンプレートのプロジェクトをパッケージ化して実行ファイル（.exe）を実行してもらいました。
無事UnrealEngineで作成したexeが動きました。
その時に確認してもらった内容をまとめます。

## GPUが入ってるのか確認する方法

GPUのありなしは「タスクマネージャー」で確認してもらいました。

1. タスクマネージャーを開く（Ctrl + Shift + ESCキー）
2. 「詳細」をクリック
3. 「パフォーマンス」タブをクリック
4. タスクマネージャーをスクリーンショット（Print Screenキー）で撮影する

![](/images/articles/ue_exe_runable/2024-09-11-15-57-59.png)

エージェントさんから送ってもらったスクリーンショットで「GPU」があることが確認できました。
調べてみたらCPUの中にGPUが内蔵されている「[インテル® Iris® Xe グラフィックス](https://www.intel.co.jp/content/www/jp/ja/products/docs/discrete-gpus/iris-xe/integrated-graphics/overview.html)」でした。
一見グラフィックボードが入っていなくてもGPUが入っているんですね。

![](/images/articles/ue_exe_runable/2024-09-11-15-55-02.png)

実行ファイル（.exe）の初回起動時には「UE Prequisitess (x64) Setup」というウィンドウが立ち上がると教えてもらいました。
「Close」をクリックし、再度実行ファイル（.exe）を起動するとパッケージ化したゲームが起動しました。

![](/images/articles/ue_exe_runable/2024-09-11-16-17-57.png)

Windows11ではタスクマネージャーは最初から詳細表示になっています。
「パフォーマンス」タブをクリックするとGPUの存在を確認できます。

![](/images/articles/ue_exe_runable/2024-09-11-16-00-15.png)

## 実行ファイル（.exe）を渡すときの工夫「終了処理は入れておこう」

Unreal Engineのプロジェクトをパッケージ化した際に終了する処理が入っていません。
終了処理を入れていない場合は「Alt + F4」を入力するとアプリケーションが終了します。（Windows共通）
普段「Alt + F4」でアプリケーションを終了させることをしない人に実行ファイル（.exe）を渡すときには終了処理を入れておいた方がいいです。

https://azby.fmworld.net/lesson/shortcuts-list/010/?from=shortcuts-list-ranking_right-area#:~:text=%E4%BD%BF%E7%94%A8%E4%B8%AD%E3%81%AE%E3%82%A2%E3%83%97%E3%83%AA%E3%82%84,Alt%20%E3%81%A8%20F4%20%E3%82%92%E6%8A%BC%E3%81%99%E3%80%82

**エディターと同じように「ESC」キーでゲーム終了する処理を紹介します。**

一番簡単に実装できるのは「レベルブループリント」に実装します。
レベルブループリントを開きます。

![](/images/articles/ue_exe_runable/2024-09-11-16-28-57.png)

「ESC」キーのキー入力イベントの「Escape」ノードを追加します。

![](/images/articles/ue_exe_runable/2024-09-11-16-32-07.png)

「Quit Game」ノードでゲームを終了できます。
「Escape」ノードの「Pressed」ピンから「Quit Game」ノードの実行ピンに接続します。

![](/images/articles/ue_exe_runable/2024-09-11-16-33-50.png)

これで実行ファイル化したときに「ESC」キーを入力すればゲームが終了するようになります。
レベルブループリントに処理を書かない方がメンテナンスがしやすいのですが、簡単に実装した内容を確認するときには便利です。

開発者が一般常識だと考えていることは、一般常識ではないことが多いです。
何かこうした方がよかったということは一般常識の差分で起きます。
メモしておくと役に立つから技術記事に残しておきましょう！

