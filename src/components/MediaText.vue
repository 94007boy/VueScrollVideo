<template>
  <li>
    <p v-if="content.isHtml"><span v-html="content.content"></span></p>
    <p v-else class="media-pure-text" v-html="content.content"></p>
  </li>
</template>
<script>
  export default {
    name: 'MediaText',
    props: {
      content: {
        type: Object,
        default: function () { return {};}
      }
    }
  }
</script>

<style scoped>
  li {
    box-sizing:border-box;
    margin-top:20px;
  }
  p {
    width:100%;
    height: auto;
    display: block;
  }
  .media-pure-text{
    font-size: 16px;
    color:#333;
  }
</style>
