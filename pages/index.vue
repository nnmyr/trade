<template>
  <ul>
    <li v-for="content in contents" :key="content.id">
      <time>{{ content.publishedAt }}</time>
      <nuxt-link :to="`/${content.id}`">
        {{ content.title }}
      </nuxt-link>
    </li>
  </ul>
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
  async asyncData() {
    const { data } = await axios.get(
      'https://trade.microcms.io/api/v1/blog?limit=1000',
      {headers: {'X-API-KEY':'8ccac964-e916-4075-ade4-0435d219c3b5'}}
    )
    data.contents.forEach((key) => {
      key['publishedAt'] = dayjs.utc(key['publishedAt']).tz('Asia/Tokyo').format('YYYY/MM/DD(ddd)')
    })
    return data
  }
}
</script>
