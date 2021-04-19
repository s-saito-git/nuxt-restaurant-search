<template>
  <div>
    <h2>
      検索結果（{{ shops.length }}件）
    </h2>
    <p v-if="error">データの取得に失敗しました</p>
    <ul>
      <li v-for="shop in shops" :key="shop.id">
        <img :src="shop.photo.pc.l">
        <p>{{ shop.id }}</p>
        <p>{{ shop.name }}</p>
        <p>{{ shop.address }}</p>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  data() {
    return {
      shops: [],
      error: false
    }
  },
  methods: {
    //レスポンス後の処理
    setShop(res) {
      this.shops = res.data.results.shop
    },
    //エラーが発生した場合の処理
    setError(err) {
      console.log(err)
      this.error = true
    }
  },
  mounted() {
    this.$axios('http://localhost:3000/api/gourmet/v1/', {
      //パラメーターの設定
      params: {
        key: process.env.API_KEY,
        lat: '35.681236',
        lng: '139.767125',
        format: 'json'
      }
    }).then(this.setShop).catch(this.setError)
  }
}
</script>

<style>

</style>
