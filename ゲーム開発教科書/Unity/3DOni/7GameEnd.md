# ゴースト鬼ごっこ
初心者向けで1～2時間想定の簡単な鬼ごっこゲームの作り方をご紹介します。

## 目次
1. [初めに](./1index.html)
2. [プロジェクト作成](./2Project.html)
3. [鬼の追加](./3Ghost.html)
4. [Player残機追加](./4Lives.html)
5. [ゲーム時間追加](./5Timer.html)
6. [画面遷移](./6Scene.html)
7. [ゲーム勝敗処理](./7GameEnd.html)
8. [最後に](./8Final.html)
---
## ゲーム勝敗処理
### ゲーム画面
 1. `Playground`画面に遷移
 2. GameManagerにButtonScriptを追加
 3. 新しくUIを作成
  <img src="./images/EndPanel.png" width="40%">
  - ゲームクリア
     | 作成オブジェクト | 名前 |
     | --- | --- |
     | Canvas | ClearCanvas |
     | Panel | ClearPanel |
     | Text - TextMeshPro | ClearText|
     | Button | TitleButton |
  - ゲームオーバー
     | 作成オブジェクト | 名前 |
     | --- | --- |
     | Canvas | OverCanvas |
     | Panel | OverPanel |
     | Text - TextMeshPro | OverText|
     | Text - TextMeshPro | TimeText|
     | Button | TitleButton |
 4. TitleButtonを下記の画像のように設定
  <img src="./images/TitleBt.png" width="75%">
 5. 下記のScriptを更新
     ### コーディング
     <details open><summary>Timer.cs </summary>

     ```cs
      using System.Collections;
      using System.Collections.Generic;
      using TMPro;
      using UnityEngine;
      
      public class Timer : MonoBehaviour
      {
          [SerializeField] public TextMeshProUGUI TimerText;
          float limitTime = 60;
          [SerializeField]
          public TextMeshProUGUI OverCount = null;
          [SerializeField]
          public Canvas gameClearCanvas = null;
      
          void Start()
          {
              gameClearCanvas.gameObject.SetActive(false);
              Time.timeScale = 1.0f;
          }
      
          // Update is called once per frame
          void Update()
          {
              limitTime -= Time.deltaTime;
              if(limitTime < 0)
              {
                  limitTime = 0;
                  //ゲームクリア処理
                  gameClearCanvas.gameObject.SetActive(true);
                  Time.timeScale = 0; //ゲーム時間停止
              }
      
              TimerText.text = "Time: " + limitTime;
              OverCount.text = "残りタイム\n" + limitTime;
          }
      }
     ```
       </details>
     <details open><summary>PlayerScript.cs </summary>
     
     ```cs
      using System.Collections;
      using System.Collections.Generic;
      using TMPro;
      using UnityEngine;
      
      public class PlayerScript : MonoBehaviour
      {
          public int Life;
          [SerializeField] public TextMeshProUGUI LifeText;
      
          [SerializeField]
          public Canvas gameOverCanvas = null;
          // Start is called before the first frame update
          void Start()
          {
              Life = 3;
              gameOverCanvas.gameObject.SetActive(false);
          }
      
          // Update is called once per frame
          void Update()
          {
              LifeText.text = "Life: " + Life;
      
              if(Life < 0)
              {
                  //ゲームオーバー処理
                  Time.timeScale = 0; //ゲーム時間停止
                  gameOverCanvas.gameObject.SetActive(true);
              }
          }
      
          private void OnControllerColliderHit(ControllerColliderHit hit)
          {
              if(hit.gameObject.tag == "Enemy")
              {
                  Life--;
                  Destroy(hit.gameObject);
              }
          }
      }
     ```
       </details>

     ### コード説明
     #### Timer.cs
     > [SerializeField]
     > public TextMeshProUGUI OverCount = null;
     > [SerializeField]
     > public Canvas gameClearCanvas = null;

     - TextとCanvasをコードで表示非表示設定するために作成
    
     > void Start()
     > {
     >    gameClearCanvas.gameObject.SetActive(false);
     >    Time.timeScale = 1.0f;
     > }
     
     - ゲーム開始時はクリア画面を非表示
     - ゲーム経過時間を1.0f（1秒）に設定

     #### PlayerScript.cs
     > [SerializeField]
     > public Canvas gameOverCanvas = null;
     > // Start is called before the first frame  update
     > void Start()
     > {
     >     Life = 3;
     >     gameOverCanvas.gameObject.SetActive(false);
     > }

     - 開始時はゲームオーバー画面を非表示

     > if(Life < 0)
     >    {
     >       //ゲームオーバー処理
     >       Time.timeScale = 0; //ゲーム時間停止
     >       gameOverCanvas.gameObject.SetActive(true);
     >   }
     - ライフが0以下になったらゲームオーバー画面を表示
 
 6. 下記のようにPlayerArmatureを設定
 <img src="./images/OverCanvas.png" width="75%">
     - PlayerScriptにOverCanvasをアタッチ
 1. 下記のようにGameManagerを設定
 <img src="./images/ClearCanvas.png" width="75% ">
     - TimerScriptにTimeTextとClearCanvasをアタッチ

 ### 実行
 完了したらクリア時とゲームオーバー時にCanvasが表示されているか確認

---
[次へ](./8Final.html)

[前へ戻る](./6Scene.html)

[Unity開発ページへ戻る](../index.html)