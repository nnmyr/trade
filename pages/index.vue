<template>
  <ul>
    <li v-for="content in contents" :key="content.id">
      <nuxt-link :to="`/${content.id}`">
        {{ content.title }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
// microCMSからコンテンツを取得
import axios from 'axios'
export default {
  async asyncData() {
    const { data } = await axios.get(
      'https://trade.microcms.io/api/v1/blog',
      {headers: {'X-API-KEY':'8ccac964-e916-4075-ade4-0435d219c3b5'}}
    )
    return data
  }
}
</script>
