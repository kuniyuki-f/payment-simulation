<template>
  <v-footer color="primary"><p>©2021 Kuniyuki Funasaka</p></v-footer>
</template>

<script>
export default {
  name: "myFooter",
};
</script>

<style lang="scss" scoped>
footer {
  position: fixed;
  width: 100%;
  bottom: 0;

  p {
    color: white;
    margin: auto;
  }
}
</style>