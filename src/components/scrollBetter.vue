<template>
  <div class="wrapper">
      <div class="content">
        <slot></slot>
      </div>
  </div>
</template>



<script>
import BScroll from "better-scroll";
export default {
  name: "scrollBetter",
  components: {},
  data() {
    return {};
  },
  mounted() {
    const Scroll = new BScroll(".wrapper", {});
  }
};
</script>
<style lang='less' scoped>
.wrapper {
  .content::before{
    content: "";
    display: table;
  }
}
</style>