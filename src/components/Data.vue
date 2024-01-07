<template>
  <div class="border-2 p-10 grid gap-5">
    <h1 class="text-lg font-bold">Hello {{ o_name }}!</h1>
    <h1 class="text-lg font-bold">{{ c_name }}</h1>
    <h1 class="text-lg font-bold">{{ greet }}</h1>
    <h1 class="text-lg font-bold">{{ greetReactive }}</h1>
    <h1 class="text-lg font-bold">{{ greetReactive }}</h1>
    <h1 class="text-lg font-bold">{{ computedGreet }}</h1>
    <input type="text" ref="inputRef" />
    <div>
      <button @click="changeOptions">Change Deep Option by 1</button>
      <p>{{ options.a }}</p>
    </div>
    <button class="p-2 border-2 bg-slate-300" @click="sendCustomEvent">
      Custom Event
    </button>
  </div>
</template>

<script>
import {
  ref,
  reactive,
  toRefs,
  computed,
  watch,
  onBeforeMount,
  onMounted,
} from "vue";
import _ from "lodash";
export default {
  name: "DataComponent",
  props: ["name"],
  emits: ["sendCustomEvent"],
  // option api
  data() {
    return {
      o_name: "Inderjit Shahi",
    };
  },

  // composition api
  setup(props, context) {
    // provide("name", "Inderjit Shahi"); //provide name with value inderjit shahi to all child components
    // inject("name","default value"); //inject name from parent component otherwise use default value

    const inputRef = ref(null); //ref to access dom element
    onMounted(() => {
      inputRef.value.focus();
      inputRef.value.value = "Aditya Shahi";
    });

    // ref() is used to create a reactive and mutable variable
    console.log(props);
    const c_name = ref(props.name);
    const c_age = ref("10");
    const greet = `Hello ${c_name.value}!`;

    //avoiding multiple ref() calls
    const state = reactive({
      firstName: "Inderjit",
      lastName: "Shahi",
      age: 10,
      options: {
        a: 1,
        b: 2,
      },
    });
    const greetReactive = `Hello ${state.firstName} ${state.lastName} of age ${state.age} !`;

    function sendCustomEvent() {
      context.emit("sendCustomEvent", state.firstName);
    }

    function changeOptions() {
      state.options.a++;
    }

    //methods directly having declared in setup()
    function updateName() {
      c_name.value = "Aditya Shahi";
    }

    // v-model works same with option api and composition api

    // Computed
    // multiple computed properties can be created
    const computedGreet = computed(function () {
      return `Hello ${state.firstName} ${state.lastName} of age ${state.age} ! setup computed`;
    });

    // watcher
    // watcher with ref
    watch(
      [c_name, c_age],
      (newVal, oldVal) => {
        console.log(newVal, oldVal);
        console.log(newVal[0], oldVal[0]); //newVal[0] is c_name and newVal[1] is c_age
      },
      {
        immediate: true,
      }
    );

    // watcher with reactive state
    watch(
      () => _.cloneDeep(state.options), // for deep watch
      (newVal, oldVal) => {
        console.log(newVal, oldVal);
      },
      {
        deep: true,
      }
    ); //watcher on state.firstName

    // watcher with reactive state
    watch(
      () => state.firstName, //or {...state}
      (newVal, oldVal) => {
        console.log(oldVal, newVal);
      },
      {
        deep: true,
      }
    ); //watcher on state.firstName

    // lifecycle hooks
    onBeforeMount(() => {
      console.log("onBeforeMount");
    });

    return {
      c_name,
      greet,
      greetReactive,
      c_age,
      state, //but we have to use state.firstName instead directly firstName
      ...toRefs(state), //now we can use firstName directly
      updateName,
      computedGreet,
      changeOptions,
      inputRef,
      sendCustomEvent,
    };
  },
};
</script>

<style lang="scss" scoped></style>
