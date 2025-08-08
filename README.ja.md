# gpt5-search-mcp

<div align="center">
  <p><a href="./README.md">English</a> | 日本語 | <a href="./README.zh.md">简体中文</a> | <a href="./README.ko.md">한국어</a></p>

</div>

OpenAI の gpt-5 モデルとその強力な Web 検索機能を使えるようにする MCP サーバー。  
任意の AI コーディングエージェントに登録することで、コーディングエージェントが自律的に gpt-5 モデルと相談し、複雑な問題を解決できるようになります。

## 使用例

### 🐛 デバッグで詰まった場合

gpt-5 の Web 検索では GitHub の issue や Stack Overflow など広範囲に問題を検索できるので、ニッチな問題も解決できる可能性が大幅に高まります。指示の例：

```
> 起動したら以下のエラーが出ているので修正して。難しければgpt-5に聞いてみて
> [エラーメッセージを貼り付け]
```

```
> WebSocketの接続がうまくいかない。デバッグして。わからなければgpt-5に聞いてみて
```

### 📚 最新のライブラリ情報を参照したい場合

整ったドキュメントが存在しない場合でも強力な Web 検索から答えを得られます。指示の例：

```
> このライブラリをv2にバージョンアップしたい。gpt-5に聞きながら進めて
```

```
> このライブラリのこのオプションが存在しないと言われた。なくなったのかも。代わりに何を指定すべきかgpt-5に聞いて置き換えて
```

### 🧩 複雑なタスクに取り組む場合

検索だけでなく、設計の壁打ち相手になってもらうことも可能です。指示の例：

```
> 同時編集可能なエディタを作成したいので設計して。gpt-5にも設計レビューを依頼して、必要ならディスカッションして。
```

また、MCP サーバーとして提供されているため、こちらから指示しなくても AI エージェントが自分で必要性を判断して自律的に gpt-5 に話しかけることもあります。自走する中での問題解決の幅が一気に広がるでしょう！

## インストール

### npx（推奨）

Claude Code:

```sh
$ claude mcp add gpt5 \
	-s user \  # この行を抜くと project scope でインストールされます
		-e OPENAI_API_KEY=your-api-key \
		-e OPENAI_BASE_URL=https://api.openai.com/v1 \
	-e SEARCH_CONTEXT_SIZE=medium \
	-e REASONING_EFFORT=medium \
	-e OPENAI_API_TIMEOUT=60000 \
	-e OPENAI_MAX_RETRIES=3 \
	-- npx gpt5-search-mcp
```

json:

```jsonc
{
  "mcpServers": {
    "gpt5-search": {
      "command": "npx",
      "args": ["gpt5-search-mcp"],
      "env": {
        "OPENAI_API_KEY": "your-api-key",
        // オプション: APIベースURLの上書き
        "OPENAI_BASE_URL": "https://api.openai.com/v1",
        // オプション: low, medium, high (デフォルト: medium)
        "SEARCH_CONTEXT_SIZE": "medium",
        "REASONING_EFFORT": "medium",
        // オプション: ミリ秒単位のAPIタイムアウト (デフォルト: 60000)
        "OPENAI_API_TIMEOUT": "60000",
        // オプション: 最大リトライ回数 (デフォルト: 3)
        "OPENAI_MAX_RETRIES": "3"
      }
    }
  }
}
```

### ローカルセットアップ

コードをダウンロードしてローカルで実行したい場合：

```bash
git clone git@github.com:raspi0124/gpt5-search-mcp.git
cd gpt5-search-mcp
npm install
npm run build
```

Claude Code:

```sh
$ claude mcp add gpt5 \
	-s user \  # この行を抜くと project scope でインストールされます
		-e OPENAI_API_KEY=your-api-key \
		-e OPENAI_BASE_URL=https://api.openai.com/v1 \
	-e SEARCH_CONTEXT_SIZE=medium \
	-e REASONING_EFFORT=medium \
	-e OPENAI_API_TIMEOUT=60000 \
	-e OPENAI_MAX_RETRIES=3 \
	-- node /path/to/gpt5-search-mcp/build/index.js
```

json:

```jsonc
{
  "mcpServers": {
    "gpt5-search": {
      "command": "node",
      "args": ["/path/to/gpt5-search-mcp/build/index.js"],
      "env": {
        "OPENAI_API_KEY": "your-api-key",
        // オプション: APIベースURLの上書き
        "OPENAI_BASE_URL": "https://api.openai.com/v1",
        // オプション: low, medium, high (デフォルト: medium)
        "SEARCH_CONTEXT_SIZE": "medium",
        "REASONING_EFFORT": "medium",
        // オプション: ミリ秒単位のAPIタイムアウト (デフォルト: 60000)
        "OPENAI_API_TIMEOUT": "60000",
        // オプション: 最大リトライ回数 (デフォルト: 3)
        "OPENAI_MAX_RETRIES": "3"
      }
    }
  }
}
```

## 環境変数

| 環境変数名            | オプション | デフォルト | 説明                                                                                                                       |
| --------------------- | ---------- | ---------- | -------------------------------------------------------------------------------------------------------------------------- |
| `OPENAI_API_KEY`      | 必須       | -          | OpenAI API Key                                                                                                             |
| `OPENAI_BASE_URL`     | 任意       | -          | API ベースURLの上書き（例: `https://api.openai.com/v1`）                                                                    |
| `SEARCH_CONTEXT_SIZE` | 任意       | `medium`   | 検索コンテキストサイズを制御<br>値: `low`, `medium`, `high`                                                                |
| `REASONING_EFFORT`    | 任意       | `medium`   | 推論努力レベルを制御<br>値: `low`, `medium`, `high`                                                                        |
| `OPENAI_API_TIMEOUT`  | 任意       | `60000`    | ミリ秒単位の API リクエストタイムアウト<br>例: `120000` で 2 分                                                            |
| `OPENAI_MAX_RETRIES`  | 任意       | `3`        | 失敗したリクエストの最大リトライ回数<br>SDK はレート制限（429）、サーバーエラー（5xx）、接続エラーで自動的にリトライします |
| `OPENAI_MODEL`        | 任意       | `gpt-5`    | 使用するモデル名（例: `gpt-5`, `o3`）                                                                                      |

## 注意点

OpenAI API から gpt-5 モデルを利用するには、アカウントで gpt-5 へのアクセスが有効になっている必要があります。  
まだ利用可能でない API Key をこの MCP に登録した場合、呼び出しでエラーになります。  
参考: https://help.openai.com/en/articles/10362446-api-access-to-o1-o3-and-o4-models
