<template>
  <div :style="getAvatarClass()" >
    <img class="avatar"  :src="getAvatarImg()">
    <img class="level" v-if="isShowLevel()" :src="getLevelImg()" :style="getLevelClass()"/>
  </div>
</template>
<script>
  import * as levelType from "../utils/UserLevel";
  export default {
    name: 'Avatar',
    props: {
      imgUrl: {
        type: String,
        default: function () { return "";}
      },
      size: {
        type: Number,
        default: function () { return 0;}
      },
      level: {
        type: Number,
        default: function () { return 0;}
      }
    },
    methods: {
      getAvatarClass(){
          return{
            width:parseInt(this.size)+'px',
            height:parseInt(this.size)+'px'
          }
      },
      getLevelClass(){
        return{
          width:parseInt(this.size/3)+'px',
          height:parseInt(this.size/3)+'px'
        }
      },
      getLevelImg(){
          if(this.level === levelType.ENTERPRISE_CERTIFIED){
            return require('../assets/v_gov.png')
          }else if(this.level === levelType.KOL_CERTIFIED){
            return require('../assets/v_kol.png')
          }else{
            return require('../assets/v_pgc.png')
          }
      },
      getAvatarImg(){
          if(!!this.imgUrl){
              return this.imgUrl
          }else{
            return require('../assets/default_header.png')
          }
      },
      isShowLevel(){
          return !(this.level === levelType.COMMON);
      },
      onAvatarClick(){
        window.androidjs.onAvatarClick();
      }
    }
  }
</script>
<style scoped>

  div{
    position:relative;
    margin-right:10px;
  }

  .avatar{
    width: 100%;
    height:100%;
    border-radius: 50%;
  }

  .level{
    position: absolute;
    right: 0px;
    bottom: 0px;
  }

</style>
