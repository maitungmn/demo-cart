<template>
 <v-card flat>
    <v-toolbar
      dark
      extended
      flat
      class="toolbar-pri"
    >
      <v-toolbar-side-icon></v-toolbar-side-icon>
    </v-toolbar>

    <v-layout row pb-2>
      <v-flex xs10 offset-xs1>
        <v-card class="card--flex-toolbar">
          <v-toolbar card prominent>
            <v-toolbar-title class="title black--text font-weight-light over-show">El-Meteo - Shopping Cart</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn icon>
              <v-icon>search</v-icon>
            </v-btn>

            <v-btn icon>
              <v-icon>apps</v-icon>
            </v-btn>

            <v-btn icon>
              <v-icon>more_vert</v-icon>
            </v-btn>
          </v-toolbar>

          <v-divider></v-divider>

          <v-app>
            <nuxt></nuxt>
          </v-app>
        </v-card>
      </v-flex>
    </v-layout>
  </v-card>
</template>

<script>
  export default {}
</script>
<style lang="scss">
 .toolbar-pri{
  &.theme--dark{
   &.v-toolbar{
    background-color: #6394F8;
   }
  }
 }
 .over-show{
  &.title{
   overflow: visible;
  }
 }
 .card--flex-toolbar {
   margin-top: -64px;
 }
</style>
