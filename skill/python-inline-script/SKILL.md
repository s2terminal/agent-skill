---
name: python-inline-script
description: 'Pythonのプログラムやスクリプトを新規作成するときに、通常のPythonプロジェクトかinline script metadata形式かを確認する。単一ファイル形式が選ばれた場合は、PEP 723の依存関係を埋め込み、uvで起動できるPythonスクリプトを作成する。既存のinline metadata scriptの修正にも使う。'
---

# Pythonの単一ファイルスクリプト

## 新規作成時の形式の確認

Pythonのプログラムを新規作成するとき、実装やファイル生成の前に次の質問をする。

「通常のPythonプロジェクト（`pyproject.toml`で依存関係を管理）と、
inline metadata script（依存関係を埋め込んだ単一の`.py`ファイル）のどちらで作成しますか？」

- 形式が未指定の場合は、ユーザーの回答を待つ。規模や依存関係の有無だけで形式を決めない。
- ユーザーがすでに形式を明示している場合は、その指定を回答として扱い、再質問しない。
- 通常のPythonプロジェクトが選ばれた場合は、通常のプロジェクト構成で作成する。
- inline metadata scriptが選ばれた場合は、以降の手順に従う。
- 既存プロジェクト内のモジュール追加や既存ファイルの修正では、新規の独立したプログラムを作るのでなければ形式を再質問しない。
  既存の構成を尊重し、依頼なしに形式を変換しない。

## 単一ファイルに含める内容

- 処理と依存関係の宣言を1つの`.py`ファイルに含める。
  実行に別のローカルPythonモジュールや依存関係定義ファイルを必要としない構成にする。
- ファイル先頭付近にPEP 723の`# /// script`から`# ///`までのコメントブロックを置く。
  ブロック内は各行を`# `で始めた有効なTOMLとして記述する。
- `requires-python`に実装と依存パッケージが対応するPythonバージョン条件を指定する。
- `dependencies`に必要な外部パッケージの配布名と必要なバージョン条件を列挙する。
  標準ライブラリは含めず、外部依存がなければ`dependencies = []`を明記する。
- `requirements.txt`、`pyproject.toml`、事前の`pip install`、起動時の`--with`指定に依存しない。
  スクリプト内からパッケージをインストールしない。
- 単一ファイルでの配布を保つため、依頼がなければ別のロックファイルは作らない。
- 入力データや認証情報が必要なら、引数や環境変数で受け取る。
  「ファイル単体」とはコードと依存関係宣言が完結することを意味し、パッケージ本体の同梱を意味しない。

## 作成と起動

標準ライブラリだけを使う最小例を示す。Pythonバージョンと処理は依頼に合わせて変更する。

```python
# /// script
# requires-python = ">=3.12"
# dependencies = []
# ///

def main() -> None:
    print("Hello, world!")


if __name__ == "__main__":
    main()
```

外部依存がある場合は、例えば`dependencies = ["rich>=13,<15"]`のように宣言し、
実際に必要なパッケージと互換性のある条件に置き換える。

uvを使って雛形や依存関係を編集する場合は、次のコマンドを使える。
雛形生成は新規ファイルにのみ行う。

```bash
uv init --script example.py --python 3.12
uv add --script example.py 'rich>=13,<15'
```

起動コマンドとして、実際のファイル名を使って次の形式を提示する。

```bash
uv run --script example.py
```

uvのインストールを前提とする。初回起動時には依存パッケージや、
必要に応じてPythonのダウンロードが発生する。
inline metadataがあるスクリプトでは、uvがスクリプト用の依存環境を管理する。

Unix系環境で直接実行も求められた場合は、ファイルの1行目に
`#!/usr/bin/env -S uv run --script`を追加し、`chmod +x example.py`で実行権限を付ける。
その場合は`./example.py`でも起動できる。

## 完了前の確認

1. metadataが有効なTOMLで、必要な外部依存とPythonバージョン条件が宣言されていることを確認する。
2. スクリプトを一時ディレクトリに単体でコピーし、安全な入力で`uv run --script`による起動と期待する動作を確認する。
   外部への書き込みなどを伴う場合は、その操作を避けられる引数や検証方法を使う。
3. 完了時にはファイルの場所と起動コマンドを伝える。
   uvが利用できないなどの理由で実行確認できなかった場合は、未検証の内容を明記する。

## 仕様の参照先

- [Inline script metadata仕様](https://packaging.python.org/en/latest/specifications/inline-script-metadata/)
- [uvのスクリプト実行ガイド](https://docs.astral.sh/uv/guides/scripts/)
