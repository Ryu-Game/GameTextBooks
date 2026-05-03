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
  
  ### ボタンの追加
 1. Hierarchy画面で右クリックしてUIのPanelを選択
 2. Panelを選択後、右クリックしてUIのButtonを2つ追加
    - 名前を`Start`と`Exit`
 3. Buttonに格納されているTextをそれぞれの名前に変更
 <img src="./images/画面遷移.png" width="75%">
 4. Game画面を見ながらInspector画面で位置と大きさを調整
 
  ### タイトルの追加
 1. Assetsファイルの中に新規フォルダーを作成
    - Project画面で右クリックしてCreateの`Folder`を選択
    - 名前を`Fonts`
 2. タイトルを日本語にするために下記URLからフォントをダウンロード
  ※英数字のみで大丈夫の場合スキップ
  [フォントフリー様](https://fontfree.me/)
 1. ダウンロードしたファイルの中から`.ttf`ファイルをFontsフォルダーにドラッグ＆ドロップ
 2. 画面のメニューバーからWindowsのTextMeshProの`Font Asset Creator`を選択
 3. 下記のURLから文字をコピーするために`Raw`を選択後、全コピー
    [日本語文字](https://gist.github.com/kgsi/ed2f1c5696a2211c1fd1e1e198c96ee4)
    <img src="./images/日本語.png" width="75%">
 4. 下記の画像通りに設定
 <img src = "./images/フォント設定.png" width="50%">
    | 設定項目 | 設定値 |
    | --- | --- |
    | Source Font File | ダウンロードした.ttf |
    | Sampling Point Size | AutoSizing |
    | Padding | 5 |
    | Paccking Method | Fast |
    | Atlas Resolution | 4096 * 4096 |
    | Chareacter Set | Custom Charactors |
    |Charactor List | コピーした文字を張り付け |
    | RenderMode | SDFAA |
 5. 設定が完了したら`Generate Font Atlas`を選択して文字を生成
    ※生成後、文字が読み取れない場合、Atlas Resolutionの値が小さいので調整
 6. 問題がなく生成されたら`Save As`を選択
 7. 保存場所はプロジェクトのFontsフォルダーに保存
    - `プロジェクト場所/Assets/Fonts`
 8. Hierarchy画面のButtonを生成したPanelを選択してTextMeshProを作成
 9. Inspector画面でタイトルとフォントを下記のように設定
    <img src="./images/タイトル.png" width="50%">
    - TextInput画面でタイトルを入力
    - Font Assetで生成したFontを選択
 10. Game画面を見ながらInspector画面で位置と大きさを調整

 ### コード追加
  下記の場所にScriptを追加
  - Assets→Script
  - 名前は`ButtonScript`

 ### コーディング
 ---
 #### ButtonScript.cs
 <details open><summary>ButtonScript.cs</summary>
 
 ```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class ButtonScript : MonoBehaviour
{
    public void QuitGame()
　{
#if UNITY_EDITOR
        // Unityエディターでの動作
        UnityEditor.EditorApplication.isPlaying = false;
#else
        // 実際のゲーム終了処理
        Application.Quit();
#endif
    }
    public void StartGame()
    {
        SceneManager.LoadScene("Playground");
    }

    public void TitleBack()
    {
        SceneManager.LoadScene("Title");
    }
}
 ```
 </details>

 ### コード説明
 #### ButtonScript.cs
 ```cs
 public void TitleBack()
 {
   SceneManager.LoadScene("Title");
 }
 ```
 TitleBackを呼び出したらTitleシーンに画面遷移
   - SceneManager.LoadScene(シーン名);

 ### タイトル画面
 1. SceneをTitle
 2. Hierarchy画面で右クリックしてCreateEmptyを選択
    - 名前を`GameManager`
 3. GameManagerにButtonScriptを追加
 4. ボタンのStartとExitを下記のようにInspector画面で設定
  <img src="./images/画面遷移1.png" width="75%">
    ①GameManagerをドラッグ＆ドロップでアタッチ
    ②Editor And Runtimeを選択
    ③ButtonScriptの`StartGame`を選択
    ※Exitは`QuitGame`を選択

  ### 完成
 完了したら実行した際、下記のことを確認
  - タイトル描画
  - Exitボタン：強制終了
  - StartGameボタン：ゲーム画面

---
[次へ](./2Project.html)

[前へ戻る](./5Timer.html)

[Unity開発ページへ戻る](../index.html)