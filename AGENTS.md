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
- 車両の追加や売約済み（SOLD OUT）更新を行う際は、ルート直下の SKILLS.md に定義されたスキル手順・HTML構造・Tailwind CSS規約に必ず準拠すること。
- コードの修正後は、自律的にGitでコミットを作成し、必要に応じてPushを行うこと。

# Language & Communication Rules

- **User Communication:**
  - ユーザーへの説明、回答、進捗報告、および生成するドキュメント（Artifacts含む）の解説は、すべて自然な日本語で行うこと。
- **Technical Artifacts & Code:**
  - ソースコード、変数名、関数名、テストコード自体は標準的な英語（またはプロジェクトの命名規約）を使用すること。
  - コードコメントは日本語で記述すること。
- **Git & System Commands:**
  - Gitのコミットメッセージは、プロジェクトの規約（Conventional Commits等）に従い、指定がない限り英語で簡潔に記述すること。