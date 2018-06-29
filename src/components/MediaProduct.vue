<template>
  <li>
    <div class="media-product-root">
      <img :src="content.image">
      <div class="media-product-right">
        <p class="media-product-title" v-html="content.content"></p>
        <p class="media-product-subtitle" v-html="content.subTitle"></p>
        <div class="media-product-bottom">
          <span>{{content.price}}</span>
          <button>立即抢购</button>
        </div>
      </div>
    </div>
  </li>
</template>
<script>
  export default {
    name: 'MediaProduct',
    props: {
      content: {
        type: Object,
        default: function () { return {};}
      },
      screenHeight:0
    }
  }
</script>

<style scoped>
  li {
    box-sizing:border-box;
    width:100%;
    margin-top:20px;
  }
  .media-product-root{
    height:130px;
    border:1px solid #ddd;
    background:#fff;
    display: flex;
    flex-direction: row;
    align-items:center;
  }
  .media-product-right{
    flex:1;
    padding:10px 15px 10px 13px;
    display: flex;
    flex-direction: column;
    align-items:left;
  }
  img {
    width:110px;
    height:110px;
    margin-left:10px;
  }
  .media-product-title{
    font-size: 14px;
    color:#333;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .media-product-subtitle{
    font-size: 12px;
    color:#e94715;
    margin-top:8px;
  }
  .media-product-bottom{
    display: flex;
    flex-direction: row;
    align-items:center;
    justify-content:space-between;
    margin-top:30px;
    width:100%;
  }
  .media-product-bottom span{
    font-size: 16px;
    color:#e94715;
  }
  .media-product-bottom button{
    font-size: 15px;
    color:#e94715;
    border:1px solid #e94715;
    border-radius: 5px;
    width:90px;
    height:25px;
    display: flex;
    flex-direction: column;
    align-items:center;
    background:#fff;
  }
</style>
