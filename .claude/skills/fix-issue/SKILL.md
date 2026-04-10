---
name: fix-issue
description: GitHub Issue を調査して修正する標準ワークフロー
disable-model-invocation: true
---
GitHub Issue を調査して修正します: $ARGUMENTS

1. `gh issue view $ARGUMENTS` で Issue の内容を確認する
2. `index.html` の関連コードを特定する
3. 修正を実装する（1 ファイル構成を維持）
4. HTML 構文チェックを実行: `python3 -c "import html.parser; html.parser.HTMLParser().feed(open('index.html').read()); print('OK')"`
5. ブラウザで動作確認できるか確認する
6. 変更をコミット: `git add index.html && git commit -m "fix: <内容>"`
7. PR を作成する
