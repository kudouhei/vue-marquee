<template>
  <div>
    <marquee direction="up" mode='vertical' :interval='0' :duration='2000'>
      <marquee-item modelv='vertical' class="size" v-for='i in number' :key='i'>垂直滚起来-滚动的点点滴滴多点点滴滴地方-{{i}}</marquee-item>
    </marquee>
    <hr />
    <marquee direction="up" mode='horizontal' :interval='2000' :duration='4000'>
      <marquee-item modelv='horizontal' class="size" v-for='i in number' :key='i'>水平滚起来-滚动的点点滴滴多点点滴滴地方-{{i}}</marquee-item>
    </marquee>
  </div>
</template>
<script>
import Marquee from './components/Marquee'
import MarqueeItem from './components/Marquee-item'
export default {
    components: {
        Marquee,
        MarqueeItem
    },
    data () {
        return {
            number : 10
        }
    }
}
</script>
<style>
.size{
    font-size:24px;
}
</style>