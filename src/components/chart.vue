<script>
import { Doughnut, mixins } from "vue-chartjs";
const { reactiveData } = mixins;
export default {
  name: "myChart",
  extends: Doughnut,
  mixins: [reactiveData],
  props: {
    data: null,
    options: null,
  },
  mounted: function () {
    this.chartData = this.data;
  },
  watch: {
    data: function () {
      this.chartData = this.data;
    },
  },
};
</script>