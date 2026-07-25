# CLAUDE.md

このファイルは、このリポジトリで Claude Code (claude.ai/code) が作業する際のガイドラインです。

## プロジェクト概要

task-board はタスクを管理するシンプルな Web アプリケーションです。
技術スタック: React + TypeScript + Vite。

機能:
- テキスト入力でタスクを追加
- チェックボックスで完了・未完了を切り替え
- タスクを削除
- 完了済みタスクはグレー表示（取り消し線付き）

## デプロイ先

https://mhtanaka17-collab.github.io/task-board/

## 技術スタック

- [React](https://react.dev/) 19
- [TypeScript](https://www.typescriptlang.org/) 6
- [Vite](https://vite.dev/) 8（ビルドツール、`@vitejs/plugin-react` 経由で React をサポート）
- [oxlint](https://oxc.rs/docs/guide/usage/linter.html) — 静的解析
- ホスティング: GitHub Pages（`.github/workflows/deploy.yml` で自動デプロイ）
- 状態の永続化: ブラウザの `localStorage`（バックエンド・DBは使用しない）

## コンポーネントの命名規約

- コンポーネント名は **PascalCase**（例: `App`, `TaskItem`）とし、ファイル名もコンポーネント名と一致させる（例: `App.tsx`）。
- 1ファイル1コンポーネントを基本とする。
- コンポーネントは `function ComponentName() { ... }` の関数宣言 + `export default` で定義する（既存の `App` に合わせる）。
- Props の型は `interface ComponentNameProps { ... }` として定義する。
- ドメインの型（`Task` など）は PascalCase の単数名詞で定義する。
- イベントハンドラは、コンポーネント内のローカル関数は「動詞 + 対象」（例: `addTask`, `toggleTask`, `deleteTask`）、外部から渡す props としてのコールバックは `onXxx`（例: `onToggle`, `onDelete`）と命名する。
- コンポーネント用スタイルは同名の CSS ファイル（例: `App.tsx` → `App.css`）に定義し、`import './ComponentName.css'` で読み込む。

## Git 運用ルール

**コードに変更を加えたら、コミットしてそのつど GitHub にプッシュすること。** ローカルに変更がコミットされたまま、あるいはプッシュされないまま放置しない。

- 1つの意味のあるまとまりの変更ごとにコミットする（コミットメッセージは変更内容が分かるように簡潔に）。
- コミット後は必ず `git push` してリモート（GitHub）に反映する。作業の区切りごとにプッシュを後回しにしない。
- push 前に `git status` で意図しないファイル（secrets、ビルド成果物など）が含まれていないか確認する。
- force push（`git push --force`）や `git reset --hard` など破壊的な操作は、事前にユーザーの明示的な許可を得てから行う。
- 認証情報や `.env` など秘匿情報を含むファイルはコミットしない。`.gitignore` で除外する。

## ビルド・テストコマンド

- `npm install` — 依存関係のインストール
- `npm run dev` — 開発サーバー起動（http://localhost:5173）
- `npm run build` — 型チェック（tsc -b）+ 本番ビルド
- `npm run lint` — oxlint による静的解析
- `npm run preview` — ビルド成果物のプレビュー

## ディレクトリ構成

- `src/App.tsx` — タスクボードのメインコンポーネント（状態管理・追加/切り替え/削除ロジック、localStorage への永続化）
- `src/App.css` — タスクボードのスタイル
- `src/main.tsx` — エントリーポイント
- `.github/workflows/deploy.yml` — GitHub Pages への自動デプロイ設定

## GitHub Pages へのデプロイ

`main` ブランチに push すると GitHub Actions（`.github/workflows/deploy.yml`）が自動でビルドし、GitHub Pages に公開する。公開URLは「デプロイ先」を参照。

- `vite.config.ts` の `base` はリポジトリ名に合わせて `/task-board/` に設定済み。リポジトリ名を変更した場合は合わせて更新すること。
- 初回のみ、GitHub リポジトリの Settings → Pages → Build and deployment → Source を **GitHub Actions** に設定する必要がある。
