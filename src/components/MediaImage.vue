<template>
  <li>
    <img :src="content.image" :style="imgClass"/>
  </li>
</template>
<script>
  export default {
    name: 'MediaImage',
    props: {
      content: {
        type: Object,
        default: function () { return {};}
      }
    },
    data () {
        let that = this;
        return {
            imgClass:{
              width:'100%',
              height:'auto'
            },
            iconTop:0,
            iconLeft:0,
            screenWidth:0
        }
    },
    created(){
      if(this.content.scaling > 0 && this.content.scaling < 1){//按照缩放比例值，对图片进行等比缩放
        this.imgClass.width = this.content.scaling*100+'%';
      }
    }
  }
</script>

<style scoped>
  li {
    box-sizing:border-box;
    margin-top:20px;
    position:relative;
  }
</style>
