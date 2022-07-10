<template>
  <div class="home-container">
    <van-cell class="title" title="推荐歌单" />
    <van-grid :border="false" :column-num="3">
      <van-grid-item v-for="item in list" :key="item.id">
        <van-image width="111" height="100" :src="item.picUrl" />
        <p class="music-name van-multi-ellipsis--l2">{{ item.name }}</p>
      </van-grid-item>
    </van-grid>

    <van-cell class="title" title="最佳匹配" />
    <van-cell
      :label="`${
        (item.song &&
          item.song.artists &&
          item.song.artists[0] &&
          item.song.artists[0].name) ||
        '未知歌手'
      }-${item.name}`"
      :title="item.name"
      center
      v-for="item in Newsong"
      :key="item.id"
    >
      <template>
        <van-icon color="#000" name="play-circle-o" size="28" />
      </template>
    </van-cell>
  </div>
</template>

<script>
import { getRecommendListApi } from '@/apis';
import { getNewsongListApi } from '@/apis';
export default {
  name: 'VueMusicIndex',

  data() {
    return {
      list: [],
      Newsong: [],
    };
  },

  mounted() {
    this.getList();
    this.getNewsong();
  },

  methods: {
    //获取数据
    async getList() {
      try {
        const res = await getRecommendListApi({
          limit: 6,
        });
        console.log(res);
        this.list = res.data.result;
      } catch (e) {
        console.log('推荐歌曲获取失败');
      }
    },
    async getNewsong() {
      try {
        const res = await getNewsongListApi({});
        console.log(res);
        this.Newsong = res.data.result;
      } catch (e) {
        console.log('新歌数据获取失败');
      }
    },
  },
};
</script>

<style lang="less" scoped>
.home-container {
  .title {
    background-color: #c71d24;
    color: #fff;
  }

  .music-name {
    font-size: 12px;
    text-align: left;
    line-height: 20px;
    width: 100%;
    padding: 0 5px;
    box-sizing: border-box;
    height: 37px;
  }

  /deep/.van-grid-item__content {
    padding: 10px 0px;
  }
  .van-cell__value {
    // padding-top: 9px;
    flex: 0 0 48px;
  }
}
</style>
