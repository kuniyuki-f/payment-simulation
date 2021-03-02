<template>
  <div>
    <input type="range" v-model="innerVal" :min="min" :max="max" />
    <vue-slider v-model="innerVal" :min="min" :max="max" />
  </div>
</template>

<script>
export default {
  name: "myRange",
  props: {
    value: { type: Number, required: true, default: 0 },
    min: { type: Number, required: true, default: 0 },
    max: { type: Number, required: true, default: 0 },
    step: { type: Number, required: true, default: 0 },
  },
  data: function () {
    return {
      dataValue: this.value,
    };
  },
  computed: {
    innerVal: {
      get() {
        return this.value;
      },
      set(value) {
        value = Number(value);
        this.$emit("update:value", value);
      },
    },
  },
};
</script>

<style lang="scss" scoped>
</style>
