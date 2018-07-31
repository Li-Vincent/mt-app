<template>
    <div class="header">
        <!-- 顶部通栏 -start -->
        <div class="top-wrapper">
            <div class="back-wrapper">
                <span class="icon-arrow_lift"></span>
            </div>

            <form class="search-wrapper">
                <span class="search-icon"></span>
                <input type="text" class="search-bar" placeholder="搜索店内的商品">
            </form>

            <div class="more-wrapper">
                <a href="#" class="spelling-bt">拼单</a>
                <div class="more-bt">
                    <i class="s-radius"></i>
                    <i class="s-radius"></i>
                    <i class="s-radius"></i>
                </div>
            </div>
        </div>
        <!-- 顶部通栏 -end -->
    </div>
</template>

<script>
export default {
  props: {
    poiInfo: {
      type: Object,
      default: {}
    }
  }
};
</script>

<style scoped>
@import url(../../common/css/icon.css);
</style>
