<template>
  <div id="app">
    <myHeader />
    <myMain />
    <myFooter />
  </div>
</template>

<script>
import myHeader from "@/components/header";
import myFooter from "@/components/footer";
import myMain from "@/views/main";
export default {
  name: "App",
  components: {
    myHeader,
    myFooter,
    myMain,
  },
};
</script>

<style>
html {
  font-size: 62.5%;
}
body {
  box-sizing: border-box;
}
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
