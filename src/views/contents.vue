<template>
  <div class="container">
    <div class="result">
      <h2 class="monthRatePayment">
        金利<br />
        <strong>{{ result["monthRatePayment"] }}<br /></strong>
        万円/月
      </h2>
      <myChart
        class="chart"
        :data="data"
        :options="options"
        :width="240"
        :height="100"
      />

      <h2 class="resultPayment">
        月々の総支払額<br />
        <strong> {{ result["resultPayment"] }}<br /></strong>
        万円/月
      </h2>

      <h2 class="monthSalaryPayment">
        元金<br />
        <strong>{{ result["monthSalaryPayment"] }}<br /> </strong>万円
      </h2>
    </div>

    <div class="price common">
      <div class="heading">
        <p>物件価格</p>
        <p>
          <strong>{{ price.toLocaleString() }}</strong
          >万円
        </p>
      </div>
      <myRange
        :value="price"
        @update:value="price = $event"
        :min="300"
        :max="10000"
        :step="10"
        style="background-color: #009688"
      />
    </div>
    <div class="rate common">
      <div class="heading">
        <p>金利</p>
        <p>
          <strong>{{ rate }}</strong
          >％
        </p>
      </div>
      <myRange
        :value="rate"
        @update:value="rate = $event"
        :min="0.1"
        :max="5"
        :step="0.1"
        style="background-color: #03a9f4"
      />
    </div>
    <div class="downPayment common">
      <div class="heading">
        <p>頭金</p>
        <p>
          <strong>{{ downPayment.toLocaleString() }} </strong>万円
        </p>
      </div>

      <myRange
        :value="downPayment"
        @update:value="downPayment = $event"
        :min="0"
        :max="maxDownPayment"
        :step="10"
        style="background-color: #ff6f00"
      />
    </div>
    <div class="bonus common">
      <div class="heading">
        <p>ボーナス</p>
        <p>
          <strong>{{ bonus.toLocaleString() }} </strong>万円/回
        </p>
      </div>

      <myRange
        :value="bonus"
        @update:value="bonus = $event"
        :min="0"
        :max="maxBonus"
        :step="1"
        style="background-color: #f9a825"
      />
    </div>
    <div class="repaymentPeriod common">
      <div class="heading">
        <p>返済期間</p>
        <p>
          <strong>{{ repaymentPeriod.toLocaleString() }} </strong>年
        </p>
      </div>
      <myRange
        :value="repaymentPeriod"
        @update:value="repaymentPeriod = $event"
        :min="1"
        :max="50"
        :step="1"
        style="background-color: #f06292"
      />
    </div>
  </div>

  <!-- <v-slider max="10000" min="300" step="10"> </v-slider> -->
</template>

