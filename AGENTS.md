# Agent Execution & Safety Policy

## 1. 自律実行（Read-Only & Safe Operations）
- ファイルやディレクトリの検索（`dir`, `Get-ChildItem`, `ls` など）
- ファイル内容の閲覧・構文チェック
- Gitの読み取り系コマンド（`git status`, `git log`, `git diff`, `git branch` など）
- ローカルビルドやテスト実行
上記のようなシステム破壊の恐れがない操作は、ユーザーの承認を待たずに自律的に実行すること。

## 2. 承認必須アクション（Destructive & External Operations）
- ファイルやディレクトリの完全削除（`rm`, `del`, `Remove-Item` など）
- 新たな外部ソフトウェアやパッケージのシステム全体へのインストール
- リポジトリの履歴改変（`git push --force`, `git reset --hard` など）
上記に該当する処理を行う場合は、必ず事前にユーザーへ理由と影響範囲を提示し、承認を求めること。

## 3. Webサイト制作・保守パイプラインの標準挙動
- HTML/CSSの生成や更新時は、既存の共通ナビゲーションやスタイル定義との整合性を必ず確認すること。
- コードの修正後は、自律的にGitでコミットを作成し、必要に応じてPushを行うこと。