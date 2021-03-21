<template>
  <div style="max-width: 300px">
    <q-input
      ref="dateOfRateRef"
      dense
      filled
      :value="dateOfRate"
      @input="$emit('update:dateOfRate', $event)"
      label="Date of Rate"
      :rules="[dateAfterToday, validYear]"
      hide-bottom-space
    >
      <template v-slot:append>
        <q-icon name="event" class="cursor-pointer">
          <q-popup-proxy
            ref="qDateProxy"
            transition-show="scale"
            transition-hide="scale"
          >
            <q-date
              :value="dateOfRate"
              @input="$emit('update:dateOfRate', $event)"
              mask="YYYY-MM-DD"
            >
              <div class="row items-center justify-end">
                <q-btn v-close-popup label="Close" color="primary" flat />
              </div>
            </q-date>
          </q-popup-proxy>
        </q-icon>
      </template>
    </q-input>
  </div>
</template>

<script>
import { date } from "quasar";

export default {
  name: "DateOfRateCalendar",
  props: {
    dateOfRate: {
      type: String,
      required: true
    }
  },
  methods: {
    dateAfterToday() {
      const today = date.formatDate(new Date(), "YYYY-MM-DD");

      if (this.dateOfRate > today) {
        return "Date can't be after today";
      }
    },
    validYear() {
      const year = this.dateOfRate.split("-")[0];

      if (year < 1999) {
        return "Year must be after 1998";
      }
    },
    validate() {
      const isDateOfRateRefValid = this.$refs.dateOfRateRef.validate();

      return isDateOfRateRefValid;
    }
  }
};
</script>
