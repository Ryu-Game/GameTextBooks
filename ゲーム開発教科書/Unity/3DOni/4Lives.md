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

## Player残機追加
 ### コード追加
 下記の場所にScriptを追加
  - Assets→Script
  - 名前は`PlayerScript`
 
 ### コーディング
---
 #### PlayerScript.cs
 <details open><summary>PlayerScript.cs</summary>

 ```cs
 using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class PlayerScript : MonoBehaviour
{
    public int Life;
    [SerializeField] public TextMeshProUGUI LifeText;
    // Start is called before the first frame update
    void Start()
    {
        Life = 3;
    }
    // Update is called once per frame
    void Update(){
        LifeText.text = "Life: " + Life;

        if(Life < 0){
            //ゲームオーバー処理
            QuitGame();
        }
    }

    private void OnControllerColliderHit(ControllerColliderHit hit){
        if(hit.gameObject.tag == "Enemy"){
            Life--;
            Destroy(hit.gameObject);
        }
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

 ※確認のために`QuitGame`強制終了を作成

 ### コード説明
 ```cs
 public int Life;
 [SerializeField] public TextMeshProUGUI LifeText;
 // Start is called before the first frame update
 void Start()
 {
    Life = 3;
 }
 ```
 - 変数`Life`を作りPlayerの残機を設定
 - TextMeshProUGUIでゲーム画面上にTextを描画

 ```cs
 void Update(){
    LifeText.text = "Life: " + Life;

    if(Life < 0){
        //ゲームオーバー処理
        QuitGame();
    }
}
 ```
 - LifeText.textで現在のPlayer残機を描画させる
 - Player残機が0以下になるとゲームオーバー処理

 ```cs
 private void OnControllerColliderHit(ControllerColliderHit hit){
    if(hit.gameObject.tag == "Enemy"){
        Life--;
        Destroy(hit.gameObject);
    }
}
 ```
 - OnControllerColliderHit関数でPlayerが何と衝突しているか検知
  ※オブジェクトにColliderが設定されていない場合検知できない
 - Playerがタグ名`Enemy`と衝突したらPlayer残機を減らして、衝突したEnemyオブジェクトを削除

 ### Player残機数UI



