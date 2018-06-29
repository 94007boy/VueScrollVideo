<template>
  <ul class="rich-text-list">
    <li :is="comp.component" v-for="comp in components" :content="comp.content" ref="childrenComp"/>
  </ul>
</template>

<script type="text/ecmascript-6">

import * as type from "../utils/MediaTypes";
import MediaImage from './MediaImage';
import MediaText from './MediaText';
import MediaProduct from './MediaProduct';
import MediaInnerVideo from './MediaInnerVideo';

export default {
  name: 'BlogRichText',
  props: {
    mediaContents: {
      type: Array,
      default: function () { return [];}
    }
  },
  data () {
    return {
      components: []
    }
  },
  mounted() {
    let that = this;
    if(this.mediaContents && this.mediaContents.length){
      for(let i = 0;i < this.mediaContents.length;i++){
        let content = this.mediaContents[i];
        that.components.push({
          'component': that.getCompByType(content.type),
          'content':content
        })
      }
    }
  },
  methods: {
    getCompByType(contentType){
      if(contentType == type.IMAGE){
          return MediaImage;
      }else if(contentType == type.TEXT){
          return MediaText;
      }else if(contentType == type.PRODUCT_REF){
          return MediaProduct;
      }else if(contentType == type.VIDEO){
        return MediaInnerVideo;
      }
    }
  },
  components: {
    MediaImage,
    MediaText,
    MediaProduct,
    MediaInnerVideo
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .rich-text-list{
    padding:0px 10px;
    width:100%;
    box-sizing:border-box;
  }
  li {
    list-style-type:none;
  }
</style>
