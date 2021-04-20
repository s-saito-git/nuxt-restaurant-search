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
const getCurrentPosition = () => {
  return new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(resolve, reject)
  })
}

export default {
  data() {
    return {
      shops: [],
      error: false
    }
  },
  methods: {
    //エラーが発生した場合の処理
    setError(err) {
      console.log(err)
      this.error = true
    }
  },
  async mounted() {
    try {
      //現在位置の取得
      const position = await getCurrentPosition()
      //APIからデータを取得
      const { data } = await
      this.$axios('http://localhost:3000/api/gourmet/v1/', {
        //パラメーターの設定
        params: {
          key: process.env.API_KEY,
          lat: position.coords.latitude,//取得した緯度を設定
          lng: position.coords.longitude,//取得した軽度を設定
          format: 'json'
        }
      })
      //店の一覧を設定
      this.shops = data.results.shop
    } catch(err) {
      this.setError(err)
    }
  }
}
</script>

<style>
</style>