<template>
  <main class="main">
    <h1 class="title">{{ title }}</h1>
    <time>{{ publishedAt }}</time>
    <div class="post" v-html="body.replace(/\n/g,'<br/>')"></div>
  </main>
</template>

<script>
// microCMSからコンテンツを取得
import axios from 'axios'

// publishedAtを日本時間に変換
import dayjs from 'dayjs'
import utc from 'dayjs/plugin/utc'
import timezone from 'dayjs/plugin/timezone'
dayjs.extend(utc)
dayjs.extend(timezone)

export default {
  async asyncData({ params }) {
    const { data } = await axios.get(
      `https://trade.microcms.io/api/v1/blog/${params.slug}`,
      {headers: {'X-API-KEY':'8ccac964-e916-4075-ade4-0435d219c3b5'}}
    )
    data.publishedAt = dayjs.utc(data.publishedAt).tz('Asia/Tokyo').format('YYYY.MM.DD')
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

.post {
  & > h1 {
    font-size: 30px;
    font-weight: bold;
    margin: 40px 0 20px;
    background-color: #eee;
    padding: 10px 20px;
    border-radius: 5px;
  }

  & > h2 {
    font-size: 24px;
    font-weight: bold;
    margin: 40px 0 16px;
    border-bottom: 1px solid #ddd;
  }

  & > p {
    line-height: 1.8;
    letter-spacing: 0.2px;
  }

  & > ol {
    list-style-type: decimal;
    list-style-position: inside;
  }
}
</style>
