<template>
  <div>
    <q-select
      :value="currency"
      @input="$emit('update:currency', $event)"
      :options="currencies"
      :label="typeOfCurrency + ' Currency'"
      dense
      filled
      hide-bottom-space
    />
  </div>
</template>

<script>
export default {
  name: "CurrencyDropdown",
  props: {
    currency: {
      type: String,
      required: true
    },
    typeOfCurrency: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      currencies: [
        "EUR",
        "CAD",
        "HKD",
        "ISK",
        "PHP",
        "DKK",
        "HUF",
        "CZK",
        "AUD",
        "RON",
        "SEK",
        "IDR",
        "INR",
        "BRL",
        "RUB",
        "HRK",
        "JPY",
        "THB",
        "CHF",
        "SGD",
        "PLN",
        "BGN",
        "TRY",
        "CNY",
        "NOK",
        "NZD",
        "ZAR",
        "USD",
        "MXN",
        "ILS",
        "GBP",
        "KRW",
        "MYR"
      ]
    };
  }
};
</script>
