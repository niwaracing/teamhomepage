# Agent Execution & Safety Policy

## 1. 承認必須アクション（Destructive & External Operations）
- ファイルやディレクトリの完全削除（`rm`, `del`, `Remove-Item` など）
- 新たな外部ソフトウェアやパッケージのシステム全体へのインストール
- リポジトリの履歴改変（`git push --force`, `git reset --hard` など）
上記に該当する処理を行う場合は、必ず事前にユーザーへ理由と影響範囲を提示し、承認を求めること。

## 2. Webサイト制作・保守パイプラインの標準挙動
- HTML/CSSの生成や更新時は、既存の共通ナビゲーションやスタイル定義との整合性を必ず確認すること。
- 車両の追加や売約済み（SOLD OUT）更新を行う際は、ルート直下の SKILLS.md に定義されたスキル手順・HTML構造・Tailwind CSS規約に必ず準拠すること。
- コードの修正後は、自律的にGitでコミットを作成し、必要に応じてPushを行うこと。

