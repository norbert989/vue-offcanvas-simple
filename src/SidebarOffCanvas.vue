<template>
  <div class="sidebar"
    style="position: fixed; top: 0; bottom: 0; height: 100vh;"
    :style="[setHorizontal, { 'width': width + 'px', 'transform': translateWidth }]">
    <slot v-if="visible"></slot>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  data() {
    return {
      visible: false
    }
  },
  props: {
    value: {
      type: Boolean
    },

    align: {
      type: String,
      default: 'left'
    },

    width: {
      type: Number,
      default: 270
    },

    duration: {
      type: Number,
      default: 0.2
    },

    effect: {
      type: String,
      default: 'linear'
    }
  },

  watch: {
    'value' (val) {
      setTimeout(() => this.toggle(val))
    }
  },

  computed: {
    setHorizontal() {
      return this.align === 'right' ? { right: 0 } : { left: 0 }
    },

    translateWidth() {
      return this.align === 'right' ? 'translateX('+this.width+'px)' : 'translateX(-'+this.width+'px)'
    }
  },

  methods: {
    setupBody() {
      let timingFunction = ''

      switch(this.effect) {
        case 'bounce':
          timingFunction = 'cubic-bezier(0, 1.1, 0.8, 1.1)'
          break

        default:
          timingFunction = this.effect
          break
      }
      document.body.style.transition = 'transform '+this.duration+'s '+timingFunction

    },

    clickOutside(e) {
      if(!this.$el.contains(e.target) && this.visible) {
        this.$emit('sidebarWasClosed', false)
        this.toggle(false)
      }
    },

    esc(e) {
      if(e.keyCode === 27) {
        this.$emit('sidebarWasClosed', false)
        return this.toggle(false)
      }
    },

    toggle(show) {
      if(show) {
        document.addEventListener('click', this.clickOutside)
        document.addEventListener('keydown', this.esc)

        document.body.style.transform = this.align === 'right' ? 'translateX(-'+this.width+'px)' : 'translateX('+this.width+'px)'
        this.setupBody()
        this.visible = true
      } else {
        document.body.style.transform = 'translateX(0)'
        setTimeout(() => this.visible = false, this.duration * 500)
      }
    }
  }
}
</script>

<style lang="css" scoped>
  .sidebar {
    overflow-y: scroll;
  }
</style>
