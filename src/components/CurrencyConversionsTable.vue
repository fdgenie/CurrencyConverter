<template>
  <q-markup-table class="col-8" dense>
    <thead>
      <tr>
        <th class="text-left">Currency</th>
        <th class="text-left">Amount</th>
        <th class="text-left">Ratio</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(currency, index) in rates.currencies" :key="index">
        <td class="text-left">{{ currency }}</td>
        <td class="text-left">
          {{ calculateTableAmount(index) }}
        </td>
        <td class="text-left">{{ calculateTableRatio(index) }}</td>
      </tr>
    </tbody>
  </q-markup-table>
</template>

<script>
export default {
  name: "CurrencyConversionsTable",
  props: {
    rates: {
      type: Object,
      require: true
    },
    tableRates: {
      type: Object,
      require: true
    }
  },
  methods: {
    calculateTableAmount(index) {
      if (this.tableRates.amount >= 0) {
        return (this.tableRates.amount * this.rates.amounts[index]).toFixed(5);
      }
    },
    calculateTableRatio(index) {
      if (this.tableRates.amount >= 0) {
        return `1:${this.rates.amounts[index].toFixed(5)}`;
      }
    }
  }
};
</script>
