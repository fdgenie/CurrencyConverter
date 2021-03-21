<template>
  <div>
    <!-- Set Values that the table will be calculated by -->
    <div class="row justify-center q-gutter-x-sm">
      <AmountInput
        :amount.sync="tableRates.amount"
        :typeOfAmount="'Source'"
        class="col-auto"
      />
      <CurrencyDropdown
        :currency.sync="tableRates.currency"
        :typeOfCurrency="'Source'"
        class="col-2"
      />
      <DateOfRateCalendar :dateOfRate.sync="tableRates.date" class="col-auto" />
    </div>
    <div class="row q-ma-md justify-center">
      <CurrencyConversionsTable :rates="rates" :tableRates="tableRates" />
    </div>
  </div>
</template>

<script>
import AmountInput from "components/AmountInput.vue";
import CurrencyDropdown from "components/CurrencyDropdown.vue";
import DateOfRateCalendar from "components/DateOfRateCalendar.vue";
import CurrencyConversionsTable from "components/CurrencyConversionsTable.vue";
import { date } from "quasar";
import axios from "axios";

export default {
  name: "ConverterTable",
  components: {
    AmountInput,
    CurrencyDropdown,
    DateOfRateCalendar,
    CurrencyConversionsTable
  },
  data() {
    return {
      rates: {
        currencies: [],
        amounts: []
      },
      tableRates: {
        amount: 1,
        currency: "EUR",
        date: date.formatDate(new Date(), "YYYY-MM-DD")
      }
    };
  },
  watch: {
    tableRates: {
      handler() {
        //Dispatch when table rate is changed
        if (this.tableRates.amount >= 0) {
          this.getRatesForTable();
        }
      },
      deep: true
    }
  },
  mounted() {
    this.getRatesForTable();
  },
  methods: {
    getRatesForTable() {
      axios
        .get(
          `https://api.exchangeratesapi.io/${this.tableRates.date}?base=${this.tableRates.currency}`
        )
        .then(response => {
          this.rates.currencies = Object.keys(response.data.rates);
          this.rates.amounts = Object.values(response.data.rates);
        });
    }
  }
};
</script>
