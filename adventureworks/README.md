# 開発セットアップ手順

## 🍺 1. 前提：uv のインストール（初回のみ）

```bash
brew install uv
uv --version
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

## 📦 3. DBT 操作

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

## 🔀 4. Git 操作フロー

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
