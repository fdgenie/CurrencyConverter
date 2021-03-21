<template>
  <div class="q-my-md">
    <!-- Date of Rate -->
    <div class="row justify-center">
      <DateOfRateCalendar
        ref="dateOfRateCalendarRef"
        :dateOfRate.sync="dateOfRate"
      />
    </div>
    <!-- Source Rate -->
    <div class="row justify-center q-ma-md q-gutter-x-md">
      <div class="col-3" />
      <AmountInput
        :amount.sync="sourceRate.amount"
        :typeOfAmount="'Source'"
        class="col"
      />
      <CurrencyDropdown
        :currency.sync="sourceRate.currency"
        :typeOfCurrency="'Source'"
        class="col"
      />
      <div class="col-3" />
    </div>
    <div class="row justify-center q-ma-md q-gutter-x-md items-center">
      <q-btn
        @click="swapCurrencies()"
        icon="mdi-swap-vertical"
        rounded
        class="flex flex-center"
      />
    </div>
    <!-- Target Rate -->
    <div class="row justify-center q-ma-md q-gutter-x-md">
      <div class="col-3" />
      <AmountInput
        :amount.sync="targetRate.amount"
        :typeOfAmount="'Target'"
        class="col"
      />
      <CurrencyDropdown
        :currency.sync="targetRate.currency"
        :typeOfCurrency="'Target'"
        class="col"
      />
      <div class="col-3" />
    </div>
  </div>
</template>

<script>
import AmountInput from "components/AmountInput.vue";
import CurrencyDropdown from "components/CurrencyDropdown.vue";
import DateOfRateCalendar from "components/DateOfRateCalendar.vue";
import axios from "axios";
import { date } from "quasar";

export default {
  name: "MainConverter",
  components: {
    AmountInput,
    CurrencyDropdown,
    DateOfRateCalendar
  },
  data() {
    return {
      sourceRate: {
        amount: 0,
        currency: "EUR"
      },
      targetRate: {
        amount: 0,
        currency: "USD"
      },
      tempTargetCurrency: "USD",
      dateOfRate: date.formatDate(new Date(), "YYYY-MM-DD")
    };
  },
  watch: {
    sourceRate: {
      handler() {
        //Dispatch when source rate is changed
        if (this.sourceRate.amount >= 0) {
          axios
            .get(
              `https://api.exchangeratesapi.io/${this.dateOfRate}?base=${this.sourceRate.currency}&symbols=${this.targetRate.currency}`
            )
            .then(response => {
              this.targetRate.amount =
                response.data.rates[this.targetRate.currency] *
                this.sourceRate.amount;
            });
        }
      },
      deep: true
    },
    targetRate: {
      handler(val) {
        //Initialize source rate amount if target currency change
        if (val.currency !== this.tempTargetCurrency) {
          this.sourceRate.amount = 0;
          this.tempTargetCurrency = val.currency;
        }
      },
      deep: true
    },
    dateOfRate() {
      //Check if date is valid
      const validDate = this.$refs.dateOfRateCalendarRef.validate();

      //Dispatch when date of rate is changed
      if (validDate) {
        axios
          .get(
            `https://api.exchangeratesapi.io/${this.dateOfRate}?base=${this.sourceRate.currency}&symbols=${this.targetRate.currency}`
          )
          .then(response => {
            this.targetRate.amount =
              response.data.rates[this.targetRate.currency] *
              this.sourceRate.amount;
          });
      }
    }
  },
  methods: {
    swapCurrencies() {
      [this.sourceRate.currency, this.targetRate.currency] = [
        this.targetRate.currency,
        this.sourceRate.currency
      ];
    }
  }
};
</script>
