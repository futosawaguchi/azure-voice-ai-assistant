# Azure Voice AI Assistant

Azure OpenAI を使ったリアルタイム音声会話アシスタントです。  
マイクに話しかけるだけで、AIが音声で返答します。

---

## 機能

| 機能 | 説明 |
|------|------|
| 🎤 リアルタイム音声認識 | Azure OpenAI Whisper による日本語認識 |
| 🤖 AI応答生成 | Azure OpenAI GPT による会話 |
| 🔊 音声合成（TTS） | Azure OpenAI TTS による音声出力 |
| ⏸️ 発話完結判定 | 「えっと」「しかし、」などを検出し続きを待機 |
| 🗣️ バージイン対応 | AI発話中にユーザーが割り込んで話せる |
| 💬 会話履歴保持 | 文脈を維持した会話が可能 |

---

## 使用技術

| カテゴリ | 技術 |
|---------|------|
| 音声認識（STT） | Azure OpenAI / gpt-4o-transcribe |
| AI応答（Chat） | Azure OpenAI / GPT |
| 音声合成（TTS） | Azure OpenAI / gpt-4o-mini-tts |
| 発話区間検出 | WebRTC VAD (`webrtcvad`) |
| 音声入出力 | `sounddevice` |
| 音声デコード | `pydub` + ffmpeg |
| 言語 | Python 3.11.7 |

---

## セットアップ（Mac）

### 必要なもの

- Python 3.11.7
- Azure OpenAI リソース（STT・Chat・TTS のデプロイ）
- ffmpeg

```bash
brew install ffmpeg
```

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/futosawaguchi/azure-voice-ai-assistant.git
cd azure-voice-ai-assistant

# 仮想環境を作成・有効化
python -m venv venv
source venv/bin/activate

# 依存パッケージをインストール
pip install -r requirements.txt
```

### 環境変数の設定

```bash
cp .env.example .env
```

`.env` を開いて以下を記入：

```
AZURE_API_KEY=your_azure_api_key_here
AZURE_BASE_URL=https://your-resource-name.openai.azure.com
AZURE_STT_DEPLOY=your_stt_deployment_name
AZURE_CHAT_DEPLOY=your_chat_deployment_name
AZURE_TTS_DEPLOY=your_tts_deployment_name
```

### 実行

```bash
python main.py
```

---

## 調整パラメータ

`main.py` 内の以下の値で挙動を調整できます：

| パラメータ | デフォルト | 説明 |
|-----------|-----------|------|
| `silence_sec` | `0.75` | 無音がこの秒数続いたら発話終了とみなす |
| `extra_wait_sec` | `1.0` | 未完結と判定した場合の追加待機秒数 |
| `max_retry` | `2` | 完結判定のリトライ上限回数 |
| `VAD_MODE` | `2` | 0〜3（大きいほど無音判定が積極的） |

---

## 注意事項

- `.env` ファイルは絶対にGitにコミットしないでください

---