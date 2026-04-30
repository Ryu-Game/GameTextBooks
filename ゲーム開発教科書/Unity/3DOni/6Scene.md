# ゴースト鬼ごっこ
初心者向けで1～2時間想定の簡単な鬼ごっこゲームの作り方をご紹介します。

## 目次
1. [初めに](./1index.html)
2. [プロジェクト作成](./2Project.html)
3. [鬼の追加](./3Ghost.html)
4. [Player残機追加](./4Lives.html)
5. [ゲーム時間追加](./5Timer.html)
6. [画面遷移](./6Scene.html)
---
## 画面遷移
 ### ゲームシーンを追加
 1. 下記の場所に保存されている`Playgroundシーン`をコピー＆ペースト
    - Assets→Scenes
    - 名前を`Title`
 2. 作成したシーンに移動
 3. 下記のモノをHierarchy画面から削除
    - MainCamera
    - PlayerArmature
    - GameManager
    - Canvas
 4. Hierarchy画面で右クリックしてUIのPanelを選択
 5. Panelを選択後、右クリックしてUIのButtonを2つ追加
    - 名前を`Start`と`Exit`
 6. Panelを選択後、右クリックしてUIのText - TextMeshProを選択
    - タイトルとして使用
 7. Buttonに格納されているTextをそれぞれの名前に変更
 <img src="./images/画面遷移.png" width="75%">
 1. Game画面を見ながらInspector画面で位置と大きさを調整
 2. タイトルを日本語にするために書きURLからダウンロード
  ※英数字のみで大丈夫の場合スキップ
  [フォントフリー様](https://fontfree.me/)



---
[次へ](./2Project.html)

[前へ戻る](./5Timer.html)

[Unity開発ページへ戻る](../index.html)