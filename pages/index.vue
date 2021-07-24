<template>
  <div>
    test
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  reactive
} from '@nuxtjs/composition-api'

export default defineComponent({
  setup () {
    const flag = ref()
    const state = reactive({

    })
    return {
      flag,
      state
    }
  }
})
</script>

<style lang="scss">

</style>
