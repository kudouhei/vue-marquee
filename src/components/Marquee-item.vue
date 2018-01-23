<template>
 
    <li :style="changeMode">
      <slot></slot>
    </li>

</template>

<script>
export default {
  name: 'marquee-item',
  mounted () {
    this.$nextTick(() => {
      this.$parent.destroy()
      this.$parent.init()
      this.$parent.start()
    })
  },
  props: {
    modelv :{
      type: String,
      default: "vertical"
    }
  },
  computed: {
    changeMode() {
      if(this.modelv === 'horizontal'){
        return 'display: inline'
      }else if(this.modelv === 'vertical'){
        return ''
      }
    }
  }
}
</script>