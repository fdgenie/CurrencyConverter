<template>
  <q-input
    :value="amount"
    @input="$emit('update:amount', $event)"
    :label="typeOfAmount + ' Amount'"
    type="number"
    debounce="1000"
    filled
    dense
    hide-bottom-space
    step="any"
    :rules="[notNegativeValue]"
    :disable="typeOfAmount === 'Target'"
  />
</template>

<script>
export default {
  name: "AmountInput",
  props: {
    amount: {
      type: [Number, String],
      required: true
    },
    typeOfAmount: {
      type: String,
      required: true
    }
  },
  methods: {
    notNegativeValue(val) {
      if (val < 0) {
        return "Number must be positive";
      }
    }
  }
};
</script>
