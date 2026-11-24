# book-llm-from-scratch — Claude Code 運用メモ

書籍『[作ってわかる大規模言語モデルの仕組み](https://www.amazon.co.jp/dp/4296205250/)』（日経BP、2026年）の公式サポートリポジトリ `elith-co-jp/book-llm-from-scratch` のフォーク（写経・実験用）。Python（uv 管理）。

## 構成

| パス | 役割 |
|---|---|
| `llm_from_scratch/transformer/` | Transformer実装（`attention.py` / `transformer.py` / `utils.py`）。2章ノートブックから参照 |
| `llm_from_scratch/gpt/` | GPT実装（`model.py` / `dataset.py` / `tokenizer.py` / `trainer.py`）。実装済みだが現状どの notebook からも未参照（chapter03 は GPT を notebook 内に自己完結で再定義している） |
| `notebooks/chapter02/`〜`chapter05/` | 各章のノートブック・スクリプト（章構成は README 参照） |
| `notebooks/chapter04/*.py` | 4章の分散学習スクリプト（`torchrun` / `deepspeed` 起動、notebookではない） |
| `refs/errors.md` | 書籍の正誤表 |

## コマンド

```bash
uv sync                    # 依存関係インストール
uv run jupyter notebook    # ノートブック起動（各章の .ipynb を開いて対話実行）
uv run torchrun notebooks/chapter04/section02_data_parallel.py   # 4.2 データ並列（例）
uv run torchrun notebooks/chapter04/section03_train_gpt2.py      # 4.3 GPT-2事前学習（例。deepspeed起動も可）
```

テスト未整備（`pyproject.toml` に `pytest` は dev-dependencies として入っているが、`tests/` 配下・テストファイルは存在しない）。

## 注意

- 書籍写経リポなので大きな再構成はしない。章立て（`notebooks/chapterNN/`）とコード品質（学習用途、本番品質ではない）は原著の構成に合わせる
- `upstream` remote（`elith-co-jp/book-llm-from-scratch`）が設定済み。原著への追従・差分確認は `git fetch upstream` を使う
- notebook は Google Colab でも実行可能な単体完結が前提。ローカル実行時は `uv run jupyter notebook` 経由でカーネルを合わせる
- `models/` `data/` `*.pt` `*.pth` `*.pkl` `wandb/` は `.gitignore` 対象。学習済みモデル・データセットはコミットしない

<!-- common-core:v1 -->
## 共通コア（携行版・全リポ共通の死守ルール）
<!-- ~/.claude/templates/common-core.md から repo-bootstrap.sh が配布・更新する。手動編集しない -->

- コミットメッセージは日本語・Conventional Commits（`feat:` `fix:` `docs:` `refactor:` `chore:` 等）
- `git push`・PR の作成/マージ/Ready 化は、実行前に必ず人間の確認を取る
- Issue 起点の実装は人間が付けた `approved` ラベルを確認してから着手する（無ければ依頼して停止）。1 Issue = 1 ブランチ = 1 PR、PR 本文に `Closes #N`
- テストは実際に実行し、実出力で green を確認してから完了と報告する（「通るはず」で済ませない）
- テストや設定を緩めて green にしない。落ちたらコードを直す（テストが間違っているときを除く）
- 推測と事実を分ける。実測・根拠で確認していないことは「〜のはず」と明示し断定しない
- シークレット（API キー・トークン・`.env` の値）を stdout・ログ・コミットに出さない
- 日本語の句読点は「。」「、」（コンマ・ピリオドは使わない）
<!-- /common-core -->
