<template>
  <div>
    <input type="text" id="answer" v-model="answer" autocomplete="off" />
    <button @click="sendAnswer">Answer</button>
  </div>
</template>

<script>
export default {
  props: [
    'handler'
  ],
  data: () => ({ answer: '' }),
  methods: {
    sendAnswer() {
      this.$props.handler(this.$data.answer);
    }
  }
};
</script>
