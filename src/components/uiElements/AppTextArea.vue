<template>
  <div class="form-control" :class="{ invalid: error }">
    <label :for="id">{{ label }}</label>
    <textarea :id="id" rows="3" :value="value" @input="change"></textarea>
    <small class="danger" v-if="error">{{ error }}</small>
  </div>
</template>

<script>
export default {
  emits: ['update:value'],
  props: {
    label: String,
    error: String,
    value: String
  },
  data () {
    return {
      id: 'input-' + Math.floor(Math.random())
    }
  },
  methods: {
    change (event) {
      this.$emit('update:value', event.target.value)
    }
  }
}
</script>
