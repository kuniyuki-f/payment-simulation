<template>
  <div class="v-slider-wrapper">
    <v-slider
      v-model="innerVal"
      :value="innerVal"
      :min="min"
      :max="max"
      :step="step"
      color="#FAFAFA"
      track-color="#9E9E9E"
    />
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
.v-slider-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;

  height: 5.6rem;

  margin: auto;
  padding: 2rem 2rem 0 2rem;

  border-radius: 0.5rem 0.5rem 2rem 2rem;
}
</style>
