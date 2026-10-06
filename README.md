# AlinerWebApp

ALINERミニアプリ（LIFF）のフロントエンド（画面のHTML/CSS/JS）だけを置くディレクトリ。
GitHub Pagesなど静的ホストへ配信することを想定している。

## なぜGASから分離したか

GASのWebアプリ(`/exec`)は内部で`script.googleusercontent.com`へ302リダイレクトする仕様があり、
これがLINEミニアプリのエンドポイントURLのドメイン検証と噛み合わず、実機で`liff.init()`が
失敗する/表示が崩れる不具合の原因と見られるため（`LINE_HealthCoach\SystemDesign\SYSTEM_DESIGN.md` §10-56参照）。

画面はここ（静的ファイル）、バックエンド（LINE Webhook・バッチ・Notion/Gemini連携）は
`LINE_HealthCoach` のGASプロジェクトに残したまま。データ取得は`fetch()`で
`LINE_HealthCoach`のdoPostをJSON APIとして呼ぶ（GAS側のCORS対応・ディスパッチャ実装は別途）。

## 構成

- `index.html` … 画面本体（元は`LINE_HealthCoach\src\AlinerApp.html`。GASのテンプレート構文と
  `google.script.run`を除去し、`fetch()`ベースに書き換え済み）

## デプロイ

GitHub Pagesにpushして配信し、LINE Developers ConsoleのALINERミニアプリの
エンドポイントURL（開発用/審査用/本番用）をそのURLに向け直す想定。
