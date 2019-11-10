<template>
  <div>
    <header></header>
    <hr/>
    <div>
      这是首页，嘿嘿嘿。
    </div>
    <hr/>
    <footer></footer>
  </div>
</template>

<script>
import header from '@/components/common/Header.vue'
import footer from '@/components/common/Footer.vue'

export default {
  name: 'Index',
  // blogHeader/blogFooter组件给申明到components里面然后在template里面使用
  components: { header, footer }
}
</script>
