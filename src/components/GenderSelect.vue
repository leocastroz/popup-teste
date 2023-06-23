<template>
  <select :value="selectedGender" @change="$emit('update:selectedGender', $event.target.value)"
    class="rounded border-none my-2 bg-violet-300 p-1">
    <option v-for="(option, index) in options" :key="index" :value="option.type">{{ option.label }}</option>
  </select>
</template>
  
<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    },
    selectedGender: {
      type: String,
      required: true
    }
  }
};
</script>

<style scoped>
select {
  color: #000;
}

select option {
  color: #000;
  font-size: 12px;
  font-weight: bold;
}
</style>
