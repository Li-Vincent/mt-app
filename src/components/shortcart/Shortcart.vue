<template>
    <div class="shortcart-wrapper">
        <!-- 购物车左侧 -->
        <div class="content-left">
            <div class="logo-wrapper">
                <span class="icon-shopping_cart logo">
                </span>
            </div>
            <div class="desc-wrapper">
                <p class="tip">另需{{poiInfo.shipping_fee_tip}}</p>
            </div>
        </div>
        <!-- 购物车右侧 -->
        <div class="content-right">
            {{poiInfo.min_price_tip}}
        </div>

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

<style>
@import url(../../common/css/icon.css);

.shortcart-wrapper {
  width: 100%;
  height: 51px;
  background: #514f4f;
  position: fixed;
  left: 0;
  bottom: 0;
  display: flex;
  z-index: 99;
}
.shortcart-wrapper .content-left {
  flex: 1;
}
.shortcart-wrapper .content-right {
  flex: 0 0 110x;
  font-size: 15px;
  color: #bab9b9;
  line-height: 51px;
  text-align: center;
  font-weight: bold;
}
.shortcart-wrapper .content-left .logo-wrapper {
  width: 50px;
  height: 50px;
  background: #666;
  border-radius: 50%;
  position: relative;
  top: -14px;
  left: 10px;
  text-align: center;
  float: left;
}
.shortcart-wrapper .content-left .logo-wrapper .logo {
  font-size: 28px;
  color: #c4c4c4;
  line-height: 50px;
}
.shortcart-wrapper .content-left .desc-wrapper {
  float: left;
  margin-left: 13px;
}
.shortcart-wrapper .content-left .desc-wrapper .tip {
  font-size: 12px;
  color: #bab9b9;
  line-height: 50px;
}
</style>
