# 規約調査レポートと公開判断（2026-11-02）

調査日：2026-11-02
対象：本スキル（Grok Build CLI 経由の読み取り専用 X 検索スキル）の自作・公開が、X / xAI の規約上問題にならないか

> 本書は規約文面と公開情報に基づく調査メモであり、法的助言ではありません。

## 公開判断

**本調査に基づき、2026年11月2日時点で、本スキルを公開する判断を下した。**

判断の前提条件（本スキルの設計が満たしていること）：

- 検索は xAI 公式のインターフェース（Grok Build CLI の `x_search`）経由でのみ行い、スクレイピングやブラウザ自動化はしない
- 認証は利用者各自が自分のアカウント・自分のサブスクリプションで行い、認証情報を共有・配布しない
- 読み取り専用とし、投稿などの書き込みは行わない
- 公開するのはコードと手順のみで、X のコンテンツのデータセットは配布しない
- 利用は個人の調査目的で、低頻度・範囲を絞った使い方にとどめる

## 調査のきっかけ

公開にあたり、次の疑問を解消する必要があった。

1. SearXNG の前身 searx には Twitter エンジンがあったのに現行ラインから消えている。もし消えた理由が「法的要件」なら、自作の X 検索機能も規約上作れないのではないか
2. xAI の利用ポリシーにある「許可されていない自動手段（unauthorized automated or non-human means）」という文言が大きい。許可が Grok Build や Hermes のような名前のある製品に限るなら、自作ラッパーは公開しづらいのではないか
3. 将来、Google Antigravity のように「他の AI エージェント経由の利用禁止」が xAI 側にも追加される可能性への備え

## 1. SearXNG の Twitter エンジンが消えた理由は、法的要請ではなかった

削除したプルリクエストの本文を確認した。

- searx PR #2372（2020-12）: 理由は「API が変わり結果を返さない、書き直しが必要」のみ。法的な言及なし。対案は Nitter 経由だった
  https://github.com/searx/searx/pull/2372
- SearXNG PR #2843（2023-09）: 理由は「Twitter が Web API を強く制限し、ツイートも検索も取得できない」のみ。法的な言及なし
  https://github.com/searxng/searxng/pull/2843

並行事例も同じ構図だった。snscrape は 2023-06 のログインウォールで技術的に動作不能になり、Nitter は 2024-01 のゲストアカウント廃止で事実上終了した。公式 API 用ツールの twarc は、法的問題ではなく API のクォータ（料金）で実質使えなくなりサポート終了した。つまり「締め付け」の実態は、法ではなく技術的封鎖と有料化である。

なお、X が法的執行（訴訟・停止通告）に動いた相手は、無許可の商用スクレイパーや代替フロントエンド（例：Nitter への 2026 年の cease-and-desist）に限られ、公式インターフェースを使う OSS ツールへの法的措置は確認できなかった。Bright Data 訴訟（2024-05）では X の請求自体が棄却されている。

## 2. 規約の線引きは「公式インターフェースを使っているか」

X の開発者向け文書（Developer Agreement / Developer Policy / Developer Guidelines、xdevplatform の公式ドキュメントリポジトリで確認）では：

- 公式 API の統合・分析はライセンスされ、検索・読み取りはレート制限内の通常利用として許可されている。学術研究も有効な用途とされる
- コードの公開自体を禁じる条項はない。禁じられているのは、取得したコンテンツの大規模な再配布（再配布は原則 Post ID のみ等）、認証情報の共有・他人の認証情報の利用、代替サービスの構築、レート制限の回避、ブラウザ自動化／スクレイピング（アカウント永久停止の対象）
- 消費者向け利用規約も、同意なきスクレイピングと技術的制限の迂回を禁じるもので、自作ツールの存在自体を禁じるものではない

出典：
- https://github.com/xdevplatform/docs/blob/HEAD/developer-terms/agreement.mdx
- https://github.com/xdevplatform/docs/blob/HEAD/developer-terms/policy.mdx
- https://github.com/xdevplatform/docs/blob/HEAD/developer-guidelines.mdx

## 3. Grok の x_search と Grok Build は公式の想定用途

- `x_search` は xAI 公式ドキュメントにある Responses API のサーバーサイドツールで、利用者側が x.com をスクレイプする仕組みではない。なお「LLM を使わない検索エンジン照会」ではなく、検索バックエンドが投稿を取得し、クエリ設計・反復・読解・引用付き合成は Grok モデルが担う https://docs.x.ai/developers/tools/x-search
- Grok Build の headless モード（`grok -p`）は、公式ドキュメントで「スクリプトや自動化、他のアプリへの統合に最適（ideal for scripts, automations, or integration into other apps）」「スクリプトやボットの中で、あるいは ACP 経由で他のアプリから」と明記されている https://docs.x.ai/build/overview

