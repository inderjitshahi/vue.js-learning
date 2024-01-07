<template>
  <div
    class="flex flex-col items-center justify-center w-[100vw] min-h-[100vh] text-center"
  >
    <!-- passing props -->
    <h3>{{ name }}</h3>
    <button class="border-2 p-2 bg-slate-300" @click="incrementCount">
      Increment Composable Count
    </button>
    <h3>{{ count }}</h3>
    <DataComponent :name="name" @sendCustomEvent="customEventHandler" />
  </div>
</template>

<script>
import { ref } from "vue";
import DataComponent from "./components/Data.vue";
import useCounter from "./composables/useCounter";

export default {
  name: "App",
  components: {
    DataComponent,
  },
  setup() {
    const name = ref("Aditya Shahi");
    const { count, incrementCount } = useCounter();

    function customEventHandler(firstName) {
      alert(`Hello ${firstName}!`);
      name.value = firstName;
    }

    // mixins or reusable logic are called composable or hooks
    return {
      name,
      customEventHandler,
      count,
      incrementCount,
    };
  },
};
</script>

<style scoped></style>
