<template>
  <div class="cart-list">
    <scroll class="content" ref="scroll">
      <cart-list-item v-for="(item, index) in cartList" :product="item" :itemIndex="index" :key="index" @CartImgLoad="CartImgLoad"></cart-list-item>
    </scroll>
  </div>
</template>

<script>
import { mapGetters } from "vuex"
import { debounce } from "@/common/utils"
import Scroll from "components/common/scroll/Scroll"
import CartListItem from "./CartListItem.vue"
export default {
  name: "CartList",
  computed: {
    ...mapGetters(["cartList"]),
  },
  data () {
    return {
      debounceRefresh: debounce(this.cartRefresh, 50),
    }
  },
  methods: {
    cartRefresh () {
      this.$refs.scroll.refresh()
    },
    CartImgLoad () {
      this.debounceRefresh()
    },
  },
  components: { Scroll, CartListItem },
  activated () {
    this.$refs.scroll && this.cartRefresh()
  },
};
</script>

<style scoped>
.cart-list {
  background-color: #fff;
}
.content {
  height: calc(100vh - 44px - 49px - 44px);
  overflow: hidden;
}
</style>