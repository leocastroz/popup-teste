<script>
export default {
  props: {
    options: {
      type: Array,
      required: true,
    },
    selectedGender: {
      type: String,
      required: true,
    },
  },
}
</script>

<template>
  <select
    :value="selectedGender"
    @change="$emit('update:selectedGender', $event.target.value)"
    class="rounded border-none my-2 bg-violet-300 py-1 px-4 ml-16 text-black"
  >
    <option
      v-for="(option, index) in options"
      :key="index"
      :value="option.type"
      class="text-black text-sm"
    >
      {{ option.label }}
    </option>
  </select>
</template>
