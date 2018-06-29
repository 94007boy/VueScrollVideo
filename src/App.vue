<template>
  <div id="app" v-if="detailRes">
    <div class="blog-cover-img-root">
      <img class="blog-cover-img"  :src="detailRes.coverImg" />
      <div class="blog-cover-img-cover"/>
    </div>
    <AuthorInfo :authorInfo="detailRes.owner" :uid="detailRes.uid"/>
    <div class="splite-line"/>
    <h1 class="blog-title" v-html="detailRes.title"></h1>
    <div class="blog-sub-title">
      <span class="blog-sub-title-time">{{!detailRes?'':detailRes.createdTime}}</span>
      <span class="blog-sub-title-read">浏览量{{!detailRes?'0':detailRes.readCount}}</span>
    </div>
    <BlogRichText :mediaContents="detailRes.mediaContents"/>
  </div>
</template>

<script>
  import AuthorInfo from './components/AuthorInfo'
  import BlogRichText from './components/BlogRichText'
  import VueResource from 'vue-resource';//解析本地json文件用的,作为模拟请求的数据
  import Vue from 'vue';
  import VueBus from 'vue-bus';

  Vue.use(VueBus);
  Vue.use(VueResource);

export default {
  name: 'App',
  data () {
    return {
      detailRes:null,//帖字数据
    }
  },
  created(){
    let that = this;
    this.$http.get('../static/myjson.json')
      .then(res => {
        that.detailRes = res.body;
      })
      .catch(e => {
          console.error(e);
      })
  },
  mounted(){
    this.$nextTick(function () {
      window.addEventListener('scroll', this.onScroll)
    })
  },
  methods: {
    onScroll() {
      let scrolled = document.documentElement.scrollTop || document.body.scrollTop;
      this.$bus.emit('handleScroll',{scrollTop:scrolled}); //Hub触发事件
    },
  },
  components: {
    AuthorInfo,
    BlogRichText
  }
}
</script>

<style>
  *{margin: 0;padding: 0;box-sizing:border-box;}
  .blog-cover-img-root{
    width:vw;
    height:calc(100vw/16*9);
    position:relative;
  }
  .blog-cover-img-cover {
    position: absolute;
    background-color:rgba(0,0,0,0.3);
    width:100%;
    height:100%;
  }
  .blog-cover-img {
    position:absolute;
    max-height:100%;
    max-width:100%;
  }
  .splite-line{
    width: 100%;
    height: 1px;
    background:#ddd;
  }
  .blog-title{
    font-size: 25px;
    color:#000;
    padding:20px 10px;
  }
  .blog-sub-title{
    display: flex;
    flex-direction: row;
    justify-content:space-between;
    font-size: 11px;
    color:#999;
    padding:0px 10px 0px 10px;
  }
</style>
