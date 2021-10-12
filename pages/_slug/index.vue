<template>
  <main class="main">
    <article>
      <h1 class="title">{{ title }}</h1>
      <time>{{ publishedAt }}</time>
      <p class="intro" v-if="body" v-html="body.replace(/\n/g,'<br/>')"></p>
      <section v-if="textMarket.exchange">
        <h2>ドル円</h2>
        <p v-html="textMarket.exchange.replace(/\n/g,'<br/>')"></p>
      </section>
      <section v-if="textMarket.stocks_us">
        <h2>米国株</h2>
        <p v-html="textMarket.stocks_us.replace(/\n/g,'<br/>')"></p>
      </section>
      <section v-if="textMarket.stocks_jp">
        <h2>日本株</h2>
        <p v-html="textMarket.stocks_jp.replace(/\n/g,'<br/>')"></p>
      </section>
      <section v-if="textMarket.commodity">
        <h2>WTI原油</h2>
        <p v-html="textMarket.commodity.replace(/\n/g,'<br/>')"></p>
      </section>
      <section v-if="textMarket.crypto">
        <h2>ビットコイン</h2>
        <p v-html="textMarket.crypto.replace(/\n/g,'<br/>')"></p>
      </section>
    </article>
  </main>
</template>

<script>
// microCMSからコンテンツを取得
import axios from 'axios'

// publishedAtを日本時間に変換
import dayjs from 'dayjs'
import utc from 'dayjs/plugin/utc'
import timezone from 'dayjs/plugin/timezone'
import 'dayjs/locale/ja'
dayjs.extend(utc)
dayjs.extend(timezone)
dayjs.locale('ja')

export default {
  async asyncData({ params }) {
    const { data } = await axios.get(
      `https://trade.microcms.io/api/v1/blog/${params.slug}`,
      {headers: {'X-API-KEY':'8ccac964-e916-4075-ade4-0435d219c3b5'}}
    )
    data.publishedAt = dayjs.utc(data.publishedAt).tz('Asia/Tokyo').format('YYYY/MM/DD(ddd)')
    return data
  }
}

</script>

<style lang="scss" scoped>
.main {
  max-width: 960px;
  margin: 0 auto;
}

.title {
  margin-bottom: 20px;
}
</style>
