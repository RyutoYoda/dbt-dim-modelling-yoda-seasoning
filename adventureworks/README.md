# 開発セットアップ手順

## 🍺 1. 前提：uv と DuckDB のインストール（初回のみ）

```bash
brew install uv
uv --version

brew install duckdb
```

---

## 🐍 2. Python 仮想環境の作成（uv 使用）

```bash
# 仮想環境を .env ディレクトリに作成
uv venv .env

# 仮想環境を有効化（Unix/macOS）
source .env/bin/activate

# パッケージの同期（requirements.txt から）
uv pip sync requirements.txt
```

> 💡 `requirements.txt` を使わず `pyproject.toml` を使う場合も自動対応可能です。

---

## 🦆 3. DuckDB の設定（dbt 用）

`profiles.yml` に以下を設定：

```yaml
adventureworks:
  target: duckdb
  outputs:
    duckdb:
      type: duckdb
      path: target/adventureworks.duckdb
      threads: 12
```

> 💡 `profiles.yml` は通常 `~/.dbt/profiles.yml` に置きます。

---

## 📦 4. DBT 操作

### インストール済みパッケージの取得

```bash
dbt deps
```

### モデルの実行

```bash
dbt build   # run + test + snapshot など全部入り
# または
dbt run     # run だけ
```

### テストの実行

```bash
dbt test
```

### スナップショットの生成

```bash
dbt snapshot
```

### ドキュメント生成と表示

```bash
dbt docs generate
dbt docs serve
```

---

## 🔎 5. DuckDB の内容を確認する

```bash
duckdb target/adventureworks.duckdb

-- SQLプロンプトが開いたら、テーブル一覧を確認
.tables

-- 任意のテーブルの中身を確認
SELECT * FROM your_table_name LIMIT 10;
```

> 💡 dbt run 後に `target/` 配下の DB ファイルを開いて、データが入っているか確認できます。

---

## 🔀 6. Git 操作フロー

### リポジトリをクローン

```bash
git clone <リポジトリURL>
```

### 新しいブランチを作って切り替え

```bash
git switch -c mybranch
```

### 現在のブランチを確認

```bash
git branch
```

### 変更をコミット

```bash
git add .
git commit -m "ここにコミットメッセージ"
```

### main ブランチの変更を取り込む（安全に）

```bash
git fetch origin
git merge origin/main
```

### 作業ブランチをリモートに push

```bash
git push origin mybranch
```

---

## ✅ 備考

* `.env` ディレクトリは `.gitignore` に含めるのが正解
* `uv pip sync` は `pip install -r requirements.txt` より高速&正確
* Poetry や Pipenv を使わない場合も `uv` だけで十分
* DuckDB はシングルバイナリで軽量、ローカル検証にも最適
* `.duckdb` ファイルは SQLite っぽく直接開いて中身を確認可能
