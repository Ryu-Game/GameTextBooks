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

## プロジェクト作成
1. VisualStudioでプロジェクトを作成
2. プロジェクト名`Janken`
   - [作成方法ページ](../MakeProject/3DMakeProject.html)

## コーディング
### main.cpp
 <details open><summary>main.cpp</summary>

 ```cpp
 #include "DxLib.h"
int WINAPI WinMain(HINSTANCE, HINSTANCE, LPSTR, int) {
	ChangeWindowMode(TRUE);	//ウィンドウモード変更
	DxLib_Init();			//Dxライブラリ初期化
	SetGraphMode(640, 480, 32);		//画面サイズ
	SetDrawScreen(DX_SCREEN_BACK);	//裏画面設定
	while (ProcessMessage() == 0) {
		ClearDrawScreen();	//画面初期化

		ScreenFlip();		//画面更新
	}
	DxLib_End();
	return 0;
}
 ```
 </details>

 ## コード説明
 ### main.cpp
 > #include "DxLib.h"
 
 #include をすることで別ファイルで宣言した内容を使えるようにする。

 > int WINAPI WinMain(HINSTANCE, HINSTANCE, LPSTR, int) \{

一般的なmain処理の書き方<br>
**※** C++言語は基本的にmain処理から始まるのでmain.cppから始まります。

 > ChangeWindowMode(TRUE);

- TRUE：`ウィンドウモード`
- FALSE：`フルスクリーンモード`

> DxLib_Init();	//Dxライブラリ初期化

DxLibソフトの初期化<br>
**※** DxLibを使用する際この処理をはじめに行う必要がある

> SetGraphMode(640, 480, 32);	//画面サイズ

ウィンドウ画面のサイズを設定<br>
`SetGraphMode(横サイズ, 縦サイズ, ビット数)`

> SetDrawScreen(DX_SCREEN_BACK);	//裏画面設定

描画を裏画面に設定<br>
**※** 表画面で常に描画し続けると描画されているものが<span style="color: red; ">チラつきゲームとして見づらい</span>

> while (ProcessMessage() == 0) {

正常処理中は常に動かし続ける

> ClearDrawScreen();		//画面初期化

現在描画されているものを削除

> ScreenFlip();			//画面更新

裏画面に描画したものを表画面に描画

> DxLib_End();

ゲーム強制終了処理

---
[次へ](./3Scene.html)

[前へ戻る](./1Jindex.html)

[DxLib開発ページへ戻る](../Dindex.html)