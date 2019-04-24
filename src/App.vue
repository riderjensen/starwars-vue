<template>
  <v-app>
    <v-content>
      <v-container style="padding: 0; padding-bottom: 20px" fluid fill-height>
        <v-layout justify-center>
          <v-flex text-xs-center>
            <router-view/>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>  
</template>

<script>
export default {
  name: "App",
 
}
</script>

<style>

</style>
