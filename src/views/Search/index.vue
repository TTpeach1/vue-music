<template>
  <div>
    <van-search v-model="value" placeholder="请输入搜索关键词" />
    <!-- 热门搜索 -->
    <template v-if="this.value==''">
      <van-cell title="热门搜索" />
      <div style="padding: 10px 16px">
        <van-tag
          color="#ccc"
          size="large"
          plain
          round
          type="primary"
          v-for="(item, index) in Tag"
          :key="index"
          @click="clickFn(item.first)"
          style="margin: 4px 4px"
        >
          {{ item.first }}
        </van-tag>
      </div>
    </template>
    <template v-else>
      <van-cell title="最佳匹配" />
      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
      >
        <van-cell
          v-for="item in Search"
          :key="item.id"
          :title="item.name"
          :label="`${item.ar[0].name || '未知歌手'}-${item.name}`"
        />
      </van-list>
    </template>
  </div>
</template>

<script>
import { getSearchTagApi } from '@/apis';
import { getSearchListApi } from '@/apis';
export default {
  name: 'VueMusicIndex',

  data() {
    return {
      value: '',
      Tag: [],
      Search: [],
      loading: '',
      finished: true,
    };
  },
  created() {
    this.getTag();
  },

  mounted() {},

  methods: {
    async getTag() {
      try {
        const res = await getSearchTagApi({});
        console.log(res);
        this.Tag = res.data.result.hots;
      } catch (e) {
        console.log('热门搜索获取失败');
      }
    },
    async clickFn(val) {
      this.value = val;
      try {
        const res = await getSearchListApi({
          keywords: this.value,
        });
        console.log(res);
        this.Search = res.data.result.songs;
      } catch (e) {
        console.log('搜索获取失败');
      }
    },
    onLoad() {},
  },
};
</script>

<style lang="scss" scoped></style>
