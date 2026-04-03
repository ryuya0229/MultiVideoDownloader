<p align="center">
  <img src="https://github.com/ryuya0229/MultiVideoDownloader/blob/main/app_icon.ico" alt="MultiVideo Downloader" width="128">
</p>

<h1 align="center">MultiVideo Downloader & Player</h1>

<p align="center">
  動画の検索・ダウンロード・再生・字幕生成をひとつにまとめたデスクトップアプリ
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.4-blue" alt="Version">
  <img src="https://img.shields.io/badge/platform-Windows-0078D6?logo=windows" alt="Platform">
  <img src="https://img.shields.io/badge/license-Personal_Use-green" alt="License">
</p>

---

## Features

### Download
- **動画検索 & ダウンロード** — キーワード検索から直接ダウンロード
- **URL指定ダウンロード** — URLを貼り付けるだけで動画・音声を保存
- **プレイリスト一括DL** — プレイリストやチャンネルの動画をまとめてダウンロード
- **キューによるバッチ処理** — 複数のダウンロードをキューで管理
- **フォーマット選択** — MP4, WebM, MP3, WAV など多数の形式に対応

### Player
- **VLCベースの内蔵プレイヤー** — 別途ソフト不要でそのまま再生
- **YouTube風コントロール** — シークバー、ボリューム、再生速度の調整
- **リピート & シャッフル** — 全曲リピート / 1曲リピート / シャッフル再生
- **字幕表示 (CC)** — SRT / VTT / ASS 形式の字幕ファイルに対応
- **オーディオモード** — 音楽ファイルの再生にも対応

### Subtitle
- **AI字幕生成** — faster-whisper によるローカル音声認識（無料・オフライン）
- **16言語対応** — 日本語、英語、中国語、韓国語ほか
- **単語レベルのタイミング** — 発話区間を正確に反映した字幕

### Tools
- **フォーマット変換** — 動画・音声ファイルの形式変換
- **トリミング** — 開始・終了時刻を指定してカット
- **ファイル結合** — 複数ファイルをひとつに結合

### Other
- **18言語対応UI** — 日本語 / English / 中文 / 한국어 ほか全18言語
- **ダウンロード履歴** — 過去のダウンロードを検索・管理
- **再生位置の保存** — 途中で閉じても続きから再生
- **自動アップデート** — 新しいバージョンを自動で通知

---

## Download

> **[Releases](https://github.com/ryuya0229/MultiVideoDownloader/releases)** ページから最新の `MultiVideoDownloader.exe` をダウンロードしてください。

### System Requirements

| 項目 | 要件 |
|------|------|
| OS | Windows 10 / 11 (64-bit) |
| メモリ | 4GB 以上推奨 |
| ストレージ | 約200MB（初回起動時に必要なツールを自動ダウンロード） |

### Getting Started

1. `MultiVideoDownloader.exe` をダウンロード
2. 好きな場所に置いて実行（インストール不要）
3. 初回起動時に利用規約が表示されます
4. 必要なツール (yt-dlp, ffmpeg, VLC) は自動でダウンロードされます

---

## Disclaimer

本ソフトウェアは動画・音声のダウンロード機能を提供します。
著作権者の許可なく著作物をダウンロード・複製・配布することは法律で禁止されています。
ダウンロードしたコンテンツの利用に関するすべての責任はユーザーに帰属します。

---

## Author

**ryuya0229**

---

<p align="center">
  <sub>Built with Python + Tkinter + VLC + faster-whisper</sub>
</p>
