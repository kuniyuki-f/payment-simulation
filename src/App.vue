<template>
  <v-app>
    <myHeader />
    <v-main>
      <myContents />
    </v-main>
    <myFooter />
  </v-app>
</template>

<script>
import myHeader from "@/components/header";
import myFooter from "@/components/footer";
import myContents from "@/views/contents";

export default {
  name: "App",

  components: {
    myHeader,
    myContents,
    myFooter,
  },

  data: () => ({
    //
  }),
};
</script>

<style lang="scss">
html {
  font-size: 62.5%;
  box-sizing: border-box;
}
* {
  padding: 0;
  margin: 0;
}
</style>