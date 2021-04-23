<template>
  <div class="restaurant-detail">
    <h2 class="restaurant-detail__shop-name">
      {{ shop.name }}
    </h2>
    <span class="restaurant-detail__genre">
      {{ shop.genre.name }}
    </span>
    <div class="restaurant-detail__img-wrap">
      <img class="restaurant-detail__img" :src="shop.photo.pc.l">
    </div>
    <p class="restaurant-detail__catch">
      {{ shop.genre.catch }}
    </p>
    <div class="coupon">
      <p class="coupon__url">{{ shop.coupon_urls.pc }}</p>
      <a class="coupon__button" :href="shop.coupon_urls.pc" target="blank">
        クーポン入手
      </a>
    </div>
    <dl class="restaurant-info-table mt32">
      <dt class="restaurant-info-table__title">
        住所
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.address }}
      </dl>
      <dt class="restaurant-info-table__title">
        交通アクセス
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.access }}
      </dl>
      <dt class="restaurant-info-table__title">
        最寄り駅
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.station_name }}
      </dl>
      <dt class="restaurant-info-table__title">
        営業時間
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.open }}
      </dl>
      <dt class="restaurant-info-table__title">
        定休日
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.close }}
      </dl>
      <dt class="restaurant-info-table__title">
        平均ディナー予算
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.budget.average }}
      </dl>
      <dt class="restaurant-info-table__title">
        食べ放題
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.free_food }}
      </dl>
      <dt class="restaurant-info-table__title">
        飲み放題
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.free_drink }}
      </dl>
      <dt class="restaurant-info-table__title">
        料金備考
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.budget_memo }}
      </dl>
      <dt class="restaurant-info-table__title">
        席数
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.capacity }}席
      </dl>
      <dt class="restaurant-info-table__title">
        個室
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.private_room }}
      </dl>
      <dt class="restaurant-info-table__title">
        座敷
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.tatami }}
      </dl>
      <dt class="restaurant-info-table__title">
        備考
      </dt>
      <dl class="restaurant-info-table__body">
        {{ shop.shop_detail_memo }}
      </dl>
    </dl>
    <button class="back-button mt32" @click="$router.back()">戻る</button>
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
