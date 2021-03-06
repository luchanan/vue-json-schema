<template>
  <div>
    <select class="uk-select" v-model="value"
      :id="id" 
      :name="name"
      :title="title"
      :class="classes"
      :multiple="multiple" 
      :required="required" 
      :disabled="disabled" 
      @invalid="invalid"
      @change="input">
      <option value="" class="placeholder">
        {{ placeholder }}</option>
      <option v-for="item of options"
        :value="item.value || item.label">
          {{ item.label }}</option>
    </select>
    <div v-if="errorMessage" class="uk-alert-danger" uk-alert>
      {{ errorMessage }}</div>
  </div>
</template>

<script>
  export default {
    name: 'v-select',
    props: {
      id: { type: String },
      name: { type: String },
      value: { type: String },
      title: { type: String },
      options: { type: Array },
      multiple: { type: Boolean },
      placeholder: { type: String },
      disabled: { type: Boolean, default: false },
      required: { type: Boolean, default: false },
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
          [this.dataClassError]: this.hasError,
          empty: this.isEmpty()
        }
      },
      currentValue () {
        return this.value
      }
    },
    created () {
      this.initialValue = this.value
    },
    methods: {
      isEmpty () {
        return !this.value || this.value.length === 0
      },
      invalid (e) {
        this.setError(this.title)
        this.$emit('invalid', e)
      },
      input (e) {
        this.clearError()

        this.$emit('input', this.value)

        if (this.value !== this.initialValue) {
          this.$emit('changed')
        }
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
  .empty, .placeholder {
    color: #999
  }
  
  select {
    padding-left: 15px;
    padding-right: 15px;
  }
  
  select.placeholder:focus {
    color: inherit
  }
  
  select + .uk-alert-danger {
    font-size: .9em;
    text-align: left;
    margin-top: 0;
    padding-top: 7px;
    padding-bottom: 7px;
  }
</style>
