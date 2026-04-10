---
name: html-validate
description: index.html の HTML 構文と JS ロジックを検証する
---
`index.html` を検証します。

1. HTML 構文チェック:
   ```
   python3 -c "import html.parser; html.parser.HTMLParser().feed(open('index.html').read()); print('HTML OK')"
   ```
2. JS の基本チェック（未定義変数、セミコロン漏れなど）をコードリーディングで確認する
3. CSS 変数が `:root` に正しく定義されているか確認する
4. xlsx.js の使用箇所（XLSX.utils, XLSX.writeFile）が正しいか確認する
5. 問題があれば報告し、修正案を提示する
