<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "Scroll",
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    //上拉加载更多
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },

  data() {
    return {
      scroll: null
    };
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    });
    //监听滚动位置
    // if (this.prototype === 2 || this.prototype === 3) {
      this.scroll.on("scroll", position => {
        // console.log(position);
        this.$emit("scroll", position);
      });
    // }
    // this.scroll.refresh()
    // console.log(this.scroll);
    if (this.pullUpLoad) {
      this.scroll.on("pullingUp", () => {
        console.log("上拉加载");
        this.$emit("pullingUp");
      });
    }
  },

  methods: {
    scrollTo(x, y, time = 300) {
      this.scroll && this.scroll.scrollTo(x, y, time);
    },
    finishPullUp() {
      this.scroll.finishPullUp();
    },
    refresh() {
      // console.log("上拉加载");
      this.scroll && this.scroll.refresh();
    },
    getScrollY(){
      return this.scroll? this.scroll.y:0
    }
  },

  components: {}
};
</script>

<style scoped>
</style>
