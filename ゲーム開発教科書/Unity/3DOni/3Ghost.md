# ゴースト鬼ごっこ
初心者向けで1～2時間想定の簡単な鬼ごっこゲームの作り方をご紹介します。

## 目次
1. [初めに](./1index.html)
2. [プロジェクト作成](./2Project.html)
3. [鬼設定](./3Ghost.html)
4. [メニュー画面作成](./4Menu.html)
5. [ゲーム画面作成](./5Game.html)
6. [最後に](./6Final.html)
---

## 鬼設定
 ### コード追加
  1. [プロジェクト作成](./2Project.html)で追加した、下記の場所の`Ghost`を選択
     - Assets→GhostCharacter_Free→Prefab
  2. 選択後、Inspector画面でGhostScriptをRemoveComponentで削除
   <img src="./images/鬼の追加.png" width="60%">
  3. 同様のやり方でCharcter Controllerも削除
  3. 下記の場所にPlayerを追いかける鬼の`C# Script`を追加<br>
     ※下記の場所で左クリックしてC＃ Scriptを選択
     - Assets→Scripts
     - 名前は`EnemyMove`
  4. 作成したScriptを1のGhostを選択後、Inspector画面の一番下`Add Component`で下記を追加
     - EnemyMove
     - MeshCollider
  5. MeshColliderのMeshをGhostに変更

 ### コーティング
 ---
 #### EnemyMove.cs
 <details open><summary>EnemyMove.cs</summary>

 ```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyMove : MonoBehaviour
{
    Transform PlayerPosi;
    [SerializeField] float speed = 2;

    // Start is called before the first frame update
    void Start()
    {
        //プレイヤーポジション取得
        PlayerPosi = GameObject.FindGameObjectWithTag("Player").transform;
    }
    void Update(){
        //プレイヤーとの距離が0.1fになったら追いかけない
        if(Vector3.Distance(transform.position, PlayerPosi.position) < 0.1f){
            return;
        }

        //プレイヤー追いかける
        transform.position = Vector3.MoveTowards(
                                transform.position,
                                new Vector3(PlayerPosi.position.x, PlayerPosi.position.y, PlayerPosi.position.z),
                                speed * Time.deltaTime);
        transform.LookAt(PlayerPosi);
    }
}
 ```
 </details>

 ### コード説明
 #### EnemyMove.cs
 ```cs
 Transform PlayerPosi;
 [SerializeField] float speed = 2;

  // Start is called before the first frame update
　void Start()
　{
　  //プレイヤーポジション取得
    PlayerPosi = GameObject.FindGameObjectWithTag("Player").transform;
   }
 ```
 - `Transform`：オブジェクトの座標や角度などを取得するために宣言
 - `[SerializeField]`：Inspector画面で編集可能にする機能
 - PlayerPosiにPlayerオブジェクトの座標を取得
   ※Playerキャラクターのタグ名を`Player`として設定
 ```cs
 //プレイヤー追いかける
        transform.position = Vector3.MoveTowards(
                                transform.position,
                                new Vector3(PlayerPosi.position.x, PlayerPosi.position.y, PlayerPosi.position.z),
                                speed * Time.deltaTime);
 ```
 敵キャラがPlayerを追いかける処理
 `MoveTowards`とは第一引数のオブジェが第二引数のオブジェを追いかける
  - MoveTowards(動かすオブジェクト,対象オブジェクト,スピード)

 ```cs
 transform.LookAt(PlayerPosi);
 ```
 敵キャラがPlayerの方角に傾ける処理
  - LookAt(傾け先対象オブジェクト)

 ### タグの追加
 Playerオブジェクトのタグを`Player`に変更
 <img src="./images/タグ追加.png" width="75%">
 タグにPlayerがいない場合`Add Tag...`で追加

 ### 実行
 完了したら下記の`再生マーク`もしくは`Ctr + P`を押してゲームを実行<br>
 GhostがPlayerを追いかけているか確認
 <img src="./images/再生.png" width="60%">

---
[次へ](./2Project.html)

[前へ戻る](./2Project.html)

[Unity開発ページへ戻る](../index.html)