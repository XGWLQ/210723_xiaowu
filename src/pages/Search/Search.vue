<template>
  <div class="search">
    <Headertop title="搜索" />
    <form class="search_form"
          action="#"
          @submit.prevent="seatch">
      <input type="search"
             name="search"
             placeholder="请输入商家或美食名称"
             v-model="keyword"
             class="search_input">
      <input type="submit"
             name="submit"
             class="search_submit">
    </form>
    <div class="list"
         v-if="!noSearchShops">
      <ul class="list_container">
        <!--:to="'/shop?id='+item.id"-->
        <router-link :to="{path:'/shop', query:{id:item.id}}"
                     tag="li"
                     v-for="item in searchShops"
                     :key="item.id"
                     class="list_li">
          <div class="item_left">
            <img :src="imgBaseUrl + item.image_path"
                 class="restaurant_img">
          </div>
          <div class="item_right">
            <div class="item_right_text">
              <p>
                <span>{{item.name}}</span>
              </p>
              <p>月售 {{item.month_sales||item.recent_order_num}} 单</p>
              <p>{{item.delivery_fee||item.float_minimum_order_amount}} 元起送 / 距离{{item.distance}}</p>
            </div>
          </div>
        </router-link>
      </ul>
    </div>
    <div class="search_none"
         v-else>很抱歉！无搜索结果</div>
  </div>
</template>
<script>
import Headertop from '../../components/HeaderTop/headertop.vue'
import { mapState } from 'vuex'
export default {
  data () {
    return {
      keyword: '',
      imgBaseUrl: 'http://cangdu.org:8001/img/', // 图片基本前缀地址
      noSearchShops: false
    }
  },
  computed: {
    ...mapState(['searchShops'])
  },
  methods: {
    seatch () {
      // 拿到搜索的数据
      const keyword = this.keyword.trim()
      // 如果有值获取查询
      if (keyword) {
        // this.noSearchShops = false
        this.$store.dispatch('searchShops', { keyword })
      }
    }
  },
  watch: {
    // 监视是否有值
    searchShops (val) {
      this.noSearchShops = !val.length
    }
  },
  components: {
    Headertop
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixins.styl';

.search {
  width: 100%;
  height: 100%;
  overflow: hidden;

  .search_form {
    clearFix();
    margin-top: 45px;
    background-color: #fff;
    padding: 12px 8px;

    input {
      height: 35px;
      padding: 0 4px;
      border-radius: 2px;
      font-weight: bold;
      outline: none;

      &.search_input {
        float: left;
        width: 79%;
        border: 4px solid #f2f2f2;
        font-size: 14px;
        color: #333;
        background-color: #f2f2f2;
      }

      &.search_submit {
        float: right;
        width: 18%;
        border: 4px solid #02a774;
        font-size: 16px;
        color: #fff;
        background-color: #02a774;
      }
    }
  }

  .list {
    .list_container {
      background-color: #fff;

      .list_li {
        display: flex;
        justify-content: center;
        padding: 10px;
        border-bottom: 1px solid $bc;

        .item_left {
          margin-right: 10px;

          .restaurant_img {
            width: 50px;
            height: 50px;
            display: block;
          }
        }

        .item_right {
          font-size: 12px;
          flex: 1;

          .item_right_text {
            p {
              line-height: 12px;
              margin-bottom: 6px;

              &:last-child {
                margin-bottom: 0;
              }
            }
          }
        }
      }
    }
  }

  .search_none {
    margin: 0 auto;
    color: #333;
    background-color: #fff;
    text-align: center;
    margin-top: 0.125rem;
  }
}
</style>