## 4. Hermes の事例：「許可」は製品名のリストではない

X 公式（@XDevelopers）は 2026-05、第三者製 OSS エージェントフレームワーク Hermes Agent を、X 公式 CLI `xurl` の skill で X API に接続するガイドを投稿した。Hermes の `x_search` の実体は、本スキルと同じ xAI 公式 x_search を呼ぶラッパーであり、2026-05-16 にマージされ v0.14.0 に同梱された（認証は SuperGrok / X Premium+ の OAuth または `XAI_API_KEY`）。

さらに xAI は 2026-05 に Hermes、OpenClaw、OpenCode との公式統合を相次いで発表した（https://x.ai/news/grok-hermes 、https://x.ai/news/grok-openclaw 、https://x.ai/news/grok-opencode ）。2026-02 の X API Pay-Per-Use 開始、2026-06 のホステッド X MCP 公開と合わせ、X / xAI が第三者エージェントを公式インターフェース上の顧客として取り込みに行く方針が読み取れる。

xAI の Acceptable Use Policy（2026-08-14 発効）の中核文言は「Accessing the Services through **unauthorized** automated or non-human means」であり、版比較では "unauthorized" は 2026-06 版から 8 月版で追加され、自動化の禁止範囲は狭まる方向に改訂されている（https://x.ai/legal/acceptable-use-policy ）。

以上から、「authorized かどうかは製品名ではなく、公式インターフェース（X API / xAI x_search / xurl / 公式 MCP）を正規の認証で経由しているかで判断される」という解釈が、証拠と最も整合する。自作ラッパーでも公式インターフェース経由であれば、Hermes の x_search と構造的に同一である。

## 5. Antigravity 型の制限は、現行の xAI 規約には存在しない

Google Antigravity では実際に起きたこと（2026-02）：OpenClaw のプラグインが公式クライアントを通さず OAuth でバックエンドを直接叩いた有料会員が、警告なく大量にアカウント停止された。Google 側の規約（Additional Terms §6）には「当社製品以外と組み合わせた利用・第三者ソフトウェアでのアクセスは違反」と明文があり、公式 FAQ では公式 CLI を第三者エージェントにラップする使い方も消費者プランでは非対応と明言された（https://antigravity.google/terms 、https://antigravity.google/docs/faq/ ）。

対照的に、現行の xAI 規約・公式文書に同種の条項は見つからず、文書と公式連携の実績は逆方向を示している。ただし将来の変更への監視ポイントとして次を挙げておく。

- Antigravity §6 型の「第三者アクセス禁止」条項の追加
- "unauthorized" の削除や、第一者クライアント限定への定義変更
- `grok -p` のラップ利用を消費者プランで不可とする明確化
- プログラム利用の別枠課金化

低リスク化策としては、`XAI_API_KEY` を使い Responses API の `x_search` を直接呼ぶ経路が最も曖昧さが小さい（API 向け規約は自社製品への統合開発を明示的に許諾している）。

## 6. 残るグレーゾーンと未確認事項

- xAI AUP の "unauthorized" に公式の定義はなく、提供側の裁量でアカウント停止され得る余地自体は残る。これは Grok Build 単体利用でも自作ラッパーでも等しく被るリスクであり、形態による差ではない
- Grok Build と X Premium 系プランの連携仕様の細部は公式プランページでの再確認が望ましい（プラン再編の報道があるため）
- Grok Build の実バイナリで `x_search` がその名で露出しているかは第三者情報が食い違い、未確定。ただし経路によらずモデル統制の検索である点は変わらない
- docs.x.com 本体は調査環境から直接取得できず、内容は公式 GitHub ドキュメント原稿で確認した
- 本調査は日本法の検討を含まない

## まとめ

- SearXNG の Twitter エンジン消滅は法的要請ではなく、技術的封鎖と API 有料化が主因
- X 検索ツールを作って公開すること自体は禁止されておらず、公式 API 自作も、Grok / Hermes 経由も、ともに正規ルート側にある
- 本スキルは「公式インターフェース経由・各自の認証情報・読み取り専用・公開はコードのみ」という条件を満たすため、上記の公開判断に至った
