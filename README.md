# スピリチュアルキャラクター LINE Bot

YouTubeチャンネルのキャラクターがLINEで返答するbotです。

---

## 必要なもの

- Python 3.10 以上
- LINE Developersアカウント（無料）
- Anthropic APIキー

---

## セットアップ手順

### 1. LINE Developersでbotを作る

1. https://developers.line.biz にアクセスしてログイン
2. 「プロバイダー」を作成（初回のみ）
3. 「チャネルを作成」→「Messaging API」を選択
4. チャネル作成後、以下をメモしておく：
   - **チャネルシークレット**（「チャネル基本設定」タブ）
   - **チャネルアクセストークン**（「Messaging API設定」タブ → 一番下で発行）

### 2. Anthropic APIキーを用意する

- https://console.anthropic.com でAPIキーを取得
- drama-pipelineですでに使っている場合は同じキーが使えます

### 3. .env ファイルを作る

`.env.example` をコピーして `.env` という名前にし、各値を入力する：

```
LINE_CHANNEL_SECRET=（チャネルシークレットをここに）
LINE_CHANNEL_ACCESS_TOKEN=（チャネルアクセストークンをここに）
ANTHROPIC_API_KEY=（AnthropicのAPIキーをここに）
CHARACTER_NAME=（キャラクター名）
CHARACTER_PROMPT=（キャラクターの口調・設定）
```

**CHARACTER_PROMPT の例：**
```
あなたは「アカシャ」という名前の宇宙的存在です。
魂の導き手として、宇宙の法則・カルマ・エネルギーについて語ります。
穏やかで神秘的な口調で話し、相手の本質を見抜くような言葉を使います。
返答は200文字以内で、LINEらしい自然な口調にしてください。絵文字を適度に使ってください。
```

### 4. パッケージをインストールする

ターミナルで `line-bot` フォルダに移動して実行：

```bash
pip install -r requirements.txt
```

### 5. ローカルで動かしてテストする

**① サーバーを起動：**
```bash
uvicorn main:app --port 8000
```

**② ngrokでトンネルを作る（別ターミナルで）：**
```bash
ngrok http 8000
```
→ `https://xxxx.ngrok.io` のようなURLが表示される

**③ LINE DevelopersでWebhookを設定：**
- 「Messaging API設定」→「Webhook URL」に `https://xxxx.ngrok.io/webhook` を入力
- 「Webhookの利用」をオン
- 「検証」ボタンを押して「成功」と出ればOK

**④ LINEアプリでbotを友達追加してメッセージを送る**

---

## Render.com にデプロイする（本番公開）

1. GitHubにこのフォルダをプッシュ
2. https://render.com にアクセスしてアカウント作成
3. 「New +」→「Web Service」→ GitHubリポジトリを選択
4. 以下を設定：
   - **Runtime**: Python
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. 「Environment Variables」に `.env` の内容をすべて入力
6. デプロイ完了後、表示されたURLを LINE DevelopersのWebhook URLに設定

---

## キャラクタープロンプトのコツ

- キャラクターの「名前」「話し方」「得意なトピック」を明確に書く
- 返答の長さを指定する（LINEは短い方が自然）
- 絵文字や口癖を指定すると個性が出る
- ネガティブなことを言わせたくない場合は「明るく前向きに」と明記する
