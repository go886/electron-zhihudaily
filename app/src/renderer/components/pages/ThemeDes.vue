<template lang="html">
  <div>
    <zhihu-header :icon="headerIcon" :title="headerTitle"></zhihu-header>
    <!-- 主题日报文章列表 -->
    <artical-list v-if="themeData.name" class="themeArticalList-wrap" :stories-data="themeData"></artical-list>
    <!-- 加载动画 -->
    <loading loading-style="loadingStyle"></loading>
  </div>
</template>


<script>
import ajax from '../../../service/http.js'
import zhihuHeader from 'components/ui/Header'
import ArticalList from 'components/ui/ArticalList'
import Loading from 'components/ui/Loading'
export default {
  name: "ThemeDes",
  data () {
    return {
      headerIcon: 'close',
      headerTitle: '',
      themeData: [],
      url: {
        url: '/api/v1' + this.$route.path
      },
      ajax,
      loadingStyle: 'white-purple'
    };
  },
  components: {
    zhihuHeader,
    ArticalList,
    Loading
  },
  mounted () {
    let dbData = sessionStorage.getItem(this.$route.path);
    if(dbData) {
      this.themeData = JSON.parse(dbData);
      this.headerTitle = this.themeData.name;
    } else {
      ajax.get(this.url)
      .then(response => {
        this.themeData = response.data.THEMEDES;
        this.headerTitle = this.themeData.name;
        let str = JSON.stringify(this.themeData);
        sessionStorage.setItem(this.$route.path, str);
      })
      .catch(error => {
        console.log(error);
      });
    }
  }
}
</script>


<style lang="less" scoped>
  .themeArticalList-wrap {
    margin-top: 100px;
  }

  .loading {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>