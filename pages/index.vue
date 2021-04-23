<template>
  <div class="main-contents">
    <p class="search-result">
      検索結果（{{ shops.length }}件）
    </p>
    <p v-if="error">データの取得に失敗しました</p>
    <ul class="restaurant-list">
      <li
        v-for="shop in shops"
        :key="shop.id"
        @click="$router.push(`/${shop.id}`)"
        class="restaurant-list__item"
      >
        <div class="restaurant-list__img-wrap">
          <img class="restaurant-list__img" :src="shop.photo.pc.l">
        </div>
        <div>
          <h2 class="restaurant-list__shop-name">{{ shop.name }}</h2>
          <span class="restaurant-list__genre">{{ shop.genre.name }}</span>
          <p class="restaurant-list__catch">{{ shop.genre.catch }}</p>
          <p class="restaurant-list__station">{{ shop.station_name }}</p>
          <p class="restaurant-list__close">{{ shop.close }}</p>
          <p class="restaurant-list__budget">{{ shop.budget.average }}</p>
        </div>
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