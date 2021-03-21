<template>
  <q-page class="bg-grey-2">
    <p class="flex flex-center q-py-md text-weight-bold text-h5">
      Simple Currency Conversion
    </p>
    <ConverterSimple />
    <p class="flex flex-center q-py-md text-weight-bold text-h5">
      Table of Currency Conversions
    </p>
    <ConverterTable />
    <!-- <p class="flex flex-center q-py-md text-weight-bold text-h5">
      Graph of Historical Rate Evolution
    </p>
    <ConverterGraph /> -->
  </q-page>
</template>

<script>
import ConverterSimple from "components/ConverterSimple";
import ConverterTable from "components/ConverterTable";
// import ConverterGraph from "components/ConverterGraph";

export default {
  name: "CurencyConverterPage",
  components: {
    ConverterSimple,
    ConverterTable
    // ConverterGraph
  }
};
</script>
