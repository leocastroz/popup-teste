<script>
export default {
  props: {
    config: {
      type: Object,
      required: true,
    },
  },
};
</script>

<template>
  <div>
    <p class="font-black sm:text-base text-sm py-3 text-white">{{ config.formsLayout.titleForms }}</p>
    <div v-for="(field, index) in config.formsLayout.formFields" :key="index" class="flex justify-between items-center my-0 mx-auto">
      <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
      <input :type="field.type" :id="'field' + index" :value="field.value"
        @input="updateFieldValue(index, $event.target.value)" required
        class="rounded border-none my-2 bg-violet-300 p-1 text-black text-sm max-w-xs" />
    </div>
  </div>
</template>
