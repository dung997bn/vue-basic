<template>
  <div class="my-app"></div>
  <!-- <CounterView /> -->
  <!-- <counter-view></counter-view> -->
  <myComponent v-bind:counter="counter"></myComponent>
  <counter-controller @on-increment="handleIncrement($event)"></counter-controller>
  <!-- <counter-controller @on-increment="handleIncrement"></counter-controller> -->
</template>

<script>
import CounterView from "./components/CounterView";
import CounterController from "./components/CounterController";
export default {
  components: {
    myComponent: CounterView,
    CounterController,
  },
  data() {
    return {
      counter: 10,
    };
  },
  methods: {
    handleIncrement(inc) {
      this.counter += inc;
    },
  },
};
</script>

<style>
</style>