<script>
import myRange from "@/components/range";
import myChart from "@/components/chart";
export default {
  name: "myContents",
  components: {
    myRange,
    myChart,
  },
  data() {
    return {
      price: 3000, //物件価格
      rate: 2, // 金利
      downPayment: 500, // 頭金
      bonus: 0, // ボーナス
      repaymentPeriod: 10, // 返済期間
      // chart start

      options: {
        responsive: false,
        rotation: -1 * Math.PI,
        circumference: 1 * Math.PI,
        cutoutPercentage: 90,
        legend: { display: false },
        tooltips: { enabled: false },
      },
      // chart end
    };
  },
  computed: {
    maxDownPayment: function () {
      // 頭金の最大値を物件価格に合わせる
      return this.price;
    },
    payTotalPrice: function () {
      // 総借入金額
      return this.price - this.downPayment;
    },
    interestRate: function () {
      // 金利
      return this.rate / 100;
    },
    // 毎月返済分借入額
    bonusRatio: function () {
      return Math.pow(1 + this.interestRate / 2, this.repaymentPeriod * 2);
    },
    bonusInterestRate: function () {
      return (this.interestRate / 2) * this.bonusRatio;
    },
    bonusPaymentRate: function () {
      return this.bonusRatio - 1;
    },
    maxBonus: function () {
      // ボーナス上限支払額（支払額の50％まで）
      return Math.floor(
        (0.5 * this.payTotalPrice * this.bonusInterestRate) /
          this.bonusPaymentRate
      );
    },
    payBonus: function () {
      // ボーナス支払額
      return (this.bonus * this.bonusPaymentRate) / this.bonusInterestRate;
    },
    payPrice: function () {
      // 支払額
      return this.payTotalPrice - this.payBonus;
    },
    bonusPercentage: function () {
      // ボーナス比率
      return this.payBonus / this.payTotalPrice;
    },
    salaryRatio: function () {
      return Math.pow(1 + this.interestRate / 12, this.repaymentPeriod * 12);
    },
    salaryPaymentRate: function () {
      return this.salaryRatio - 1;
    },
    defaultSalaryPayment: function () {
      return this.payPrice * (this.interestRate / 12) * this.salaryRatio;
    },
    pricePayment: function () {
      // 月々の支払額
      return (
        Math.round((this.defaultSalaryPayment / this.salaryPaymentRate) * 10) /
        10
      );
    },
    rateTotal: function () {
      // 総支払金利額（ボーナス以外の支払額合計からボーナス支払いを引いた元金を引く）
      return (
        (this.defaultSalaryPayment / this.salaryPaymentRate) *
          this.repaymentPeriod *
          12 -
        this.payTotalPrice * (1 - this.bonusPercentage)
      );
    },
    ratePayment: function () {
      // 支払金利額（月平均）
      return (
        Math.round((this.rateTotal / (this.repaymentPeriod * 12)) * 10) / 10
      );
    },
    defaultPrice: function () {
      // 支払元金額（月平均）
      return (
        Math.round(
          (this.defaultSalaryPayment / this.salaryPaymentRate -
            this.rateTotal / (this.repaymentPeriod * 12)) *
            10
        ) / 10
      );
    },
    result: function () {
      let result = {};
      // 頭金が物件価格を超えていた場合、すべての支払額を０円に設定
      if (this.price <= this.downPayment) {
        result["monthSalaryPayment"] = 0; // 月々の元金
        result["monthRatePayment"] = 0; // 月々の金利支払い額
        result["resultPayment"] = 0; // 月々の総支払額
      } else {
        result["monthSalaryPayment"] = this.defaultPrice; // 月々の元金
        result["monthRatePayment"] = this.ratePayment; // 月々の金利支払い額
        result["resultPayment"] = Math.round(this.pricePayment * 10) / 10; // 月々の総支払額
      }

      return result;
    },
    data: function () {
      let data = {
        labels: ["月々の金利", "月々の元金"],
        datasets: [
          {
            label: "Pattern1",
            data: [
              this.result["monthRatePayment"],
              this.result["monthSalaryPayment"],
            ],
            borderColor: "transparent",
            backgroundColor: ["#2196F3", "#FF80AB"],
          },
        ],
      };

      if (
        data["datasets"][0]["data"][0] === 0 &&
        data["datasets"][0]["data"][1] === 0
      ) {
        data["datasets"][0]["data"][1] = 1;
      }

      return data;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  height: 100%;
  width: 100%;
  margin: 0 auto 0 auto;
  padding: 0.1px 10% 2rem 10%;

  background-color: #eceff1;

  .common {
    margin-top: 2rem;
    .heading {
      font-size: 1.6rem;
      display: flex;
      justify-content: space-between;

      strong {
        font-size: 2rem;
        padding: 0 1rem;
        border-bottom: 1px solid;
      }
    }
  }

  .price {
    color: #009688;
    margin-top: 14rem;
  }
  .rate {
    color: #03a9f4;
  }

  .downPayment {
    color: #ff6f00;
  }

  .bonus {
    color: #f9a825;
  }

  .repaymentPeriod {
    color: #f06292;
  }
}

.result {
  position: fixed;
  width: 100vw;
  left: 0;
  z-index: 1;
  background-color: #eceff1;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  .chart {
    display: flex;
    justify-content: center;
  }
  .resultPayment {
    position: absolute;

    color: #4caf50;
    font-size: 1.2rem;

    text-align: center;

    padding-top: 2rem;

    strong {
      font-size: 2em;
    }
  }
  .monthSalaryPayment {
    color: #ff80ab;
    font-size: 1.2rem;

    text-align: center;

    width: 8rem;
    height: 8rem;
    padding-top: 0.8rem;
    margin-right: auto;

    border: solid 1px #ff80ab;
    border-radius: 100%;
    strong {
      font-size: 1.4em;
    }
  }
  .monthRatePayment {
    color: #2196f3;
    font-size: 1.2rem;

    text-align: center;
    width: 8rem;
    height: 8rem;
    padding-top: 0.8rem;
    margin-left: auto;

    border: solid 1px #2196f3;
    border-radius: 100%;
    strong {
      font-size: 1.4em;
    }
  }
}

@media screen and(max-width:800px ) {
  .container {
    padding: 0.1px 5% 4rem 5%;
  }
  .result {
    .monthSalaryPayment {
      border: none;
      border-radius: 0;
    }
    .monthRatePayment {
      border: none;
      border-radius: 0;
    }
  }
}
</style>