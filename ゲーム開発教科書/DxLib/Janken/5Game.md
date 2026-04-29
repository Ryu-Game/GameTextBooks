# じゃんけんゲーム
初心者向けで1～2時間想定の簡単なじゃんけんゲームの作り方をご紹介します。

## 目次
1. [初めに](./1Jindex.html)
2. [プロジェクト作成](./2JMakeProject.html)
3. [画面遷移作成](./3Scene.html)
4. [メニュー画面作成](./4Menu.html)
5. [ゲーム画面作成](./5Game.html)
6. [最後に](./6Final.html)
---
### ゲーム画面作成 
---
1. ソースファイルを右クリックで新しい項目を選択
2. C++ファイルを選択
3. 名前`Game.cpp`
4. ヘッダーファイルを右クリックで新しい項目を選択
5. ヘッダーファイルを選択
6. 名前`Game.h`

### コーディング
---
#### Game.h
 <details open><summary>Game.h</summary>

 ```h
 #pragma once

struct Game {
	bool changeflg;
	int counttime;
	int judgenum;	//0:あいこ1:かち2:まけ
};

struct Player {
	int gh[3];
	int jw = 300, jh = 360;
	double exRate = 0.5;
	int pHand, eHand;
};

extern struct Game mg;
extern struct Player mp;

void Game_Initialize();	//初期化
void Game_Finalize();	//終了処理
void Game_Update();		//更新
void Game_Draw();		//描画

void PlayerHands();		//手の状態処理
void Game_Judge();		//勝敗処理
 ```
 </details>

#### Game.cpp
 <details open><summary>Game.cpp</summary>

 ```cpp
#include "DxLib.h"
#include "Game.h"
#include "SceneMgr.h"

struct Game mg;
struct Player mp;

void Game_Initialize() {
	LoadDivGraph("images/janken.png", 3, 3, 1, mp.jw, mp.jh, mp.gh);
	mg.changeflg = true;
	mg.counttime = rand();
	mg.judgenum = 4;
}


void Game_Finalize() {
	
}


void Game_Update() {
	
	PlayerHands();
	Game_Judge();
}


void Game_Draw() {
	SetFontSize(60);
	DrawBox(0, 0, mgr.SCREEN_WIDTH, mgr.SCREEN_HEIGHT / 2, 0xff0000, 1);			//上半分赤を表示
	DrawBox(0, mgr.SCREEN_HEIGHT / 2, mgr.SCREEN_WIDTH, mgr.SCREEN_HEIGHT, 0x0000ff, 1);	//上半分青を表示

	//エネミー表示
	DrawRotaGraph(mgr.SCREEN_WIDTH / 2, mp.jh * mp.exRate - 50, mp.exRate, 0, mp.gh[mp.eHand], 1, 0);

	//Player表示
	DrawRotaGraph(mgr.SCREEN_WIDTH / 2, mp.jh+30, mp.exRate, 0, mp.gh[mp.pHand], 1, 0);

	//勝敗描画
	switch (mg.judgenum) {
	case 0:
		DrawString(mgr.SCREEN_WIDTH / 2, mgr.SCREEN_HEIGHT - 120, "あいこで・・・", 0xffffff);
		break;
	case 1:
		DrawString(30, mgr.SCREEN_HEIGHT - 120, "勝ち！", 0xffffff);
		break;
	case 2:
		DrawString(30, mgr.SCREEN_HEIGHT - 120, "負け…", 0xffffff);
		break;
	}
}

void PlayerHands() {
	//ランダム描画処理
	if (mg.changeflg) {
		mp.eHand = ++mg.counttime / 5 % 3;
		mp.pHand = ++mg.counttime / 5 % 3;
	}

	//PlayerHand処理
	if (mg.changeflg) {
		if (CheckHitKey(KEY_INPUT_1) != 0 ||
			CheckHitKey(KEY_INPUT_2) != 0 ||
			CheckHitKey(KEY_INPUT_3) != 0) {
			if (CheckHitKey(KEY_INPUT_1) != 0) {
				mp.pHand = 0;
			}
			else if (CheckHitKey(KEY_INPUT_2) != 0) {
				mp.pHand = 1;
			}
			else if (CheckHitKey(KEY_INPUT_3) != 0) {
				mp.pHand = 2;
			}
			mg.changeflg = false;
		}
	}
}

void Game_Judge() {
	if (!mg.changeflg) {
		if (mg.judgenum == 4) {

			int ehand_Judge = mp.eHand;
			if (ehand_Judge == 0) {
				ehand_Judge = 3;
			}
			//あいこ
			if (mp.pHand == mp.eHand) {
				mg.judgenum = 0;
			}
			//勝ち
			else if (mp.pHand == ehand_Judge - 1) {
				mg.judgenum = 1;
			}
			//負け
			else {
				mg.judgenum = 2;
			}

		}
		if (CheckHitKey(KEY_INPUT_SPACE) != 0) {
			mg.judgenum = 4;			
			mg.changeflg = true;
		}
	}
}
 ```
 </details>

