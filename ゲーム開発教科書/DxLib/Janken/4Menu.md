# じゃんけんゲーム
初心者向けで1～2時間想定の簡単なじゃんけんゲームの作り方をご紹介します。

## 目次
1. [初めに](./1index.html)
2. [プロジェクト作成](./2Project.html)
3. [画面遷移作成](./3Scene.html)
4. [メニュー画面作成](./4Menu.html)
5. [ゲーム画面作成](./5Game.html)
6. [最後に](./6Final.html)
---
### メニュー画面作成
---
1. ソースファイルを右クリックで新しい項目を選択
2. C++ファイルを選択
3. 名前`Menu.cpp`
4. ヘッダーファイルを右クリックで新しい項目を選択
5. ヘッダーファイルを選択
6. 名前`Menu.h`

### コーディング
---
#### Menu.h
 <details open><summary>Menu.h</summary>

 ```h
 #pragma once

 void Menu_Initialize();	//初期化
 void Menu_Finalize();	//終了処理
 void Menu_Update();		//更新
 void Menu_Draw();		//描画
 ```
 </details>

#### Menu.cpp
 <details open><summary>Menu.cpp</summary>

 ```cpp
  #include "DxLib.h"
  #include "Menu.h"
  #include "SceneMgr.h"
  #include "Input.h"

  void Menu_Initialize() {

  }  

  void Menu_Finalize() {

  }

  void Menu_Update(){
	if (CheckHitKey(KEY_INPUT_SPACE)) {
		SceneMgr_ChangeScene(eScene_Game);
	}
  }

  void Menu_Draw() {
	SetFontSize(50);
	DrawString(mgr.SCREEN_WIDTH/2- 120, mgr.SCREEN_HEIGHT/2 - 50, " SPASEキー\nゲーム開始", 0xffffff);
  }
```
</details>

### コード説明
#### Menu.cpp
```cpp
 if (CheckHitKey(KEY_INPUT_SPACE)) {
	SceneMgr_ChangeScene(eScene_Game);
 }
```
 
  1. ESCキーを押したらif文の処理を実行
  2. 現在シーンから`eScene_Game`に変更

```cpp
SetFontSize(50);
```

 描画する文字のサイズを50に設定

```cpp
DrawString(mgr.SCREEN_WIDTH/2- 120, mgr.SCREEN_HEIGHT/2 - 50, " SPASEキー\nゲーム開始", 0xffffff);
```

 DxLibでの文字を描画させる方法
 - `DrawString(x座標, Y座標, 描画文字, 文字色)`

### SceneMgr.cpp更新
#### SceneMgr.cpp
<details open><summary>Menu.cpp</summary>

```cpp
#include "DxLib.h"
#include "SceneMgr.h"
//#include "Game.h"
#include "Menu.h"

static eScene mScene = eScene_Menu;		//シーン管理変数(デバッグでゲーム開始)
static eScene mNextScene = eScene_None; //次のシーン管理変数

struct SceneMgr mgr;

static void SceneMgr_InitializeModule(eScene scene);//指定モジュールを初期化する
static void SceneMgr_FinalizeModule(eScene scene);	//指定モジュールの終了処理を行う
/******************************
* 初期化処理
*******************************/
void SceneMgr_Initialize() {
	SceneMgr_InitializeModule(mScene);
}

/******************************
* 終了処理
*******************************/
void SceneMgr_Finalize() {
	SceneMgr_FinalizeModule(mScene);
}

/******************************
* 更新処理
*******************************/
void SceneMgr_Update() {
	if (mNextScene != eScene_None) {		//次のシーンがセットされていたら
		SceneMgr_FinalizeModule(mScene);	//現在のシーンの終了処理を実行
		mScene = mNextScene;				//次のシーンを現在のシーンセット
		mNextScene = eScene_None;			//次のシーン情報をクリア
		SceneMgr_InitializeModule(mScene);	//現在のシーンを初期化
	}
	
	if (CheckHitKey(KEY_INPUT_ESCAPE)) {
		DxLib_End();
	}
	
    switch (mScene) {
	case eScene_Menu:		//メニュー画面
		Menu_Update();
		break;
	//case eScene_Game:		//ゲーム画面
	//	Game_Update();
	//	break;
	}
}

/******************************
* 描画処理
*******************************/
void SceneMgr_Draw() {
	DrawGraph(0, 0, mgr.BackImage, false);

	switch (mScene) {
	case eScene_Menu:
		Menu_Draw();
		break;
	//case eScene_Game:
	//	Game_Draw();
	//	break;
	}
}
/******************************
* 関数
*******************************/
// 引数 nextScene にシーンを変更する
void SceneMgr_ChangeScene(eScene NextScene) {
	mNextScene = NextScene;	//次のシーンをセットする
}

// 引数sceneモジュールを初期化する
static void SceneMgr_InitializeModule(eScene scene) {
	switch (scene) { 
	case eScene_Menu: 
		Menu_Initialize();
		mgr.BackImage = LoadGraph("images/MenuBack.png");
		break;
	//case eScene_Game:
	//	Game_Initialize();
	//	break;
	}
}

// 引数sceneモジュールの終了処理を行う
static void SceneMgr_FinalizeModule(eScene scene) {
	switch(scene) { 
	case eScene_Menu:
		Menu_Finalize();
		break;
	//case eScene_Game:
	//	Game_Finalize();
	//	break;
	}
	mgr.BackImage = 0;
}
```
</details>

### コード説明
```cpp
#include "Menu.h"
```
 追加したMenu処理を追加

```cpp
 mgr.BackImage = LoadGraph("images/MenuBack.png");
```

背景画像変数に画像を格納
- `LoadGraph(画像URL)`

```cpp
switch (mScene) {
	case eScene_Menu: //現在の画面がメニュー画面なら
		Menu_Draw(); 
		break;
    // case eScene_Game:
        // 	Game_Draw();
        // 	break;
}
```
メニュー画面を実行するためにコメントアウトを解除

### 実行
実行後この画面になっていたら成功
<img src="../Janken/images/Back_Menu.png" width="50%">
---
[次へ](./5Game.html)

[前へ戻る](./3Scene.html)

[DxLib開発ページへ戻る](../index.html)