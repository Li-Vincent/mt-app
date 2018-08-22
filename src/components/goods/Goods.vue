<template>
    <div class="goods">
        <!-- 分类列表 -->
        <div class="menu-wrapper" ref="menuScroll">
            <ul>
                <!-- 专场 -->
                <li class="menu-item" :class="{'current':currentIndex ===0}" @click="selectMenu(0)">
                    <p class="text">
                        <img class="icon" :src="container.tag_icon" v-if="container.tag_icon"> {{container.tag_name}}
                    </p>
                </li>
                <!-- 具体分类 -->
                <li class="menu-item" :class="{'current':currentIndex ===index+1}" v-for="(item,index) in goods" :key="index" @click="selectMenu(index+1)">
                    <p class="text">
                        <img class="icon" :src="item.icon" v-if="item.icon"> {{item.name}}
                    </p>
                </li>
            </ul>
        </div>
        <!-- 商品列表 -->
        <div class="foods-wrapper" ref="foodScroll">
            <ul>
                <!-- 专场 -->
                <li class="container-list food-list-hook">
                    <div v-for="(item,index) in container.operation_source_list" :key="index">
                        <img :src="item.pic_url">
                    </div>
                </li>
                <!-- 具体分类 -->
                <li class="food-list food-list-hook" v-for="(item,index) in goods" :key="index">
                    <h3 class="title">
                        {{item.name}}
                    </h3>
                    <!-- 具体的商品列表 -->
                    <ul>
                        <li v-for="(food,index) in item.spus" :key="index" class="food-item">
                            <div class="icon" :style="head_bg(food.picture)"></div>
                            <div class="content">
                                <h3 class="name">{{food.name}}</h3>
                                <p class="desc" v-if="food.desciption">{{food.desciption}}</p>
                                <div class="extra">
                                    <span class="saled">{{food.month_saled_content}}</span>
                                    <span class="praise">{{food.praise_content}}</span>
                                </div>
                                <img :src="food.product_label_picture" class="product">
                                <p class="price">
                                    <span class="text">￥{{food.min_price}}</span>
                                    <span class="unit">/{{food.unit}}</span>
                                </p>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <app-cart-control :food="food"></app-cart-control>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>

        </div>
        <!-- 购物车 -->
        <app-shortcart :poiInfo="poiInfo"></app-shortcart>
    </div>
</template>

<script>
import BScroll from "better-scroll";
import Shortcart from "../shortcart/Shortcart";
import CartControl from "../cartcontrol/CartControl";

