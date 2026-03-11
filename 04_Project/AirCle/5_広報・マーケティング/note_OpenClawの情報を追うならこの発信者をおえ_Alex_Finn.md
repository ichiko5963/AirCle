# OpenClaw界隈で、まず見るべきたった1人の発信者を教えます

OpenClawの情報を追い始めると、「誰を見ればいいか」で理解の深さがまったく変わります。

日本語の情報だけだと「なんとなくすごい」で終わりがち。本気で使いこなしたいなら、**海外でOpenClawを実運用している発信者**をまず1人、押さえるのがいちばん早いです。

結論から言います。  
**まず見るべきたった1人は、Alex Finn（@AlexFinn）です。**

---

## この人、誰？ Alex Finn のプロフィール

**Alex Finn**  
X: [@AlexFinn](https://x.com/AlexFinn)  
Substack: [Ship/It Weekly - AI, made simple](https://shipitai.substack.com/)  
公式サイト: <https://www.alexfinn.ai/>

本人のXプロフィールはこうです。

> I love vibe coding. Founder/CEO of Creator Buddy, the only AI trained on all of your X posts. Built a 300k ARR app by myself.  
> <https://creatorbuddy.io>

要するに、

- **Vibe Coding**でプロダクトを作っている人  
- **Creator Buddy**の創業者・CEO（あなたのX投稿で学習したAI）。**ARR 30万ドル**をひとりで達成  
- その人が **OpenClaw にフルベットしている**  

という顔です。

### 経歴と「一発」のきっかけ

元**MongoDB**のチームリード。AIを軸にした発信を続け、**3年でXフォロワー26万超**まで伸ばしました。

転機は**2023年**。イーロン・マスクがXのアルゴリズムをオープンソース化したとき、Alexは**14時間かけて約40万行のコードを読み、アルゴリズムの仕組みを解説するスレッド**を投稿。**Elon Musk本人やMark Cubanにリツイート**され、一気に認知が広がります。

今では**Xフォロワー43万人以上**、**YouTubeも約6.4万登録**で、vibe codingやClaude、**OpenClaw**の実践ネタを発信しています。

彼の**YouTubeもやばい**です。OpenClawのセットアップ、ローカルモデルの繋ぎ方、複数エージェントの運用まで、動画で手順が追える。しかも**日本語字幕でも視聴できる**から、英語が苦手でも学びやすい。日本語でも見れるから最高、という意味で、日本人が「まず見る1人」として彼を押さえるメリットはかなり大きいです。

---

## Alex Finn が OpenClaw で何をしているか（＝なぜ“まず見る1人”か）

AlexはOpenClawを**仕事と生活の中心**に置いています。「まず見るべきたった1人」と言い切れる理由は、**発信の量・質・実運用**の三点です。

### 1. ローカルモデルで「無制限・無料」に近いOpenClaw

Substackに **「Unlimited Free OpenClaw: How to connect your OpenClaw to a local model (even on a Mac Mini)」** という記事があります。

- クラウドAPIだけだと月数千ドル・レート制限が厳しい  
- **ローカルモデル（例: Qwen 3.5）**を接続すると、無制限トークン・完全プライバシー・API料金ゼロ  
- **Mac Mini 32GB**でも動かせる（LM Studio で管理）  
- 彼自身は**Mac Studioを3台**使ってOpenClawを24時間稼働  

「フロンティアモデルで計画、実行はローカルモデル」の**ハイブリッド構成**を推していて、設定方法を具体的に公開しています。

### 2. 複数エージェントの「SaaS工場」を実運用

- **4体のOpenClawエージェント**が同じプロダクト向けに並行してタスクを実行  
- タスクが終わったエージェントは**自分で次のタスクを探して取りにいく**  
- 別エージェント **「Ralph」** が**QA**を担当し、他エージェントの成果をレビュー・改善  
- **閉じたループで自己改善する仕組み**を、ローカルモデルでAPI料金ほぼゼロで回している  

「1人ビジネスなのに、複数エージェントが24時間働いている」状態を、自分でやりながら発信している人です。

### 3. 「OpenClawは Mission Control で100倍良くなる」

Xなどで **「OpenClaw becomes 100x better when you build it a Mission Control」** と発信しています。

Mission Control は、**複数OpenClawエージェントを統括・可視化する仕組み**。タスク割り当て・進捗・連携を「司令塔」からコントロールするイメージです。Alexは、この**マルチエージェント運用の最前線**を記事・動画で共有しています。

### 4. 学ぶ場：Vibe Coding Academy（$100/月）

**Vibe Coding Academy**（<https://vibecodingacademy.dev/>）を運営しています。

- **毎週ライブのOpenClawブートキャンプ**に参加できる  
- **OpenClaw Masterclass**、**Full ClawdBot Bootcamp** などOpenClaw特化コンテンツあり  
- **800人以上**が参加  

無料の記事・動画だけでなく、**最新のやり方や質問を直接できる**環境があります。

---

## なぜ「まず見るべきたった1人」が Alex Finn なのか

1. **実績** … Vibe CodingでCreator Buddyをひとりで作り**2週間でARR 30万ドル**、X 43万フォロワー、アルゴリズム分析でElon Muskにリツイートされた「AI×プロダクト」の実績者  
2. **OpenClawにフルベット** … 複数エージェント＋ローカルモデルで24時間運用し、SaaS工場やQAエージェントまで実践している  
3. **発信が具体的** … Substackで設定方法（Mac Mini、Qwen、LM Studio）を公開し、Mission Controlや「100倍良くなる」など、**何をすると効果が出るか**を語っている  
4. **学ぶ入口がある** … 無料の記事・YouTube（日本語字幕あり）に加え、$100/月のコミュニティでライブ質問・マスタークラスに参加できる  

海外でAIで結果を出している人が、OpenClawに人生をかけ、**数字と仕組みレベルで発信している**。  
だから、OpenClawの情報を追うなら、**まずこの1人を押さえておけば間違いない**です。

---

## まとめ

- **OpenClaw界隈で、まず見るべきたった1人 = Alex Finn（@AlexFinn）**  
- 元MongoDB、Xアルゴリズム分析でElon Muskにリツイート、X 43万フォロワー  
- **YouTubeもやばい**（OpenClaw・ローカルモデル・複数エージェントを動画で学べる。日本語字幕で日本語でも見れるから最高）  
- Creator Buddy CEO、Vibe CodingでARR 30万ドルをひとりで達成  
- OpenClawをローカルモデル＋複数エージェントで24時間運用し、Substack・YouTube・Vibe Coding Academyで発信  

まず見るなら、この1人。Alex Finn を押さえろ。
