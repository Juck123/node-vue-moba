<template>
  <div>
    <swiper :options="swiperOptions">
      <swiper-slide>
        <img
          class="w-100"
          src="../assets/images/ca86ea9d85a6b34d36451147a63eda51.jpeg"
          alt=""
        />
      </swiper-slide>

      <swiper-slide>
        <img
          class="w-100"
          src="../assets/images/e7dd2e380c184c9d3c880a43b319b5c7.jpeg"
          alt=""
        />
      </swiper-slide>

      <swiper-slide>
        <img
          class="w-100"
          src="../assets/images/0221ad22647925d260d36c3cb9bd42ef.jpeg"
          alt=""
        />
      </swiper-slide>

      <div
        class="swiper-pagination pagination-home text-right px-3 pb-2"
        slot="pagination"
      ></div>
    </swiper>

    <!-- end of swiper -->

    <div class="nav-icons bg-white mt-3 text-center pt-3">
      <div class="d-flex flex-wrap">
        <div class="nav-item mb-3" v-for="n in 10" :key="n">
          <i class="sprite sprite-news"></i>
          <div class="py-2">爆料站</div>
        </div>
      </div>

      <div class="bg-light py-2 fs-sm">
        <i class="sprite sprite-arrow mr-1"></i>
        <span>收起</span>
      </div>
    </div>

    <!-- end of icons -->

    <m-list-card icon="cc-menu-circle" title="新闻资讯" :categories="newsCats">
      <template #items="{category}">
        <router-link
        tag="div"
        :to="`/articles/${news._id}`"
          v-for="(news, i) in category.newsList"
          :key="i"
          class="py-2 fs-lg d-flex"
        >
          <span class="text-info">[{{ news.categoryName }}]</span>
          <span class="px-2"> | </span>
          <span class="flex-1 text-dark-1 text-ellipsis pr-2">{{
            news.title
          }}</span>
          <span class="text-grey-1 fs-sm">{{ news.createdAt | date }}</span>
        </router-link> 
      </template>
    </m-list-card>

    <m-list-card icon="card-hero" title="英雄列表" :categories="heroCats">
      <template #items="{category}">
        <div class="d-flex flex-wrap" style="margin: 0 -0.5rem;">
          <router-link  tag="div"
          :to="`/heroes/${hero._id}`"
          v-for="(hero, i) in category.heroList" :key="i" class="p-2 text-center" style="width:20%;">
            <img :src="hero.avatar"  class="w-100">
            <div >{{ hero.name }}</div>
          </router-link>
        </div>
      </template>
    </m-list-card>

    <m-card icon="VIDEO" title="精彩视频"></m-card>
    <m-card icon="VIDEO" title="图文攻略"></m-card>
    <m-card icon="VIDEO" title="精彩视频"></m-card>
    <m-card icon="VIDEO" title="精彩视频"></m-card>
    <m-card icon="VIDEO" title="精彩视频"></m-card>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  filters: {
    date(val) {
      return dayjs(val).format("MM/DD");
    },
  },
  data() {
    return {
      swiperOptions: {
        pagination: {
          el: ".pagination-home",
        },
      },
      newsCats: [],
      heroCats: [],
    };
  },
  methods: {
    async fetchNewsCats() {
      const res = await this.$http.get("news/list");
      this.newsCats = res.data;
    },

    async fetchHeroCats() {
      const res = await this.$http.get("heroes/list");
      this.heroCats = res.data;
    },
  },

  created() {
    this.fetchNewsCats();
    this.fetchHeroCats();
  },
};
</script>

<style lang="scss">
// 使用嵌套
@import "../assets/scss/variables"; //引用

.pagination-home {
  .swiper-pagination-bullet {
    opacity: 1;
    border-radius: 0.1538rem;
    background: map-get($colors, "white");
    &.swiper-pagination-bullet-active {
      background: map-get($colors, "info");
    }
  }
}

.nav-icons {
  border-top: 1px solid $border-color;
  border-bottom: 1px solid $border-color;
  .nav-item {
    width: 25%;
    border-left: 1px solid $border-color;
    &:nth-child(4n + 1) {
      border-left: none;
    }
  }
}
</style>
