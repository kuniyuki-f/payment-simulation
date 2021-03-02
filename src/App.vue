<template>
  <div id="app">
    <myHeader />
    <myFooter />
  </div>
</template>

<script>
import myHeader from "@/components/header";
import myFooter from "@/components/footer";
export default {
  name: "App",
  components: {
    myHeader,
    myFooter,
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
