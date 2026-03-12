# cmx helper

`cmx` は cmux を **2行 x 3列（合計6ペイン）で均等サイズ**にして起動するためのスクリプトです。

今回は「うまくいかない」ケースを減らすために、以下を強化しています。

- 起動直後のフォーカス中surfaceを取得して split（`surface:1` 固定依存を撤廃）
- セッションファイル生成待機を追加
- 起動時に cmux ウィンドウを画面いっぱいに配置（通常ウィンドウ最大化）

## 使い方

```bash
./cmx
```

エイリアスで使う場合:

```bash
chmod +x /path/to/cmx
ln -s /path/to/cmx /usr/local/bin/cmx
cmx
```

## 動作手順（スクリプト内）

1. 既存の cmux を終了
2. セッションファイルを削除
3. cmux を起動
4. （既定）cmux ウィンドウを最大化
5. 3列 x 2行（計6ペイン）を作成
6. 一度終了してセッションの `dividerPosition` を `1/3` に書き換え
7. 再起動して均等6ペインを反映

## オプション

- `CMX_MAXIMIZE_WINDOW=1`（既定）: 起動時にウィンドウ最大化
- `CMX_MAXIMIZE_WINDOW=0`: 最大化せず実行

例:

```bash
CMX_MAXIMIZE_WINDOW=0 cmx
```

## 前提

- macOS
- `/Applications/cmux.app` が存在
- `cmux` CLI が利用可能（PATH にあること）
- `osascript`, `python3` が利用可能

## トラブルシュート

- `必要なコマンドが見つかりません`:
  - `cmux` / `osascript` / `python3` が PATH にあるか確認
- `cmuxソケット待機がタイムアウトしました`:
  - cmux 単体起動で正常に立ち上がるか確認
- `初期surfaceの取得に失敗しました`:
  - 起動直後の内部状態が不安定。cmuxを完全終了して再実行
- `セッションファイル生成待機がタイムアウトしました`:
  - cmux終了時にセッション保存が行われていない可能性。権限やディスク状態を確認
