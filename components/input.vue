<template>
  <div>
    <input class="uk-input"
      :class="classes"
      :type="type" 
      :name="name"
      :value="value"
      :title="title"
      :placeholder="placeholder"
      :minlength="minlength"
      :maxlength="maxlength"
      :disabled="disabled" 
      :required="required"
      :autocomplete="autocomplete"
      @invalid="invalid"
      @keyup="keyup"
      @input="input">
    <div v-if="errorMessage" class="uk-alert-danger" uk-alert>
      {{ errorMessage }}</div>
  </div>
</template>

<script>
  export default {
    name: 'v-input',
    props: {
      type: { type: String, default: 'text' },
      name: { type: String },
      value: { type: String },
      title: { type: String },
      placeholder: { type: String },
      minlength: {
        type: Number,
        validation: (value) => value > 0
      },
      maxlength: {
        type: Number,
        validation: (value) => value > 0 && value > this.minlenth
      },
      disabled: { type: Boolean, default: false },
      required: { type: Boolean, default: false },
      autocomplete: { type: String, default: null },
      dataClassError: { type: String, default: 'uk-form-danger' }
    },
    data () {
      return {
        initialValue: null,
        hasError: false,
        errorMessage: null
      }
    },
    computed: {
      classes () {
        return {
          [this.dataClassError]: this.hasError
        }
      }
    },
    created () {
      this.initialValue = this.value
    },
    methods: {
      invalid (e) {
        this.setError(this.title)
        this.$emit('invalid', e)
      },
      input (e) {
        this.value = e.target.value
        this.clearError()

        this.$emit('input', this.value)

        if (this.value !== this.initialValue) {
          this.$emit('changed')
        }
      },
      keyup (e) {
        this.$emit('keyup')
      },
      setError (message) {
        this.hasError = true
        this.errorMessage = message
      },
      clearError () {
        this.hasError = false
        this.errorMessage = null
      }
    }
  }
</script>

<style scoped>
  input {
    padding-left: 15px;
    padding-right: 15px;
  }
  
  input + .uk-alert-danger {
    font-size: .9em;
    text-align: left;
    margin-top: 0;
    padding-top: 7px;
    padding-bottom: 7px;
  }
</style>
