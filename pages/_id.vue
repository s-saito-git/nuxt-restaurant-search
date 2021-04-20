<template>
  <div>
    <p>{{ $route.params.id }}</p>
    <p>{{ shop.name }}</p>
    <a @click="$router.back()">戻る</a>
  </div>
</template>

<script>
export default {
  scrollTop: true,
  async asyncData({ $axios, params, env, redirect }) {
    try {
      //データの取得
      const { data } = await $axios.get("http://localhost:3000/api/gourmet/v1/",{
        params: {
          key: env.API_KEY,
          id: params.id,
          format: "json"
        }
      }
    );
    //対象データがない場合は一覧ページに戻る
    if (data.results.shop.length <= 0) {
      redirect("/");
    }
    //dataにマージ
    return {
      shop: data.results.shop[0],
      error: undefined
    };
  }catch (error) {
      //エラーが発生した場合のdataへのマージ
      return {
        shop: undefined,
        error: {
          message: "データの取得に失敗しました"
        }
      };
    }
  }
}
</script>
