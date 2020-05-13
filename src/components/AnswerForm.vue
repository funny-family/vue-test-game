<template>
  <div>
    <input
      type="text"
      autocomplete="off"
      :value="value"
      @keyup.enter="sendAnswer"
      @input="updateInputValue($event.target.value)"
    />
    <button @click="sendAnswer">Answer</button>
  </div>
</template>

<script>
export default {
  props: {
    handler: Function,
    value: String
  },
  methods: {
    sendAnswer() {
      this.$props.handler(this.$props.value);
    },
    updateInputValue (value) {
      this.$emit('input', value)
    }
  }
};
</script>