### コード説明
#### Game.h
```h
struct Game {
	bool changeflg;
	int counttime;
	int judgenum;	//0:あいこ1:かち2:まけ
};
```
 ゲーム全体に関わるメンバー変数を作成
 | メンバー変数 | 働き |
 | --- | --- |
 | bool changeflg | ゲーム判断 | 
 | int counttime | ランダムに手を描画させるカウント | 
 | int judgenum | 勝敗判定 | 

```h
struct Player {
	int gh[3];
	int jw = 300, jh = 360;
	double exRate = 0.5;
	int pHand, eHand;
};
```
 プレイヤー処理に関わるメンバー変数を作成
  | メンバー変数 | 働き |
 | --- | --- |
 | int gh[3] | 手の画像 | 
 | int jw = 300, jh = 360 | 描画サイズ | 
 | double exRate = 0.5 | 描画角度 | 
 | int pHand, eHand | プレイヤーと相手の手の状態 | 

```h
 void PlayerHands();
```
 手の状態を判断処理

```h
 void Game_Judge();
```
 ゲーム判定処理

#### Game.cpp
```cpp
struct Game mg;
struct Player mp;
```
ヘッダーファイルで作成したゲーム情報、プレイヤー情報をcppファイルでも読めるように宣言

```cpp
void Game_Initialize() {
    LoadDivGraph("images/janken.png", 3, 3, 1, mp.jw, mp.jh, mo.gh);
    mg.changeflg = true;
    mg.counttime = rand();
    mg.judgenum = 4;
}
```

初期化処理
 - 画像格納
  `LoadDivGraph(画像URL, 分割数, 横分割数, 縦分割数, 横分割サイズ, 縦分割サイズ, 画像格納, 背景透過判断)`
 - 手を選んでいる状態に変更
  `mgame.changeflg = true`
 - ランダム時間をカウンターに格納
  `mgame.counttime = rand()`
 - 勝敗判定外の値格納
  `mgame.judgenum = 4`

```cpp
 DrawBox(0, 0, mgr.SCREEN_WIDTH, mgr.SCREEN_HEIGHT / 2, 0xff0000, 1);
 DrawBox(0, mgr.SCREEN_HEIGHT / 2, mgr.SCREEN_WIDTH, mgr.SCREEN_HEIGHT, 0x0000ff, 1);
```
背景として上（赤）下（青）に分けて描画
 - `DrawBox(左座標, 上座標, 右座標, 下座標, 色コード, 背景塗り判定)` 

```cpp
//エネミー表示
DrawRotaGraph(mgr.SCREEN_WIDTH / 2, mp.jh * mp.exRate - 50, mp.exRate, 0, mp.gh[mp.eHand], 1, 0);

//Player表示
DrawRotaGraph(mgr.SCREEN_WIDTH / 2, mp.jh+30, mp.exRate, 0, mp.gh[mp.pHand], 1, 0);
```
手の画像を描画
 - `DrawRotaGraph(X座標, Y座標, 拡大率, 角度, 画像, 透明度判定, 左右反転判定)`

```cpp
//勝敗描画
switch (mg.judgenum) {
case 0:
	DrawString(mgr.SCREEN_WIDTH / 2, mgr.SCREEN_HEIGHT - 120, "あいこで・・・", 0xffffff);
	break;
case 1:
	DrawString(30, mgr.SCREEN_HEIGHT - 120, "勝ち！", 0xffffff);
	break;
case 2:
	DrawString(30, mgr.SCREEN_HEIGHT - 120, "負け…", 0xffffff);
	break;
}
```
 勝敗が決まり次第結果を画面に描画

