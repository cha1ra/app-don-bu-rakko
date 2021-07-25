<template>
  <v-container>
    <section class="my-6">
      <div class="mb-2 title font-weight-bold">
        Donburakko Applications
      </div>
      <div class="body-2">
        開発した便利ツールを掲載していきます
      </div>
    </section>
    <v-row>
      <v-col
        v-for="app in apps"
        :key="app.href"
        cols="12"
        sm="6"
      >
        <v-card
          nuxt
          elevation="1"
          :to="app.href"
          class="pa-4"
        >
          <div class="font-weight-bold mb-2">
            {{ app.title }}
          </div>
          <div class="caption grey--text text--darken-1">
            {{ app.description }}
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import {
  defineComponent,
  ref
} from '@nuxtjs/composition-api'

interface App {
  href: string
  title: string
  description: string
}

export default defineComponent({
  setup () {
    const apps = ref<App[]>([
      {
        href: '/zenkaku-hankaku',
        title: '全角→半角変換ツール',
        description: '全角カタカナを半角カタカナに変換するツールです'
      }
    ])

    return {
      apps
    }
  }
})
</script>

<style lang="scss">

</style>
