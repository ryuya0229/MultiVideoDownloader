# SignPath.io OSS 署名 セットアップ手順

MultiVideoDownloader を SignPath.io の OSS 無料プランで署名するための手順書。

---

## 前提条件（チェック済み）

- [x] パブリックな GitHub リポジトリ
  - https://github.com/ryuya0229/MultiVideoDownloader
- [x] OSI 承認ライセンス（MIT）
  - `LICENSE` ファイルが存在
- [x] ビルドプロセスが CI（GitHub Actions）で再現可能
  - ※ 本プロジェクトは現状ローカルビルドなので、申請前に CI 化が必要

---

## Step 1: GitHub Actions で CI ビルドを構築

SignPath の OSS 無料プランは **GitHub Actions 経由でのビルド** が必須条件。
ローカルでビルドした exe に署名することはできない。

### 1-1. ワークフローファイル作成

`.github/workflows/build.yml`:

```yaml
name: Build

on:
  push:
    tags:
      - 'v*'
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pyinstaller

      - name: Build exe
        run: pyinstaller MultiVideoDownloader.spec --noconfirm

      - name: Upload unsigned artifact
        uses: actions/upload-artifact@v4
        with:
          name: unsigned-exe
          path: dist/MultiVideoDownloader.exe
```

### 1-2. requirements.txt の整備

ローカルで使用しているパッケージを出力:

```powershell
pip freeze > requirements.txt
```

不要なパッケージを削除してコミット。

---

## Step 2: SignPath.io に登録申請

### 2-1. アカウント作成
1. https://signpath.io/ にアクセス
2. 「Sign up for free」→ GitHub アカウントで連携

### 2-2. OSS プロジェクトとして申請
1. ダッシュボードで「New Project」
2. 「Open Source Free Plan」を選択
3. 以下の情報を入力:
   - **Project name**: MultiVideoDownloader
   - **Repository URL**: https://github.com/ryuya0229/MultiVideoDownloader
   - **License**: MIT
   - **Build system**: GitHub Actions
   - **Description**: Desktop application for video download & playback
4. 申請送信 → 審査に **2〜4週間**

### 2-3. 審査通過後の設定
1. SignPath が提供する署名ポリシー（Origin Verification）をプロジェクトに追加
2. GitHub Secrets に以下を登録:
   - `SIGNPATH_API_TOKEN`
   - `SIGNPATH_ORGANIZATION_ID`
   - `SIGNPATH_PROJECT_SLUG`
   - `SIGNPATH_SIGNING_POLICY_SLUG`

---

## Step 3: 署名ステップを CI に追加

`.github/workflows/build.yml` に追記:

```yaml
      - name: Submit signing request
        uses: signpath/github-action-submit-signing-request@v1
        with:
          api-token: ${{ secrets.SIGNPATH_API_TOKEN }}
          organization-id: ${{ secrets.SIGNPATH_ORGANIZATION_ID }}
          project-slug: ${{ secrets.SIGNPATH_PROJECT_SLUG }}
          signing-policy-slug: ${{ secrets.SIGNPATH_SIGNING_POLICY_SLUG }}
          github-artifact-id: ${{ steps.upload.outputs.artifact-id }}
          wait-for-completion: true
          output-artifact-directory: 'signed'

      - name: Upload signed artifact to release
        uses: softprops/action-gh-release@v2
        with:
          files: signed/MultiVideoDownloader.exe
```

---

## Step 4: FFmpeg の扱い（今後の課題）

FFmpeg も SAC でブロックされるが、**別リポジトリの再配布には SignPath の OSS 枠を使いにくい**。

### 選択肢
- **A**: FFmpeg を同梱して一緒に署名（exe サイズ +80MB）
- **B**: 当面は README の手動許可手順で対応
- **C**: 将来的に専用の署名ワークフローを別途構築

現在は **B** を採用中。

---

## 注意点

### レピュテーション問題
SignPath の標準署名（非 EV）は、SmartScreen/SAC のレピュテーション蓄積に時間がかかる（数週間〜数ヶ月）。
署名後すぐにブロックが消えるわけではないので注意。

### ビルド再現性
SignPath は **同じソースから同じバイナリが生成できる** ことを前提とする。
依存パッケージのバージョンは `requirements.txt` で固定すること。

### 月間ビルド上限
OSS 無料プランには月間署名回数の制限がある可能性。
頻繁なリリースは控える。

---

## 参考リンク

- [SignPath.io OSS Free Plan](https://about.signpath.io/product/open-source)
- [SignPath GitHub Action](https://github.com/signpath/github-action-submit-signing-request)
- [公式申請フォーム](https://signpath.io/open-source)
