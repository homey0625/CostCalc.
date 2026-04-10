# CostCalc — PHILOCOFFEA カフェ原価帳

## プロジェクト概要
シングルページ HTML アプリ。バックエンドなし。CDN から xlsx.js を読み込む。
ファイル: `index.html` 1 本にすべての HTML / CSS / JS が含まれる。

## コーディングスタイル
- インデント: スペース 2 つ
- CSS 変数は `:root` に集中管理する（既存変数を流用し、不必要に増やさない）
- JS は ES Modules 構文ではなく Vanilla JS（script タグ内に直書き）
- 日本語 UI テキストを維持すること
- ブランドカラー: `--brand: #8C1324`（変更禁止）

## 変更時の必須確認事項
- 変更後は必ず `index.html` が有効な HTML か確認する（`python3 -c "import html.parser; html.parser.HTMLParser().feed(open('index.html').read())"` または ブラウザで開く）
- Excel エクスポート機能（xlsx.js）が破壊されていないか確認すること
- モバイル幅 (max-width: 520px) のレイアウトが崩れていないか確認すること

## ワークフロー
- 大きな変更の前に Plan Mode で調査・計画を立てる
- 実装後にブラウザ確認またはスクリーンショット検証を行う
- コミットメッセージは日本語 or 英語どちらでも可

## 重要な制約（IMPORTANT）
- `index.html` は **1 ファイル構成を維持** すること（分割禁止）
- 外部 CDN の追加は最小限に抑え、事前に確認すること
- ブランドカラー・フォント設定を無断で変更しないこと