```cpp
void PlayerHands() {
	//ランダム描画処理
	if (mg.changeflg) {
		mp.eHand = ++mg.counttime / 5 % 3;
		mp.pHand = ++mg.counttime / 5 % 3;
	}
```
 勝敗がまだ決まっていない状態だったらランダムで手を描画

```cpp
 if (CheckHitKey(KEY_INPUT_1) != 0 ||
    CheckHitKey(KEY_INPUT_2) != 0 ||
    CheckHitKey(KEY_INPUT_3) != 0) 
```
 キーボードの1,2,3どれかを入力したらプレイヤーの手を決める
  | キーボード | 手 |
  | --- | --- |
  | 1 | ぐー |
  | 2 | ちょき |
  | 3 | ぱー |

```cpp
 mgame.changeflg = false;
```

手を決めたらランダム処理を止める

```cpp
 int ehand_Judge = mgame.eHand;
 if (ehand_Judge == 0) {
    ehand_Judge = 3;
 }
 //あいこ
 if (mgame.pHand == mgame.eHand) {
    mgame.judgenum = 0;
 }
 //勝ち
 else if (mgame.pHand == ehand_Judge - 1) {
    mgame.judgenum = 1;
 }
 //負け
 else {
    mgame.judgenum = 2;
 }
```
 1. 手の状態を計算するために格納
     > int ehand_Judge = mgame.eHand;
 2. 手が同じだったらあいこ処理
     > if (mgame.pHand == mgame.eHand) {
	 >   mgame.judgenum = 0;
	 > }
 3. 勝ちの処理
     > else if (mgame.pHand == ehand_Judge - 1) {
	 >	 mgame.judgenum = 1;
	 > }

     | ehand_Judge | プレイヤー | 相手 |
     | --- | --- | --- |
     | 0 | ぐー | ちょき |
     | 1 | ちょき | ぱー |
     | 2 | ぱー | ぐー |
 4. 負けの処理
     > else {
	 >  mgame.judgenum = 2;
	 > }

```cpp
if (CheckHitKey(KEY_INPUT_SPACE) != 0) {
    mgame.judgenum = 4;	
    mgame.changeflg = true;
}
```
スペースキーを押したら再度ゲーム開始

### SceneMgr.cpp更新
 #### SceneMgr.cpp
 <details open><summary>SceneMgr.cpp</summary>

```cpp
#include "DxLib.h"
#include "SceneMgr.h"
#include "Game.h"
#include "Menu.h"

static eScene mScene = eScene_Menu;		//シーン管理変数(デバッグでゲーム開始)
static eScene mNextScene = eScene_None; //次のシーン管理変数

static void SceneMgr_InitializeModule(eScene scene);//指定モジュールを初期化する
static void SceneMgr_FinalizeModule(eScene scene);	//指定モジュールの終了処理を行う

struct SceneMgr mgr;

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
	case eScene_Game:		//ゲーム画面
		Game_Update();
		break;
	}
}

/******************************
* 描画処理
*******************************/
void SceneMgr_Draw() {
	DrawGraph(0, 0, mgr.BackImg, false);

	switch (mScene) {
	case eScene_Menu: //現在の画面がメニュー画面なら
		Menu_Draw(); 
		break;
	case eScene_Game:
		Game_Draw();
		break;
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
	switch (scene) { //シーンによって処理を分岐
	case eScene_Menu: //指定画面がメニュー画面なら
		Menu_Initialize(); 
		mgr.BackImg = LoadGraph("images/MenuBack.png");
		break;
	case eScene_Game:
		Game_Initialize();
		break;	
	}
}
// 引数sceneモジュールの終了処理を行う
static void SceneMgr_FinalizeModule(eScene scene) {
	switch
		(scene) { //シーンによって処理を分岐
	case eScene_Menu: //指定画面がメニュー画面なら
		Menu_Finalize(); 
		break;

	case eScene_Game:
		Game_Finalize();
		break;
	}
	mgr.BackImg = 0;
}
```
</details>

ゲーム画面を実行するためにコメントアウト解除

---
[次へ](./6Final.html)

[前へ戻る](./4Menu.html)

[DxLib開発ページへ戻る](../Dindex.html)