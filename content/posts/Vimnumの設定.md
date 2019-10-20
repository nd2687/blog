---
title: "Vimnumの設定"
date: 2019-10-04T18:30:49+09:00
tags: ["Vimnum", "ChromeExtension"]
---

date: 2019-10-04T18:30:49+09:00

## (俺的)Vimnumの設定

編集の仕方  

- Vimnumのアイコンをクリック  

- 左下の「Options」をクリック  

- Custom key mappings に以下を設定する(俺的)  

```
# Insert your preferred key mappings here.
map h previousTab
map l nextTab
unmap d
map dd removeTab
map u restoreTab
map H goBack
map L goForward
map <c-f> scrollFullPageDown
map <c-b> scrollFullPageUp
```

- 「Save Changes」をクリックで反映  

## その他コマンド

? ... コマンドヘルプをみれる。カスタムキーマップも反映されている。  
f ... ページ内リンクを表示  
b ... bookmarkから検索  
o ... bookmarkか履歴から検索  
T ... タブ検索  

## 感想

個人的にこのカスタムキーマップはかなり使いやすい
