# 池田皐月 ポートフォリオサイト

動画編集・AI活用・SNSコンテンツ制作のポートフォリオサイト。

## 構成

依存ライブラリなしの静的サイト。`index.html` 1ファイルにHTML・CSS・JavaScriptをすべて内包している。

```
index.html    サイト本体（データもこの中の window.PROFILE / WORKS / CONTACT に定義）
images/       プロフィール写真・作品サムネイル（WebP）、OGP画像・ファビコン
videos/       作品動画（Web用に圧縮済みのmp4）
```

ビルド不要。ローカルで確認する場合は、このフォルダで簡易サーバーを立てて開く。

```bash
py -m http.server 8765
```

`file://` で直接開いても動くが、動画やOGPの挙動はサーバー経由の方が本番に近い。

## 作品を追加するとき

1. 動画をWeb用に圧縮する

   ```bash
   ffmpeg -i 元動画.mp4 -vf "scale=640:-2" -c:v libx264 -preset slow -crf 28 -movflags +faststart -c:a aac -b:a 96k videos/新しい作品.mp4
   ```

2. サムネイルを書き出してWebPに変換する

   ```bash
   ffmpeg -i 元動画.mp4 -ss 2 -frames:v 1 -vf "scale=480:-2" images/新しい作品-thumb.webp
   ```

3. `index.html` の `window.WORKS` に追記する（`id` は重複しない数値）

   ```js
   {
     id: 4,
     emoji: "🎬",
     title: "作品タイトル",
     category: "Short Video",
     thumbnail: "images/新しい作品-thumb.webp",
     localVideo: "videos/新しい作品.mp4",
     externalUrl: "",
     description: "作品の説明",
     responsibilities: ["撮影", "編集"],
     tools: ["Premiere Pro"],
     duration: "30秒"
   }
   ```

サムネイル枠は縦型（9:16）が既定。横長の作品は `orientation: "landscape"` を足すと16:9で表示される。

## 確認しておくこと

見た目を変更したら、ブラウザで **375px / 768px / 1280px** の3サイズを確認する。過去に、縦型サムネイルが横長枠で切り取られていた／スマホでファーストビューが崩れていた不具合を出したことがある。
