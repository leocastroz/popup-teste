<template>
    <button :class="buttonClasses" type="submit" :disabled="!isFormValid">
      {{ buttonText }}
    </button>
  </template>
  
  <script>
  export default {
    props: {
      isFormValid: {
        type: Boolean,
        required: true,
      },
      buttonText: {
        type: String,
        required: true,
      }
    },
    computed: {
      buttonClasses() {
        return {
          'my-2': true,
          'py-2': true,
          'px-5': true,
          'rounded': true,
          'text-xs': true,
          'font-extrabold': true,
          'text-slate-500': true,
          'bg-black bg-opacity-50': !this.isFormValid,
          'bg-emerald-200': this.isFormValid,
        }
      }
    }
  }
  </script>
  