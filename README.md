# vue-offcanvas-simple
A basic vuejs off canvas sidebar component

## Installation

```
$ npm install vue-offcanvas-simple --save
```


## Examples

```vue
<template>
  <div>
    <off-canvas v-model="show" :width="300" :duration=".5" effect="ease-in-out" @sidebarWasClosed="show = $event">
        Sidebar Content goes here
    </off-canvas>
    <div>
          <button @click="show = !show">Toggle</button><button @click="show = false">Close</button>
          <p>{{ show }}</p>
    </div>
  </div>
</template>

<script>
import offCanvas from 'vue-offcanvas-simple/src/SidebarOffCanvas.vue'

export default {
  data() {
    return {
      show: false
    }
  },
  components: {
    offCanvas
  }
}
</script>

```
