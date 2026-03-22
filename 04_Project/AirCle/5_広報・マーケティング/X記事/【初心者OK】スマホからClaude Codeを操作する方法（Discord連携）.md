## スマホからAIに「開発しろ」って指示できるの、知ってましたか？

正直、これを知ったとき震えました。

外出先で、スマホのDiscordからメッセージを1つ送る。するとPCで Claude Code が勝手に動き出して、コードを書いて、ファイルを作って、GitHub に Push する。

**自分はスマホ片手にカフェでコーヒー飲んでるだけ。**

これ、SF映画じゃなくて、今すぐできます。

**Claude Code Channels** という機能を使います。

ただし。

この機能、まだ **Research Preview（実験機能）** です。普通にやっても動かないことがめちゃくちゃ多い。ネットの記事通りにやっても「なんで動かないの？」ってなります。

僕も最初ハマりまくりました。

**だからこの記事では、動かなくてハマるポイントまで全部書きます。**

この記事を上から順にやれば、スマホからClaude Codeを遠隔操作できる環境が完成します。

---

## そもそも Claude Code Channels って何？

めちゃくちゃ簡単に言うと、

**Discord や Telegram を、Claude Code のリモコンにできる機能**

です。

仕組みはこう。

```
スマホ（Discord）
  ↓
Discord Bot
  ↓
Claude Code plugin
  ↓
PCでAIが作業
```

Discordでメッセージを送ると、それがClaude Codeのセッションに転送されて、Claudeが処理して返信してくれる。

つまり、**Discord = Claude Codeのリモコン** になります。

---

## これで何ができるの？

例えば、スマホから一言送るだけで

- コード生成・修正
- サーバーのログ解析
- GitHub操作（Push、PR作成）
- エージェントタスクの実行
- リサーチ依頼

ができます。

Claude Codeを **「AIエージェントサーバー」** として使えるようになる、ということです。

**布団の中からAIに仕事させることも可能です。** 実際やってます。

---

## 用意するもの

設定を始める前に、以下を揃えてください。

| 必要なもの | 備考 |
| --- | --- |
| Claude Code | バージョン 2.1.80 以上 |
| Discord アカウント | 持ってない人はほぼいないと思いますが |
| Discord サーバー | 自分専用のサーバーを1つ作ると楽 |
| Bun | JavaScriptランタイム。**これが一番ハマるポイント。後で詳しく説明します。** |

---

## ステップ1：Claude Code のバージョンを確認

まずここから。ターミナルで以下を実行。

```
claude --version
```

**2.1.80 以上** であればOK。

古い場合は

```
claude update
```

でアップデートしてください。これが古いと全部動きません。

---

## ステップ2：Discord Bot を作成する

ここからがメインです。順番通りにやってください。

### 2-1. Discord Developer Portal にアクセス

https://discord.com/developers/applications

### 2-2. 新しいアプリを作成

右上の **「New Application」** をクリック。名前は何でもOK。「ClaudeCode」とかで大丈夫です。

### 2-3. Bot を追加

左メニューの **「Bot」** をクリック。

### 2-4. Bot トークンを発行

**「Reset Token」** をクリック。

表示されたトークンをコピーします。

**⚠️ このトークンは絶対に外部に公開しないでください。** 公開したら誰でもあなたのBotを操作できるようになります。

### 2-5. Bot の重要設定（ここ見落とすと動かない）

同じページに **「Privileged Gateway Intents」** があります。

**以下をすべてON**にしてください。

- Presence Intent ✅
- Server Members Intent ✅
- **Message Content Intent** ✅

**特に Message Content Intent。これがOFFだと、Botはメッセージの中身を取得できません。** ここを見落として「なんで動かないの？」ってなる人、めちゃくちゃ多いです。

### 2-6. Bot を Discord サーバーに招待

左メニューの **「OAuth2」** をクリック。

**スコープ**で以下にチェック：

- `bot`

**Bot権限**で以下をON：

| 権限 | 必要な理由 |
| --- | --- |
| メッセージを送る | Claude Codeの返信を送るため |
| 公開スレッドを作成 | スレッド形式で会話するため |
| プライベートスレッドを作成 | 同上 |
| スレッドでメッセージを送る | 同上 |
| メッセージ履歴を読む | 会話の文脈を把握するため |
| リンクを埋め込む | コード結果のリンクを共有するため |
| ファイルを添付 | ファイル出力を共有するため |
| リアクションを付ける | 処理状態の表示 |

