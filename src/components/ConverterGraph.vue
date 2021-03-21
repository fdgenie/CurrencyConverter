<template>
  <div>
    <div class="row justify-center q-gutter-x-sm">
      <DateOfRateCalendar :dateOfRate.sync="dateFrom" class="col-auto" />
      <DateOfRateCalendar :dateOfRate.sync="dateTo" class="col-auto" />
      <CurrencyDropdown
        class="col-2"
        :currency.sync="sourceCurrency"
        :typeOfCurrency="'Source'"
      />
      <CurrencyDropdown
        class="col-2"
        :currency.sync="targetCurrency"
        :typeOfCurrency="'Target'"
      />
    </div>
    <!-- <q-card class="row q-mt-md">
      <q-card-section class="col-8">
        <v-chart autoresize :options="graphConfig" style="width:100%" />
      </q-card-section>
    </q-card> -->
  </div>
</template>

<script>
import VChart from "vue-echarts";
import "echarts/lib/chart/line";
import axios from "Axios";
import CurrencyDropdown from "components/CurrencyDropdown.vue";
import DateOfRateCalendar from "components/DateOfRateCalendar.vue";
import { date } from "quasar";

export default {
  name: "ConverterGraph",
  components: {
    CurrencyDropdown,
    DateOfRateCalendar,
    VChart
  },
  data() {
    return {
      sourceCurrency: "EUR",
      targetCurrency: "USD",
      dateFrom: date.formatDate(new Date(), "YYYY-MM-DD"),
      dateTo: date.formatDate(new Date(), "YYYY-MM-DD")
    };
  },
  mounted() {
    axios.get(
      `https://api.exchangeratesapi.io/history?start_at=${this.dateFrom}&end_at=${this.dateTo}&base=${this.sourceCurrency}&symbols=${this.targetCurrency}`
    );
  }
};
</script>
