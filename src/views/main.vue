<template>
  <main>
    <myRange
      :value="price"
      @update:value="price = $event"
      :min="300"
      :max="10000"
      :step="10"
    />
    {{ price }}
  </main>
</template>

<script>
import myRange from "@/components/range";
export default {
  name: "myMain",
  components: {
    myRange,
  },
  data() {
    return {
      price: 3000,
    };
  },
};
</script>