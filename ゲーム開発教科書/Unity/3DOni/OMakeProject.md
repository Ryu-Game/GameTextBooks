# 鬼の追加

下記のURLから鬼キャラクターをダウンロード

[リンクテキスト](https://x.gd/Rroao)

## ダウンロード手順
1. Open in Unityをクリック
2. UnityEditorを開く
3. 制作しているUnity画面に移動

4. 下記の画面が出たらInportをクリック


スライド11右側の画像

---


### Scene画面上にダウンロードした**Ghost**を好きな場所に配置

※Ghostを選択してGhostScriptをRemoveComponentで削除

Ghostは下記の場所

場所：**Assets→GhostCharacter_Free→Prefab**

スライド12左側の画像

### 鬼キャラをPlayerに追いかけるための**C# Script(コード)** を追加

Script追加場所は下記の場所と名前

場所：Assets→Scripts

名前：EnemyMove

スライド12右側の画像

※左クリックで追加表示

## EnemyMove中身

using System.Collections; 

using System.Collections.Generic;

using UnityEngine;

public class EnemyMove : MonoBehaviour
