# API設計書

## TabPFNとは

TabPFNは、既存の表形式データに対して次のタスクを可能にするツール

- **回帰** 連続値の予測。

1. **トレーニング済みで即時利用:** TabPFNは既に幅広いデータっセットでの学習を完了しており、ユーザーが追加トレーニングを行う必要はない。
2. **少量データへの対応** データセットが小さい場合でも堅牢で、一貫性のある結果を提供。
3. **迅速な計算:** 多くの機械学習方法と比較して、学習・推論が高速。

## TabPFN API 概要

### 提供する機能

- **データ前処理:** 欠陥値処理、カテゴリ変数のエンコーディングなどを自動化。
- **予測:** 入力データに基づき、ラベルまたは目標値を予測。
- **結果の可視化:** モデルのパフォーマンスや予測結果を可視化する機能を実装

### 実装例
1. **ビジネス予測:** 売上予測・需要予測
2. **健康分析:** 疾病予測や患者データ分析。
3. **教育:** 試験成績や卒業率の予測。
4. **リスク評価:** 保険や金融におけるリスクアセスメント。

## 2. 用語定義
- 本ドキュメントで使用する用語の説明
  - 目的変数
  - 説明変数
  - テストデータ割合 

## 3. エンドポイント一覧
| パス         | メソッド | 概要           |
|--------------|----------|----------------|
| /predict     | POST     | 回帰予測実行   |
| ...          | ...      | ...            |

| パス         | メソッド | 概要           |
|--------------|----------|----------------|
| /status     | GET     | ステータスチェック   |
| ...          | ...      | ...            |

## 4. 各エンドポイント詳細

### 4.1 /predict

#### 概要
- TabPFNで回帰分析を行い、予測値と評価指標を返す。

#### リクエスト

- Content-Type: application/json
- パラメータ一覧

|名前    |型    |必須    |説明   |
|-------|------|-------|-------|
|table_data|array|○|テーブルデータ(2次元配列)|
|target_column|string|○|目的変数のカラム名|
|feature_columns|array|○|説明変数のカラムリスト|
|test_size|float|○|テストデータに使用する割合|

- サンプルリクエスト

```
{
    "table_data": [
        {"age": 23, "height": 170, "weight": 65},
        {"age": 25, "height": 180, "weight": 80}
    ],
    "target_column": "weight",
    "feature_columns": ["age","height"],
    "test_size": 0.2
}
```

#### レスポンス

- Content-Type: application/json
- 各フィールドの説明

    |名前    |型    |説明   |
    |-------|------|-------|
    |predictions|array|予測値リスト|
    |metrics|object|評価指標(RMSE,MAE等)|

- サンプルレスポンス

    ```
    {
        "predictions": [67.5, 79.2],
        "metrics": {
            "rmse": 2.1,
            "mae": 1.8
        }
    }
    ```

#### エラー

|ステータス|内容例|
|-----|-----|
|400|入力不正値|
|500|サーバー内部エラー|

サンプルエラーレスポンス

```
{
    "detail": "Invalid input: test_size must be between 0 and 1."
}
```

## 5. データスキーマ
- リクエスト/レスポンスのデータ構造

リクエストボディ定義例(JSON Schema風)

```
{
    "type": "object",
    "properties": {
        "table_data": {
            "type": "array",
            "items": {"type": "object"}
        },
        "target_column": {"type": "string"},
        "feature_columns": {
            "type": "array",
            "items": {"type": "string"}
        }, 
        "test_size": {"type": "number", "minimun": 0, "maximum": 1}
    },
    "required": ["table_data", "target_column", "feature_columns", "test_size"]
}
```

レスポンスボディ定義例(JSON Schema風)

```
{
  "type": "object",
  "properties": {
    "predictions": {
      "type": "array",
      "items": { "type": "number" }
    },
    "metrics": {
      "type": "object",
      "properties": {
        "rmse": { "type": "number" },
        "mae": { "type": "number" }
      }
    }
  }
}
```

## 6. 認証・認可
- なし

## 7. ステータスコード一覧

|コード|意味|
|-----|-----|
|200|正常|
|400|入力不正|
|500|サーバー内部エラー|

## 8. その他
- レートリミットなし
- ログ出力あり
