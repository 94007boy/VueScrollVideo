<template>
  <div class="author-root">
    <Avatar :imgUrl="authorInfo.avatarImage" :size="40" :level="authorInfo.type" />
    <span class="author-name" >{{authorInfo.nickName}}</span>
    <button v-if="authorInfo.id !== uid" class="author-is-follow" :class="{active:isFollowed}" @click="onFollowBtnClick">{{isFollowed?'已关注':'关注'}}</button>
  </div>
</template>

<script type="text/ecmascript-6">

  import Avatar from './Avatar';

  export default {
    name: 'AuthorInfo',
    props: {
      authorInfo: {
        type: Object,
        default: function () { return {};}
      },
      uid: {
        type: String,
        default: function () { return "";}
      }
    },
    data(){
        return {
            isFollowed:this.authorInfo.currentUserIsFollowing === 1,
            lastClickTime:0
        }
    },
    methods: {
        onFollowBtnClick(){
            if(new Date().getTime() - this.lastClickTime < 500){//防止频繁点击
                this.lastClickTime = new Date().getTime();
                return;
            }
            this.lastClickTime = new Date().getTime();
            this.isFollowed = !this.isFollowed;
            window.androidjs.onFollowBtnClick();//vue_method需要注册到原生代码里
        },

        onAvatarClick(){
          window.androidjs.onAvatarClick();
        }
    },
    components: {
      Avatar
    }
  }
</script>
<style scoped>
  .author-root {
    display: flex;
    flex-direction: row;
    padding:10px;
    align-items:center;
  }
  img {
    width: 40px;height: 40px;border-radius: 50%;
  }
  .author-name{
    flex:1;
    font-size: 15px;
    color:#333333;
    padding-left:10px;
  }
  .author-is-follow{
    border:1px solid #e94715;
    border-radius: 5px;
    background:#fff;
    color:#e94715;
    width:60px;
    height:27px;
    display: flex;
    justify-content: center;
    align-items: center;
    outline:none;
  }
  .author-is-follow.active{
    outline:none;
    color:#999;
    border:1px solid #999;
  }
</style>
