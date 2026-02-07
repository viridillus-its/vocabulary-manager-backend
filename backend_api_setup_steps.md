# TabPFN回帰分析API バックエンド作成手順

## 1. 要件整理・設計
- 入力データ形式（CSV, JSON等）の決定
- APIエンドポイント設計（例: `/predict`）
- 入力（テーブルデータ、目的変数、説明変数、テストデータ割合）のバリデーション仕様決定
- 出力（予測値、評価指標等）の形式決定

## 2. 環境構築
- Python仮想環境の作成（venv, poetry, conda等）
- 必要パッケージのインストール
  - fastapi, uvicorn, pandas, numpy, tabpfn, scikit-learn など

## 3. ディレクトリ・ファイル構成の設計
- APIエンドポイント用ファイル（例: `api/endpoints.py`）
- サービス層（例: `services/predict.py`）
- モデル・スキーマ定義（例: `models/request.py`, `models/response.py`）
- ユーティリティ（例: `utils/`）

## 4. モデル実装
- TabPFNによる回帰分析ロジックの実装
- 入力データの前処理、学習、予測、評価指標計算

## 5. API実装
- FastAPI等でエンドポイント作成
- リクエスト受信→バリデーション→サービス層呼び出し→レスポンス返却

## 6. テスト
- 単体テスト、統合テストの作成（pytest等）
- APIの動作確認（curl, httpie, Postman等）

## 7. ドキュメント整備
- READMEやAPI仕様書の作成
- OpenAPI（Swagger）自動生成確認

## 8. デプロイ・運用準備
- requirements.txtやDockerfileの整備
- 本番・開発環境の切り替え設計
