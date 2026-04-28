# じゃんけんゲーム
初心者向けで1～2時間想定の簡単なじゃんけんゲームの作り方をご紹介します。

## 目次
1. [初めに](Jindex.html)
2. [プロジェクト作成](JMakeProject.html)
3. [画面遷移作成](Scene.html)
4. [メニュー画面作成](Menu.html)
5. [ゲーム画面作成](Game.html)
6. [最後に](Final.html)
---
## 画面遷移とは
 画面遷移とはある画面から別の画面へ表示を切り替えること

### 画面遷移作成
---
1. ソースファイルを右クリックで新しい項目を選択
2. C++ファイルを選択
3. 名前`SceneMgr.cpp`
4. ヘッダーファイルを右クリックで新しい項目を選択
5. ヘッダーファイルを選択
6. 名前`SceneMgr.h`

### コーディング
---
#### SceneMgr.h
 <details open><summary>SceneMgr.h</summary>

 ```h
#pragma once
typedef enum {
	eScene_Menu,	    //メニュー画面
	eScene_Game,	    //ゲーム画面
	eScene_Config,	//設定画面
	eScene_None,	    //無し
} eScene;

static class SceneMgr {
public:
	//画面サイズの大きさ
	const int SCREEN_WIDTH = 640;
	const int SCREEN_HEIGHT = 480;
	//背景画像
	int BackImage;
}mgr;

void SceneMgr_Initialize();	//初期化
void SceneMgr_Finalize();	//終了処理
void SceneMgr_Update();	    //更新
void SceneMgr_Draw();		//描画

// 引数 nextSceneにシーンを変更する
void SceneMgr_ChangeScene(eScene nextScene);

 ```
 </details>

 #### SceneMgr.cpp

 <details open><summary>SceneMgr.cpp</summary>

 ```cpp
#include "DxLib.h"
#include "SceneMgr.h"
//#include "Game.h"
//#include "Menu.h"

static eScene mScene = eScene_Menu;		//シーン管理変数(デバッグでゲーム開始)
static eScene mNextScene = eScene_None; //次のシーン管理変数

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
	
    //switch (mScene) {
	//case eScene_Menu:		//メニュー画面
	//	Menu_Update();
	//	break;
	//case eScene_Game:		//ゲーム画面
	//	Game_Update();
	//	break;
	//}
}

/******************************
* 描画処理
*******************************/
void SceneMgr_Draw() {
	DrawGraph(0, 0, mgr.BackImage, false);

	//switch (mScene) {
	//case eScene_Menu:
	//	Menu_Draw();
	//	break;
	//case eScene_Game:
	//	Game_Draw();
	//	break;
	//}
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
	//switch (scene) { 
	//case eScene_Menu: 
	//	Menu_Initialize();
	//	mgr.BackImage = LoadGraph("images/MenuBack.png");
	//	break;
	//case eScene_Game:
	//	Game_Initialize();
	//	break;
	//}
}

// 引数sceneモジュールの終了処理を行う
static void SceneMgr_FinalizeModule(eScene scene) {
	//switch(scene) { 
	//case eScene_Menu:
	//	Menu_Finalize();
	//	break;
	//case eScene_Game:
	//	Game_Finalize();
	//	break;
	//}
	mgr.BackImage = 0;
}

 ```
 </details>

 ### コード説明
  #### SceneMgr.h
 
```h
typedef enum {
    eScene_Menu,   // メニュー画面
    eScene_Game,   // ゲーム画面
    eScene_Config, // 設定画面
    eScene_None,   // 無し
} eScene;
```
列挙型でプロジェクトのシーンを管理

```h
static class SceneMgr {
public:
	//画面サイズの大きさ
	const int SCREEN_WIDTH = 640;
	const int SCREEN_HEIGHT = 480;
	//背景画像
	int BackImage;
}mgr;
```
SceneMgrのクラスを作成<br>
**※** staticで作ることで**メモリに保存**

```h
 void SceneMgr_Initialize();	//初期化
 void SceneMgr_Finalize();		//終了処理
 void SceneMgr_Update();	    //更新
 void SceneMgr_Draw();			//描画
```

 ファイルの初期化や終了、更新、描画をまとめる場所を作成
 | 関数 | 働き |
 | --- | --- |
 | ファイル名_Initialize() | 初期化処理 |
 | ファイル名_Finalize() | 終了処理 |
 | ファイル名_Update() | 更新処理 |
 | ファイル名_Draw() | 描画処理 |

```h
void SceneMgr_ChangeScene(eScene nextScene);
```
次シーンに遷移する処理
- 例）メニュー画面からゲーム画面に移動

  ### SceneMgr.cpp
   ```cpp
    static eScene mScene = eScene_Menu;
    static eScene mNextScene = eScene_None; 
   ```

   ヘッダーファイルで作成したシーン要素（列挙型）を現在と次シーンの変数として作成

   ```cpp
    void SceneMgr_Update() {
    if (mNextScene != eScene_None) {
   	 SceneMgr_FinalizeModule(mScene);
   	 mScene = mNextScene;
   	 mNextScene = eScene_None;
   	 SceneMgr_InitializeModule(mScene);
    }
   ```
   次シーンがあるのであれば現在シーンを削除して現在シーンを次シーンに移動

   ```cpp
    if (CheckHitKey(KEY_INPUT_ESCAPE)) {
		DxLib_End();
	}
   ```
   ESCキーを押したら強制終了

   ```cpp
    DrawGraph(0, 0, mgr.BackImage, false);
   ```

   画像を描画する際に使用するDxLib方法<br>
   - `DrawGraph(X座標, Y座標, 画像, 背景透過判定)`

   ```cpp
    void SceneMgr_ChangeScene(eScene NextScene) {
    	mNextScene = NextScene;
    }
   ```
    現在シーンから次シーンに変更したいときに使用

   ```cpp
	switch(scene) { 
	case eScene_Menu:
   		Menu_Finalize();
   		break;
   	case eScene_Game:
   		Game_Finalize();
   		break;
	}
   ```

   現在シーンによって処理を働かせる
    - 現在シーン(scene)がメニュー画面だったらcase eScene_Menuに入る

## main.cpp更新

### コーディング
### main.cpp
<details open><summary>main.cpp</summary>

 ```cpp
  #include "DxLib.h"
#include "SceneMgr.h"

int WINAPI WinMain(HINSTANCE, HINSTANCE, LPSTR, int) {
	ChangeWindowMode(TRUE);	//ウィンドウモード変更
	DxLib_Init();			//Dxライブラリ初期化
	SetGraphMode(mgr.SCREEN_WIDTH, mgr.SCREEN_HEIGHT, 32);	//画面サイズ
	SetDrawScreen(DX_SCREEN_BACK);	//裏画面設定
	SceneMgr_Initialize();	//初期シーン
	while (ProcessMessage() == 0) {
		ClearDrawScreen();	//画面初期化

		SceneMgr_Update();	//更新
		SceneMgr_Draw();	//描画

		ScreenFlip();		//画面更新
	}
	SceneMgr_Finalize();	//終了画面

	DxLib_End();
	return 0;
}
 ```
  SceneMgrで作成したシーン処理をmain.cppに追加

---
[戻る](../../index.html)