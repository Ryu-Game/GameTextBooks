# ゴースト鬼ごっこ
初心者向けで1～2時間想定の簡単な鬼ごっこゲームの作り方をご紹介します。

## 目次
1. [初めに](./1index.html)
2. [プロジェクト作成](./2Project.html)
3. [鬼の追加](./3Ghost.html)
4. [Player残機追加](./4Lives.html)
5. [ゲーム画面作成](./5Game.html)
6. [最後に](./6Final.html)
---

## ゲーム時間追加
 ### コード追加
 下記の場所にScriptを追加
  - Assets→Script
  - 名前は`Timer`
 
 ### コーディング
---
 #### Timer.cs
 <details open><summary>Timer.cs</summary>
 
 ```cs
 using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class Timer : MonoBehaviour
{
    float limitTime = 60;
    
　// Update is called once per frame
    void Update()
    {
        limitTime -= Time.deltaTime;
        if(limitTime < 0)
        {
            limitTime = 0;
　　　//ゲームクリア処理
            QuitGame();
        }

        TimerText.text = "Time: " + limitTime;
    }

    //強制終了
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
}
 ```
 </details>

 ### コード説明
 ```cs
 float limitTime = 60;
 ```
 変数`limitTime`を作りゲーム時間を作成
  - 今回は60秒だが値を変えることで変更可能

 ```cs
 limitTime -= Time.deltaTime;
if(limitTime < 0)
    {
        limitTime = 0;
　　    //ゲームクリア処理
        QuitGame();
    }

 ```
 - `Time.deltaTime`で毎フレームゲーム時間減らす
 - ゲーム時間が0秒以下になったらゲームクリア処理

 ```cs 
 TimerText.text = "Time: " + limitTime;
 ```
 残りゲーム時間をゲーム画面上に描画

 ### ゲーム時間UI
 ゲーム画面上にPlayerの残機数をTextで描画
  1. Hierarchy画面で右クリックしてUIの`Panel`を選択
  2. 生成後、Inspector画面でImageのColorの透明度を0に設定
  3. 生成したPanelを選択して右クリックUIの`Text - TextMeshPro`を選択
   ※TextMeshProダウンロード画面が出たら
     - 名前を`Timer`に設定
  1. Game画面を見ながらInspector画面で位置と大きさを調整
   ※文字の大きさは`FontSize`で調整
  1. Hierarchy画面で何も選択しない状態で右クリックして`CreateEmpty`を選択
     - 名前を`GameManager`に設定
  2. GameManagerにTimerScriptを追加
  3. 追加したTimerScriptに3で作成したTextをTimer Textにドラッグ＆ドロップ
   ※やり方は[Player残機数UIのやり方](./4Lives.html)に記載

 ### 実行
 完了したらゲーム時間が進んでいるのか確認

---
[次へ](./2Project.html)

[前へ戻る](./4Lives.html)

[Unity開発ページへ戻る](../index.html)