ページ下部にURLが生成されるので、そのURLを開いてBotをサーバーに追加します。

---

## ステップ3：Bun をインストール（ここでハマる人、多い）

**Bun って何？** って思いますよね。

Bun は **高速 JavaScript ランタイム** です。Node.js の代替として作られた環境で、Claude Code の Discord plugin は **Bun の上で動きます**。

つまり、**Bun が入ってないと、Claude Code と Discord が通信できない。**

ここでハマって「なんで動かないの？」ってなる人、本当に多いです。

### インストール方法

まず確認。

```
bun --version
```

バージョンが表示されたらOK。表示されない場合：

```
curl -fsSL https://bun.sh/install | bash
```

インストール後：

```
source ~/.zshrc
```

もう一度確認。

```
bun --version
```

これでバージョンが表示されればOKです。

---

## ステップ4：Claude Code で Discord plugin を設定

### 4-1. Claude Code を起動

```
claude
```

### 4-2. plugin をインストール

Claude Codeの画面で以下を実行。

```
/plugin install discord@claude-plugins-official
/reload-plugins
```

### 4-3. トークンの設定（ここも注意）

本来なら

```
/discord:configure TOKEN
```

で設定できるはずなんですが、**実際にやると**

```
Unknown skill: discord:configure
```

**になることがあります。**

これは plugin のバージョン差異が原因です。

### 確実な解決方法

トークンを環境変数で設定します。ターミナルで：

```
export DISCORD_BOT_TOKEN="ここにステップ2でコピーしたトークンを貼る"
```

---

## ステップ5：Claude Code Channels を起動

以下のコマンドで起動します。

```
claude --channels plugin:discord@claude-plugins-official
```

これで Claude Code + Discord plugin が起動します。

---

## ステップ6：ペアリング

ここでDiscord側の操作をします。

### 6-1. Bot に DM を送る

Discord で、作成した Bot に **DM** を送ります。

```
hello
```

すると **pairing code（ペアリングコード）** が返ってきます。

### 6-2. Claude Code でペアリング

Claude Code 側で：

```
/discord:access pair ここにペアリングコードを入力
```

### 6-3. セキュリティロック（必ずやる）

**これ絶対にやってください。**

```
/discord:access policy allowlist
```

これで **自分以外は使えなくなります**。やらないと、Botに話しかけた人が誰でもあなたのClaude Codeを操作できるようになります。怖すぎます。

---

## ステップ7：サーバーのチャンネルでも使えるようにする

DM だけじゃなく、サーバーのチャンネルでも使いたいですよね。

### 7-1. Discord で開発者モードをON

設定 → アプリの設定 → 詳細設定 → 開発者モード → ON

### 7-2. チャンネル ID を取得

使いたいチャンネルを右クリック → 「IDをコピー」

### 7-3. チャンネルを登録

Claude Code で：

```
/discord:access channel add チャンネルID
```

### メンションなしでも反応させたい場合

```
/discord:access group rm チャンネルID
/discord:access group add チャンネルID --no-mention
```

これで、チャンネルに書き込むだけでClaude Codeが反応するようになります。

---

## よくあるトラブルと解決方法

### Bot が返信しない

原因は大体この4つのどれかです。

| 原因 | 解決方法 |
| --- | --- |
| Claude Code が停止している | ターミナルを確認。起動し直す |
| Message Content Intent が OFF | Discord Developer Portal で ON にする |
| Bun がインストールされていない | `bun --version` で確認 |
| ペアリングが完了していない | DM で hello → コード取得 → pair |

### OpenClaw と Claude Code が同じチャンネルで両方反応する

これ、実際にハマる人多いです。

**原因**：Bot が同じチャンネルを監視している

**解決方法**：チャンネルを分ける

```
#openclaw → OpenClaw の Bot のみ
#claudecode → Claude Code の Bot のみ
```

権限設定でBotごとにチャンネルを分ければ解決です。

---

## 常時起動したい場合

Claude Code は **ターミナルを閉じると停止します**。

「24時間動かしたい」って人は、以下の方法があります。

### 方法1：tmux を使う

```
tmux
claude --channels plugin:discord@claude-plugins-official
```

tmux のセッションを閉じても、バックグラウンドで動き続けます。

### 方法2：VPS を使う

AWS、GCP、さくら VPS など。サーバー上で常時起動させる方法です。

