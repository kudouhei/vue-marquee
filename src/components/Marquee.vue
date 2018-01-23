<template>
  <div>
    <div v-if="mode==='vertical'">
      <div class="vux-marquee" :style="{height: height + 'px',width: width+'px'}" >
        <ul class="vux-marquee-box" ref="box" :style="{transform: `translate3d(0,${currenTranslateY}px,0)`, transition: `transform ${noAnimate ? 0 : duration}ms`}">
          <!-- <li class='bb' v-for="i in number" :key = "i" >混无霹雳手点点滴滴的点点滴滴点点滴滴-ziksang{{i}}</li> -->
          <slot></slot>
        </ul>
      </div>
    </div>

    <div v-if="mode==='horizontal'">
        <div class="vux-marquee" :style="{width: width + 'px'}">
          <ul class="vux-marquee-box" ref="box" :style="{transform: `translate3d(${currenTranslateX}px,0,0)`, transition: `transform ${noAnimate ? 0 : duration}ms linear`}">
            <!-- <li class='bb' v-for="i in number" :key = "i" :style="{display:'inline'}">混无霹雳手点点滴滴的点点滴滴点点滴滴-ziksang{{i}}</li> -->
            <slot></slot>
          </ul>
        </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "marquee",
  props: {
    interval: {
      // 每个marquee条目间隔
      type: Number,
      default: 1000
    },
    duration: {
      // 对动画时长设置
      type: Number,
      default: 1000
    },
    direction: {
      // 对上下marquee配置
      type: String,
      default: "down"
    },
    mode: {
      type: String,
      default: "vertical" // horizontal水平 vertical垂直
    },
    itemHeight: Number,
    itemWidth: Number
  },
  beforeDestroy() {
    this.destroy();
  },
  data() {
    return {
      currenTranslateY: 0, // 改变的y轴
      height: "", // 高度存放值
      length: 0, // marquee条目的总长度
      currentIndex: 0, // 当前每个条目的index下标值存放
      noAnimate: false, // 是否是进行动画的配置值
      width: "",
      currenTranslateX: 0,
      number: 10
    };
  },
  methods: {
    destroy() {
      this.timer && clearInterval(this.timer);
    },
    init() {
      this.destroy();
      if (this.cloneNode) {
        this.$refs.box.removeChild(this.cloneNode);
      }
      this.cloneNode = null;
      let firstItem = this.$refs.box.firstElementChild;
      if (!firstItem) {
        return false;
      }
      this.length = this.$refs.box.children.length;
      this.height = this.itemHeight || firstItem.offsetHeight;
      this.width = this.itemWidth || firstItem.offsetWidth;
      if (this.direction === "up") {
        // 如果向上滚动，就深度复制第一条目进行最后节点插入
        this.cloneNode = firstItem.cloneNode(true);
        this.$refs.box.appendChild(this.cloneNode);
      } else {
        // 如果的down，就对最后一条目深度复制插入到第一个条目前。
        this.cloneNode = this.$refs.box.lastElementChild.cloneNode(true);
        this.$refs.box.insertBefore(this.cloneNode, firstItem);
      }
      return true;
    },
    start() {
      if (this.direction === "down") this.go(false);
      this.timer = setInterval(() => {
        if (this.direction === "up") {
          this.currentIndex += 1;
          this.currenTranslateY = -this.currentIndex * this.height;
          this.currenTranslateX = -this.currentIndex * this.width;
        } else {
          this.currentIndex -= 1;
          this.currenTranslateY = -(this.currentIndex + 1) * this.height;
          this.currenTranslateX = (this.currentIndex + 1) * this.width;
        }
        if (this.currentIndex === this.length) {
          setTimeout(() => {
            this.go(true);
          }, this.duration);
        } else if (this.currentIndex === -1) {
          setTimeout(() => {
            this.go(false);
          }, this.duration);
        } else {
          this.noAnimate = false;
        }
      }, this.interval + this.duration);
    },
    go(toFirst) {
      this.noAnimate = true;
      if (toFirst) {
        // 向上滚动
        this.currentIndex = 0;
        this.currenTranslateY = 0;
        this.currenTranslateX = 0;
      } else {
        // 向下滚动
        this.currentIndex = this.length - 1;
        this.currenTranslateY = -(this.currentIndex + 1) * this.height;
        this.currenTranslateX = -(this.currentIndex + 1) * this.width;
      }
    }
  }
};
</script>
<style>
.vux-marquee {
  width: 100%;
  overflow: hidden;
}
.vux-marquee-box {
  padding: 0;
  margin: 0;
  width: 100%;
  height: auto;
  white-space: nowrap;
}
.vux-marquee-box li {
  margin: 0;
  /* display: inline; */
  padding: 0 10px;
}
</style>