---
# try also 'default' to start simple
theme: ./theme
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# some information about the slides, markdown enabled
info: |
  ## Cloudflare Workers + Vite = Vitedge

  Learn more at [Vitedge](https://vitedge.js.org)
---

# SSR at the Edge

A look at a real serverless platform:
<br>
Cloudflare Workers for edge computing.

<div class="flex justify-center space-x-2 items-center -mt-40">
<span>Vite</span><mdi-plus /><span>CF Workers</span><mdi-equal /><span>Vitedge</span>
</div>

<!--

-->

---

# サーバーレスの誤解

- <mdi-server class="inline mb-1 text-indigo-400"/> 従量課金による誰かのサーバーでホストされる
  <LiSub>It's just somebody else's server with metered billing.</LiSub>

<VClicks>

- <mdi-map-marker class="inline mb-1 text-indigo-400"/> 基本、世界で 1 ロケーションのみ
  <LiSub>Generally, only **1 location** in the world.</LiSub>

<div>

- <mdi-speedometer-slow class="inline mb-1 text-indigo-400 "/> コールドスタートのため遅い
  <LiSub>Slower due to **coldstarts**.</LiSub>

<ColdstartComparison />

</div>

<div>

<br><br><br><br>

- <mdi-head-question class="inline mb-1 text-indigo-400"/> ステートレスロジックは一定のタスクを複雑にするかもしれない
  <LiSub>Stateless logic might complicate certain tasks.</LiSub>

</div>

</VClicks>

<!--

-->

<!-- <style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style> -->

---

# 真のサーバーレスとは: Cloudflare Workers (1)

<div class="flex">

<div class="pr-4">

- <subway-world class="inline mb-1 text-indigo-400"/> コードは世界中の CDN ノードで時刻される(アフリカや中国を含む)
  <LiSub>Code runs in CDN nodes **all over the world** (including Africa and China\*).</LiSub>

<VClicks>

- <mdi-lightning-bolt class="inline mb-1 text-indigo-400"/> とても速い！コールドスタートなし、そしてコードはエンドユーザーの近くで動作する
  <LiSub>Super-fast! **No coldstarts**, and code **runs near the end-user**.</LiSub>
- <mdi-infinity class="inline mb-1 text-indigo-400 mr-1"/> 超スケーラブル。数百万のリクエストを同時に処理できる
  <LiSub>Ultra-scalable. It can handle millions of requests at the same time.</LiSub>
- <ri-user-location-fill class="inline mb-1 text-indigo-400 mr-1"/> ユーザーのロケーションにアクセス(国、市、郵便番号)
  <LiSub>Access to **user location** (country, city, postal code). `console.log(request.cf.city, request.cf.country)`</LiSub>

</VClicks>

</div>

<div>
<CfwRegions />
</div>

</div>

---

# 真のサーバーレスとは: Cloudflare Workers (2)

<div class="flex">

<div class="pr-4">

- <mdi-cached class="inline mb-1 text-indigo-400"/> フレキシブルなエッジキャッシュ
  <LiSub>Flexible **edge cache**.</LiSub>

<VClicks>

- <ri-global-line class="inline mb-1 text-indigo-400"/> グローバルな Key-Value ストア(Redis 風な)
  <LiSub>Global Key-Value store (~ala Redis).</LiSub>
- <websymbol-updown-circle class="inline mb-1 text-indigo-400"/> リアルタイム: WebSoket サポート
  <LiSub>Real time: it **supports WebSockets**.</LiSub>
- <mdi-security class="inline mb-1 text-indigo-400"/> DNS、アナリティクス、クロンジョブ、そしてセキュリティがビルドイン
  <LiSub>Built-in DNS, analytics, cron jobs and security.</LiSub>
- <mdi-currency-jpy class="inline mb-1 text-indigo-400"/> 経済: "従来"の cloud functions より安い
  <LiSub>Economic: it's cheaper than "traditional" cloud functions.</LiSub>

</VClicks>

</div>

<div>
<CfwRegions />
</div>

</div>

---

# Workers の例

- <mdi-ab-testing class="inline mb-1 text-indigo-400"/> ユーザーのロケーションに依存する A/B テスト
  <LiSub>A/B testing depending on user location, language, etc.</LiSub>

<VClicks>

- <mdi-google-analytics class="inline mb-1 text-indigo-400"/> たくさんの JavaScript をブラウザにプッシュすることなく、アナリティクスを実現
  <LiSub>Analytics without pushing a ton of JS to the browser.</LiSub>

<div>

- <mdi-shopping class="inline mb-1 text-indigo-400"/> 日本国内以外のユーザーにのみ広告バナーを表示する（海外への配信）
  <LiSub>Show an advertising banner only to users that are not located in Japan (shipping overseas).</LiSub>

<OverseasBanner />

</div>

</VClicks>

---

# Workers の欠点

Workers の実行は速い、が環境の制約がある:
<PSub>Workers run in a fast but constrained environment:</PSub>

- <mdi-api-off class="inline mb-1 text-indigo-400"/> Node.js API ではない: 互換のある NPM パッケージが少ない
  <LiSub>No Node.js APIs: fewer compatible NPM packages.</LiSub>

<VClicks>

- <mdi-floppy class="inline mb-1 text-indigo-400"/> 実行制限: メモリ、CPU、リクエスト
  <LiSub>Execution limits: memory, CPU time, sub-requests.</LiSub>
- <cib-experts-exchange class="inline mb-1 text-indigo-400"/> 開発体験 (改善中) と不可解なエラー
  <LiSub>Developer Experience (it's improving).</LiSub>
- <mdi-comment-account class="inline mb-1 text-indigo-400"/> コミュニティはまだ小さい
  <LiSub>Community is still small.</LiSub>

</VClicks>

---

# Vite

次世代フロントエンドツール (作者 Evan You & コミュニティ)
<PSub>Next Generation Frontend Tooling (by Evan You & community)</PSub>

<div class="absolute right-[100px] top-[120px] max-w-[200px]">
  <img src="https://vitejs.dev/logo.svg">
</div>

- <mdi-check-decagram class="inline mb-1 text-indigo-400"/> 素晴らしい開発体験！すぐに動作します
  <LiSub>Fantastic developer experience. It **just works™** out of the box.</LiSub>

<VClicks>

- <mdi-lightning-bolt class="inline mb-1 text-indigo-400"/> ほぼ瞬時に HMR とサーバーが動作する
  <LiSub>Almost instant HMR and server start.</LiSub>
- <mdi-comment-account class="inline mb-1 text-indigo-400"/> 信じられないほど速く成長するコミュニティ
  <LiSub>Incredibly fast-growing community.</LiSub>
- <fa-solid-hand-spock class="inline mb-1 text-indigo-400"/> フレームワーク依存なし、でもそれは Vue 中心に見据えられています <3
  <LiSub>Framework-agnostic but keeps Vue at its heart <3.</LiSub>
- <mdi-lightbulb class="inline mb-1 text-indigo-400"/> 驚異的な拡張性 (WebSockets、SSR、Rollup プラグイン)
  <LiSub>Absurdly extendable (WebSockets, SSR, Rollup plugins).</LiSub>

</VClicks>

---

# Vitedge: エッジで動作する SSR (1)

Vite と Cloudflare workers と構成されたエッジサイドレンダリング(ESR)フレームワーク
<PSub>Edge Side Rendering (ESR) framework that combines Vite and Cloudflare Workers.</PSub>

<ViteCloud />

<VClicks>

Vitedge は、最初の表示をエッジワーカーでプリレンダリングし、残りを SPA として実行する、ただの Vite アプリ ™ です。つまり、SPA のキビキビとしたルーティングや DX を維持しつつ、良好な SEO につながるということです。
<PSub>Vitedge is just a Vite app ™ that prerrenders the first view in an edge worker and runs the rest as an SPA. That means it will lead to good SEO while keeping the snappy routing and DX of an SPA.</PSub>

それは、オンザフライでビルドし、エッジでキャッシュするため、ある状況によっては静的サイトジェネレータを置き換えることができます。そのため、CDN からから静的な index.html を取得する代わりに、CDN 自身オンザフライで index.html を作成したり、すでにアクセスされている場合(cache age + stale-while-revalidate で構成可能)は、キャッシュから提供します。
<PSub>It can replace static site generators in some situations since it builds on the fly and caches at the edge. Therefore, instead of getting a static index.html from the CDN, the CDN itself will create it on the fly or provide it from cache if it was already accessed (with configurable cache age + stale-while-revalidate).</PSub>

</VClicks>

---

# Vitedge: エッジで動作する SSR (2)

- <mdi-lightning-bolt class="inline mb-1 text-indigo-400"/> SEO とロード時間の改善しつつ、ビルドに時間がかかることがなくコンテンツを動的を保つ
  <LiSub>Improve SEO and loading time but keep content dynamic without slow builds.</LiSub>

<VClicks>

- <mdi-cloud-print class="inline mb-1 text-indigo-400"/> レンダリングとキャッシュをエッジで行うことで、プロダクションでのパフォーマンスを最大限にする
  <LiSub>Renders and caches at the edge for maximum performance in production.</LiSub>
- <mdi-cached class="inline mb-1 text-indigo-400"/> 構成可能なキャッシュ、必要に応じて無効化できる
  <LiSub>Configurable cache. Invalidate on demand.</LiSub>
- <mdi-api class="inline mb-1 text-indigo-400"/> シンプルなファイルベースな API エンドポイントの作成
  <LiSub>Create simple file-based API endpoints.</LiSub>
- <file-icons-vite class="inline mb-1 text-indigo-400"/> Vite のすべてのメリットを享受できる、API 側も(ES Modules を使って)
  <LiSub>All the benefits of Vite, even for the API side (using ES Modules).</LiSub>

</VClicks>

<VClicks>

免責事項: SSR は必要ないかもしれません
<PSub>Disclaimer: You might not need SSR.</Psub>

</VClicks>

---

# Vitedge: エッジで動作する SSR (3)

Installation in a Vite app.

<div class="flex gap-2">

<VitedgeSrc />

<div grid="~ cols-2 gap-2" m="-t-2">

Import plugin in `vite.config.ts`

A single entry point by default in `main.ts`

```ts
import { defineConfig } from "vite";
import vitedge from "vitedge/plugin.js";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [vitedge(), vue()],
});
```

```ts
import "./index.css";
import App from "./App.vue";
import routes from "./routes";
import vitedge from "vitedge";

export default vitedge(App, { routes }, (ctx) => {
  const { app, router, initialState } = ctx;
  // Setup i18n, Vuex/Pinia, etc.
});
```

</div>

</div>

---

# Vitedge: エッジで動作する SSR (4)

<div class="flex gap-2">

<VitedgeFn />

<div grid="~ cols-2 gap-2" m="-t-2">

```vue
<script setup>
// Route: { name: 'post', path: '/posts/:slug' }
import { defineProps } from "vue";
import { useHead } from "@vueuse/head";

const props = defineProps({
  post: Object,
});

const content = props.post.description;

useHead({
  html: { lang: "en" },
  meta: [{ name: "description", content }],
});
</script>

<template>
  <h1>{{ post.title }}</h1>
  <div>{{ post.body }}</div>
  <!-- ... -->
</template>
```

```ts
// <root>/functions/props/post.ts
export default {
  async handler({ request, params: { slug } }) {
    if (request.method !== "GET") {
      throw new Error("Method not supported");
    }

    const cmsApi = "https://my-cms-api.com/posts/";
    const cmsResponse = await fetch(cmsApi + slug);
    const post = await cmsResponse.json();

    return {
      data: { post },
      cache: { html: 8001 },
    };
  },
};
```

</div>

</div>

---

# Thanks for listening!

<div class="flex space-x-10 pt-20 justify-center">

<SliDev />

</div>
