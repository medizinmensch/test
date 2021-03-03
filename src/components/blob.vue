<template>
  <!-- <button @click="state.count++">count is: {{ state.count }}</button> -->
  <button
    class="text-white font-bold py-0.5 px-1.5 rounded-lg m-1"
    v-bind:class="color"
    @click="$emit('clicked', nextBlobState(state))"
  >
    {{ content }}
  </button>
</template>

<script>
export default {
  methods: {
    nextBlobState(oldState) {
      console.log("current State", this.state);
      if (oldState == "default") return "must";
      if (oldState == "must") return "mustnot";
      return "default";
    },
  },
  computed: {
    color() {
      if (this.state == "must") return "bg-green-400";
      if (this.state == "mustnot") return "bg-red-400";
      return "bg-gray-300";
    },
  },
  props: {
    content: {
      type: String,
      required: true,
    },
    state: {
      type: String,
    },
  },
  data() {return {}},
};
</script>

<style scoped>
</style>