export default {
  data() {
    return {
      container: {},
      goods: [],
      listHeight: [],
      menuScroll: {},
      foodScroll: {},
      scrollY: 0,
      poiInfo: {}
    };
  },
  components: {
    appShortcart: Shortcart,
    appCartControl: CartControl
  },
  methods: {
    head_bg(imgName) {
      return "background-image: url(" + imgName + ");";
    },
    selectMenu(index) {
      let foodlist = this.$refs.foodScroll.getElementsByClassName(
        "food-list-hook"
      );
      let element = foodlist[index];
      this.foodScroll.scrollToElement(element, 250);
    },
    initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuScroll, {
        click: true
      });
      this.foodScroll = new BScroll(this.$refs.foodScroll, {
        probeType: 3,
        click: true
      });

      // foodScroll监听事件
      this.foodScroll.on("scroll", pos => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    // 计算分类区间高度
    calculateHeight() {
      // 获取元素
      let foodlist = this.$refs.foodScroll.getElementsByClassName(
        "food-list-hook"
      );
      let height = 0;
      this.listHeight.push(height);
      for (let index = 0; index < foodlist.length; index++) {
        let item = foodlist[index];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  },
  created() {
    fetch("/api/goods")
      .then(res => res.json())
      .then(response => {
        if (response.code == 0) {
          this.container = response.data.container_operation_source;
          this.goods = response.data.food_spu_tags;
          this.poiInfo = response.data.poi_info;
          // DOM已经更新之后执行
          this.$nextTick(() => {
            // 执行滚动方法
            this.initScroll();
            // 计算分类区间高度
            this.calculateHeight();
            // 监听滚动位置
            // 根据滚动位置确认下标，与左侧对应
            // 通过下标实现点击左侧，滚动右侧
          });
        }
      });
  },
  computed: {
    currentIndex() {
      for (let index = 0; index < this.listHeight.length; index++) {
        //获取商品区间高度范围
        let height1 = this.listHeight[index];
        let height2 = this.listHeight[index + 1];
        if (!height2 || (this.scrollY >= height1 && this.scrollY <= height2)) {
          return index;
        }
      }
      return 0;
    }
  }
};
</script>

<style>
.goods {
  display: flex;
  position: absolute;
  top: 190px;
  bottom: 51px;
  overflow: hidden;
  width: 100%;
}
.goods .menu-wrapper {
  flex: 0 0 85px;
  background: #f4f4f4;
}
.goods .foods-wrapper {
  flex: 1;
  /* background: blue; */
}

/* Menu item */
.goods .menu-wrapper .menu-item {
  padding: 16px 23px 15px 10px;
  border-bottom: 1px solid #e4e4e4;
}
.goods .menu-wrapper .menu-item .text {
  font-size: 13px;
  color: #333333;
  line-height: 17px;
  vertical-align: middle;
  /* 只展示2行 */
  -webkit-line-clamp: 2;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.goods .menu-wrapper .menu-item .text .icon {
  width: 15px;
  height: 15px;
  vertical-align: middle;
}

/*专场样式*/
.goods .foods-wrapper .container-list {
  padding: 11px 11px 0 11px;
  border-bottom: 1px solid #e4e4e4;
}
.goods .foods-wrapper .container-list img {
  width: 100%;
  margin-bottom: 11px;
  border-radius: 5px;
}

/*具体分类商品布局*/
.goods .foods-wrapper .food-list {
  padding: 11px;
}
.goods .foods-wrapper .food-list .title {
  height: 13px;
  font-size: 13px;
  background: url(./img/btn_yellow_highlighted@2x.png) no-repeat left center;
  background-size: 2px 10px;
  padding-left: 7px;
  margin-bottom: 12px;
}
.goods .foods-wrapper .food-list .food-item {
  display: flex;
  margin-bottom: 25px;
  position: relative;
}
.goods .foods-wrapper .food-list .food-item .icon {
  flex: 0 0 63px;
  background-position: center;
  background-size: 120% 100%;
  background-repeat: no-repeat;
  margin-right: 11px;
  height: 75px;
}
.goods .foods-wrapper .food-list .food-item .content {
  flex: 1;
}

/*具体内容样式*/
.goods .foods-wrapper .food-list .food-item .content .name {
  font-size: 16px;
  line-height: 21px;
  color: #333;
  margin-bottom: 10px;
  padding-right: 27px;
}
.goods .foods-wrapper .food-list .food-item .content .desc {
  font-size: 10px;
  line-height: 19px;
  color: #bfbfbf;
  margin-bottom: 8px;
  /* 超出部分显示省略号*/
  -webkit-line-clamp: 1;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.goods .foods-wrapper .food-list .food-item .content .extra {
  font-size: 10px;
  color: #bfbfbf;
  margin-bottom: 7px;
}
.goods .foods-wrapper .food-list .food-item .content .extra .saled {
  margin-right: 14px;
}
.goods .foods-wrapper .food-list .food-item .content .product {
  height: 15px;
  margin-bottom: 6px;
}
.goods .foods-wrapper .food-list .food-item .content .price {
  font-size: 0;
}
.goods .foods-wrapper .food-list .food-item .content .price .text {
  font-size: 14px;
  color: #fb4344;
}
.goods .foods-wrapper .food-list .food-item .content .price .unit {
  font-size: 12px;
  color: #bfbfbf;
}

/*当前选中*/
.goods .menu-wrapper .menu-item.current {
  background: white;
  font-weight: bold;
  margin-top: -1px;
}
.goods .menu-wrapper .menu-item:first-child.current {
  margin-top: 1px;
}

.goods .foods-wrapper .food-list .food-item .cartcontrol-wrapper {
  position: absolute;
  right: 0;
  bottom: 0;
}
</style>
