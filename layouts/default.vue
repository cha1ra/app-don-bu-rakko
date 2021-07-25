<template>
  <v-app>
    <v-app-bar
      app
      dense
      elevation="1"
      color="#EDAAAA"
    >
      <nuxt-link
        to="/"
      >
        <img
          width="40"
          src="~/assets/dbk.png"
          alt=""
        >
      </nuxt-link>
      <v-spacer />
      <v-btn
        href="https://don-bu-rakko.com/"
        elevation="0"
      >
        ラッコWeb
      </v-btn>
    </v-app-bar>
    <v-main
      app
    >
      <Nuxt />
    </v-main>
  </v-app>
</template>
<style lang="scss" scoped>
.v-application {
  background-color: #ECEFF1;
}

</style>