### 方法3：Mac mini を常時起動

僕はこれでやってます。Mac mini を自宅に置いて、24時間つけっぱなし。ここに Claude Code を常駐させてます。

---

## Telegram でも使える

Discord じゃなくて Telegram 派の人はこっち。

```
/plugin install telegram@claude-plugins-official
```

起動：

```
claude --channels plugin:telegram@claude-plugins-official
```

設定の流れは Discord とほぼ同じです。

---

## まとめ：設定の重要ポイント

ここまで長かったですが、やることをまとめると6つです。

| 順番 | やること |
| --- | --- |
| 1 | Claude Code が 2.1.80 以上か確認 |
| 2 | Bun をインストール |
| 3 | Discord Bot を作成してトークンを取得 |
| 4 | トークンを環境変数で設定 |
| 5 | DM でペアリング |
| 6 | allowlist でセキュリティロック |

これで完成するのが、

```
スマホ（Discord）
  ↓
Discord Bot
  ↓
Claude Code
  ↓
PCでAIが作業
```

という **「AIエージェントの遠隔操作環境」** です。

カフェで、電車で、布団の中で。スマホから一言送るだけで、PCのAIが動き出す。

**マジで未来が来てます。**

設定自体は30分あれば終わります。ハマるポイントは全部この記事に書いたので、上から順にやれば大丈夫です。

ぜひやってみてください。

---

## Claude Code Channels「だけ」だと、正直もったいない

ここまで読んで、Claude Code Channelsの設定はできたと思います。

でも、正直に言います。

**Claude Code Channels単体で使っても、そこまで変わらない。**

「スマホからAIに指示できる、すごい！」で終わるんです。

本当にヤバいのは、**OpenClaw × Claude Code Channels** の組み合わせです。

OpenClawには **永続メモリ** があります。あなたの判断基準、過去のやりとり、事業の情報——全部覚えてる。

つまり、スマホから「いつものやつやっといて」って一言送るだけで、**あなたの文脈を完全に理解したAIが動く**。

Claude Code Channelsだけだと「賢いAIに毎回ゼロから指示する」。
OpenClaw × Claude Code Channelsだと「あなた専用のAI従業員に一言で仕事を振る」。

**この差はデカすぎる。**

しかもOpenClawはオープンソースだから、特定のサービスに依存しない。Claude Code Channelsが仮になくなっても、TelegramでもLINEでもDiscordでも、**自分で接続先を変えられる**。

だから僕は、Claude Code単体じゃなくて、**OpenClawをベースにした上でClaude Codeを活用する** というスタイルを推してます。

### OpenClawの情報、1人で追うのキツくないですか？

ただ、OpenClawは進化がとにかく速い。毎週のようにアップデートが入るし、使い方もどんどん変わる。

**1人で追うのは正直キツい。**

だから、僕が運営している **OpenClawギルド（オープンチャット）** に来てください。

OpenClawの最新情報、活用事例、つまずいたときの質問——全部ここに集まってます。初心者の方もめちゃくちゃ多いので、「こんなこと聞いていいのかな」は一切気にしなくて大丈夫です。

**無料です。** 参加はこちらから👇
https://line.me/ti/g2/yqJWRJmOhKo3INXYGmnvZGRZuyYEfTrHatnwyw

---

## 【明日開催】OpenClaw vs Claude Code Channels セミナー（完全無料）

ちなみに、この記事を読んで「Claude Code ChannelsとOpenClawってどう使い分けるの？」って思った方。

**明日、まさにそのテーマのセミナーをやります。**

📅 **3/10（月）20:00〜22:00（120分）**
🎥 オンライン（無料・Zoom）

いち × ユニコ🦄さんの2時間コラボセミナーです。

内容はこんな感じ👇

- なぜOpenClawなのか？なぜClaude CodeではなくOpenClawなのか？非エンジニアにとってのOpenClawの重要性
- OpenClawを使った実際の開発事例・ライブデモ
- OpenClawだけ学んでも不十分？ AI基礎力＋開発知識が必要な理由とユニコスクールの紹介
- OpenClawの未来・8週間ワークショップの案内

**「OpenClawとClaude Code Channels、どう使い分けるべきか」が完全にクリアになる120分です。**

第1回は1,000名申込、第2回・第3回も各500名申込の人気セミナーの第4回。今回もすぐ埋まる可能性大なので、お早めにどうぞ👇

🔗 https://peatix.com/event/4939620/view
